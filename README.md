<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>مذكرات جعفر</title>
  <style>
    body {font-family:"Cairo",sans-serif;background:linear-gradient(135deg,#6a11cb,#2575fc);margin:0;padding:0;}
    header {text-align:center;color:#fff;padding:20px;font-size:26px;}
    .slider{position:relative;width:90%;max-width:800px;margin:30px auto;background:#fff;border-radius:16px;
      box-shadow:0 8px 20px rgba(0,0,0,0.2);overflow:hidden;text-align:center;padding:40px 20px;min-height:250px;}
    .slide{display:none;font-size:26px;font-family:"Amiri",serif;line-height:1.8;color:#333;position:relative;}
    .active{display:block;}
    .date{font-size:14px;color:#777;margin-top:15px;font-style:italic;}
    .nav{position:absolute;top:50%;transform:translateY(-50%);font-size:32px;color:#2575fc;background:rgba(255,255,255,0.9);
      border-radius:50%;padding:12px;cursor:pointer;}
    .prev{left:15px;} .next{right:15px;}
    .slide img{max-width:100%;border-radius:12px;margin-top:15px;}
    .slide a{display:inline-block;margin-top:15px;color:#2575fc;text-decoration:none;font-weight:bold;}
    .publish-btn{margin-top:20px;background:linear-gradient(135deg,#2575fc,#6a11cb);color:#fff;border:none;border-radius:10px;
      padding:12px 20px;font-size:18px;cursor:pointer;}
    .delete-btn,.edit-btn{position:absolute;top:10px;border:none;border-radius:6px;padding:6px 10px;cursor:pointer;font-size:14px;}
    .delete-btn{left:10px;background:#ff6b6b;color:#fff;}
    .edit-btn{right:10px;background:#feca57;color:#333;}
    .modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.6);
      justify-content:center;align-items:center;}
    .modal-content{background:#fff;padding:20px;border-radius:10px;width:80%;max-width:400px;}
    .modal-content textarea,.modal-content input{width:100%;margin:8px 0;padding:10px;border:1px solid #ccc;border-radius:6px;}
    .close{float:right;cursor:pointer;color:#2575fc;font-weight:bold;}
    .owner-only{display:none;} /* الأزرار مخفية افتراضياً */
  </style>
</head>
<body>
  <header>✨ مذكرات جعفر ✨</header>

  <div class="slider" id="slider">
    <!-- يبدأ الموقع فارغ، المنشورات تظهر هنا بعد النشر -->
    <div class="nav prev" onclick="changeSlide(-1)">&#10094;</div>
    <div class="nav next" onclick="changeSlide(1)">&#10095;</div>
    <button class="publish-btn owner-only" onclick="openModal()">➕ نشر جديد</button>
  </div>

  <!-- نافذة النشر -->
  <div class="modal" id="modal">
    <div class="modal-content">
      <span class="close" onclick="closeModal()">×</span>
      <textarea id="postText" placeholder="اكتب اقتباسك..."></textarea>
      <input type="file" id="postImage" accept="image/*">
      <input type="text" id="postLink" placeholder="رابط خارجي (اختياري)">
      <button onclick="addPost()">نشر</button>
    </div>
  </div>

  <script>
    let current=0;let slides=document.querySelectorAll(".slide");
    function changeSlide(d){
      if(slides.length===0)return;
      slides[current].classList.remove("active");
      current=(current+d+slides.length)%slides.length;
      slides[current].classList.add("active");
    }
    function openModal(){document.getElementById("modal").style.display="flex";}
    function closeModal(){document.getElementById("modal").style.display="none";}
    function addPost(){
      const text=document.getElementById("postText").value;
      const image=document.getElementById("postImage").files[0];
      const link=document.getElementById("postLink").value;
      const date=new Date().toLocaleString("ar-EG");
      if(!text&&!image&&!link)return;
      const slider=document.getElementById("slider");const newSlide=document.createElement("div");newSlide.classList.add("slide");
      let content="";if(text)content+=`<div>${text}</div>`;
      if(image){
        const reader=new FileReader();
        reader.onload=e=>{
          content+=`<img src="${e.target.result}">`;
          if(link)content+=`<a href="${link}" target="_blank">🔗 ${link}</a>`;
          content+=`<div class="date">${date}</div><button class="delete-btn owner-only" onclick="deletePost(this)">🗑️ حذف</button><button class="edit-btn owner-only" onclick="editPost(this)">✏️ تعديل</button>`;
          newSlide.innerHTML=content;
          slider.insertBefore(newSlide,document.querySelector(".nav.next"));
          slides=document.querySelectorAll(".slide");
          slides.forEach(s=>s.classList.remove("active"));
          current=slides.length-1;
          newSlide.classList.add("active");
        };
        reader.readAsDataURL(image);
      } else {
        if(link)content+=`<a href="${link}" target="_blank">🔗 ${link}</a>`;
        content+=`<div class="date">${date}</div><button class="delete-btn owner-only" onclick="deletePost(this)">🗑️ حذف</button><button class="edit-btn owner-only" onclick="editPost(this)">✏️ تعديل</button>`;
        newSlide.innerHTML=content;
        slider.insertBefore(newSlide,document.querySelector(".nav.next"));
        slides=document.querySelectorAll(".slide");
        slides.forEach(s=>s.classList.remove("active"));
        current=slides.length-1;
        newSlide.classList.add("active");
      }
      document.getElementById("postText").value="";
      document.getElementById("postImage").value="";
      document.getElementById("postLink").value="";
      closeModal();
    }
    function deletePost(btn){
      btn.parentElement.remove();
      slides=document.querySelectorAll(".slide");
      current=0;if(slides.length>0)slides[current].classList.add("active");
    }
    function editPost(btn){
      const slide=btn.parentElement;
      const newText=prompt("غيّر النص أو الرابط:", slide.querySelector("div").innerText);
      if(newText){slide.querySelector("div").innerText=newText;}
    }
    // تفعيل الأزرار للمالك فقط
    const isOwner=true; // إذا خليتها false تختفي الأزرار
    if(isOwner){
      document.querySelectorAll(".owner-only").forEach(btn=>btn.style.display="inline-block");
    }
  </script>
</body>
</html>
