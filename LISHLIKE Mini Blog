<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>LISHLIKE - Mini Blog</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      margin: 0;
      padding: 0;
    }
    header {
      background: #4CAF50;
      color: white;
      padding: 15px;
      text-align: center;
      font-size: 24px;
      font-weight: bold;
      position: relative;
    }
    #followBtn {
      position: absolute;
      right: 20px;
      top: 15px;
      padding: 8px 16px;
      background: white;
      color: #4CAF50;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 14px;
    }
    #followBtn.following {
      background: #2e7d32;
      color: white;
    }
    .container {
      width: 90%;
      max-width: 900px;
      margin: 20px auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    .upload-section {
      margin-bottom: 20px;
    }
    .upload-section input, .upload-section textarea, .upload-section button {
      width: 100%;
      margin: 5px 0;
      padding: 10px;
      border-radius: 5px;
      border: 1px solid #ddd;
    }
    .upload-section button {
      background: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    .post {
      border-bottom: 1px solid #ddd;
      padding: 15px 0;
    }
    .post h3 {
      margin: 0 0 10px;
      color: #333;
    }
    .post img, .post video {
      max-width: 100%;
      border-radius: 10px;
    }
    .actions {
      margin-top: 10px;
    }
    .actions button {
      margin-right: 10px;
      padding: 5px 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .likeBtn {
      background: #ff5722;
      color: white;
    }
    .comment-section {
      margin-top: 10px;
      padding-left: 10px;
    }
    .comment-section input {
      width: 70%;
      padding: 5px;
      margin-right: 5px;
    }
    .comment {
      background: #f0f0f0;
      padding: 5px 10px;
      margin: 5px 0;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <header>
    LISHLIKE - Mini Blog
    <button id="followBtn">Follow</button>
  </header>

  <div class="container">
    <div class="upload-section">
      <h2>Create Post</h2>
      <input type="text" id="title" placeholder="Enter Title">
      <textarea id="content" rows="4" placeholder="Write your article here..."></textarea>
      <input type="file" id="media" accept="image/*,video/*">
      <button onclick="addPost()">Upload</button>
    </div>

    <div id="posts">
      <!-- Uploaded posts will appear here -->
    </div>
  </div>

  <script>
    let posts = JSON.parse(localStorage.getItem("posts")) || [];
    let following = localStorage.getItem("following") === "true";

    const followBtn = document.getElementById("followBtn");
    updateFollowBtn();

    followBtn.addEventListener("click", () => {
      following = !following;
      localStorage.setItem("following", following);
      updateFollowBtn();
    });

    function updateFollowBtn() {
      if (following) {
        followBtn.textContent = "Following";
        followBtn.classList.add("following");
      } else {
        followBtn.textContent = "Follow";
        followBtn.classList.remove("following");
      }
    }

    function addPost() {
      const title = document.getElementById('title').value;
      const content = document.getElementById('content').value;
      const media = document.getElementById('media').files[0];

      if (!title && !content && !media) {
        alert("Please add some content!");
        return;
      }

      let post = {
        id: Date.now(),
        title,
        content,
        media: media ? { name: media.name, type: media.type, url: URL.createObjectURL(media) } : null,
        likes: 0,
        comments: []
      };

      posts.unshift(post);
      localStorage.setItem("posts", JSON.stringify(posts));
      renderPosts();

      document.getElementById('title').value = "";
      document.getElementById('content').value = "";
      document.getElementById('media').value = "";
    }

    function likePost(id) {
      posts = posts.map(p => {
        if (p.id === id) p.likes++;
        return p;
      });
      localStorage.setItem("posts", JSON.stringify(posts));
      renderPosts();
    }

    function addComment(id, input) {
      const text = input.value.trim();
      if (!text) return;
      posts = posts.map(p => {
        if (p.id === id) p.comments.push(text);
        return p;
      });
      localStorage.setItem("posts", JSON.stringify(posts));
      renderPosts();
    }

    function renderPosts() {
      const postsDiv = document.getElementById("posts");
      postsDiv.innerHTML = "";
      posts.forEach(post => {
        let postDiv = document.createElement("div");
        postDiv.classList.add("post");

        let titleEl = document.createElement("h3");
        titleEl.innerText = post.title;
        postDiv.appendChild(titleEl);

        if (post.content) {
          let contentEl = document.createElement("p");
          contentEl.innerText = post.content;
          postDiv.appendChild(contentEl);
        }

        if (post.media) {
          if (post.media.type.startsWith("image/")) {
            let img = document.createElement("img");
            img.src = post.media.url;
            postDiv.appendChild(img);
          } else if (post.media.type.startsWith("video/")) {
            let video = document.createElement("video");
            video.src = post.media.url;
            video.controls = true;
            postDiv.appendChild(video);
          }
        }

        let actions = document.createElement("div");
        actions.classList.add("actions");
        let likeBtn = document.createElement("button");
        likeBtn.classList.add("likeBtn");
        likeBtn.innerText = `❤️ Like (${post.likes})`;
        likeBtn.onclick = () => likePost(post.id);
        actions.appendChild(likeBtn);
        postDiv.appendChild(actions);

        let commentSection = document.createElement("div");
        commentSection.classList.add("comment-section");
        let commentInput = document.createElement("input");
        commentInput.placeholder = "Write a comment...";
        let commentBtn = document.createElement("button");
        commentBtn.innerText = "Add";
        commentBtn.onclick = () => addComment(post.id, commentInput);
        commentSection.appendChild(commentInput);
        commentSection.appendChild(commentBtn);

        post.comments.forEach(c => {
          let cEl = document.createElement("div");
          cEl.classList.add("comment");
          cEl.innerText = c;
          commentSection.appendChild(cEl);
        });

        postDiv.appendChild(commentSection);

        postsDiv.appendChild(postDiv);
      });
    }

    renderPosts();
  </script>
</body>
</html>
