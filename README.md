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
