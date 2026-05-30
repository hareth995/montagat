<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>Eng.HARETH Generator Pro</title>
    <style>
        body { background: #121212; color: #fff; font-family: sans-serif; padding: 20px; }
        .box { max-width: 500px; margin: auto; background: #1e1e1e; padding: 20px; border-radius: 15px; border: 2px solid #885cfb; }
        input, select, button { width: 100%; padding: 12px; margin: 8px 0; border-radius: 8px; border: none; box-sizing: border-box; }
        button { background: #885cfb; color: #fff; font-weight: bold; cursor: pointer; transition: 0.3s; }
        button:hover { background: #36e5f5; color: #121212; }
        #output { width: 100%; height: 150px; margin-top: 10px; background: #000; color: #36e5f5; padding: 10px; border: 1px solid #444; display: none; }
    </style>
</head>
<body>

<div class="box">
    <h2>مولد كروت (3D VERSE)</h2>
    <input type="file" id="imgInput" accept="image/*">
    <input type="text" id="nameInput" placeholder="اسم المنتج">
    <input type="text" id="priceInput" placeholder="السعر">
    <select id="catInput">
        <option value="weapons">أسلحة</option>
        <option value="stands">ستاندات</option>
        <option value="masks">أقنعة</option>
        <option value="keycaps">كيكابس</option>
        <option value="figures">مجسمات</option>
        <option value="stickers">ستيكرات</option>
        <option value="keychains">ميداليات</option>
        <option value="others">أخرى</option>
    </select>
    <button onclick="generate()">توليد الكود</button>
    <textarea id="output" readonly></textarea>
    <button id="copyBtn" style="display:none;" onclick="copyCode()">📋 نسخ الكود</button>
</div>

<script>
function generate() {
    const file = document.getElementById('imgInput').files[0];
    if(!file) return alert("اختر صورة!");
    
    const reader = new FileReader();
    reader.onload = function(e) {
        const img = new Image();
        img.onload = function() {
            // معالجة الصورة لتكون مربعة ومقصوفة
            const canvas = document.createElement('canvas');
            canvas.width = 600; canvas.height = 600;
            const ctx = canvas.getContext('2d');
            const min = Math.min(img.width, img.height);
            ctx.drawImage(img, (img.width-min)/2, (img.height-min)/2, min, min, 0, 0, 600, 600);
            const dataUrl = canvas.toDataURL('image/jpeg', 0.8);

            let price = document.getElementById('priceInput').value.replace(/[٠-٩]/g, d => "٠١٢٣٤٥٦٧٨٩".indexOf(d));
            const name = document.getElementById('nameInput').value;
            const cat = document.getElementById('catInput').value;

            const code = `<div class="fire-card" data-category="${cat}">
    <img src="${dataUrl}" loading="lazy">
    <h3>${name}</h3>
    <p>${price} د.ع</p>
    <a href="https://wa.me/9647773113888?text=اريد طلب ${name} بسعر ${price} د.ع" class="fire-btn">اطلبه الآن من الواتساب 💬</a>
</div>`;
            document.getElementById('output').style.display = 'block';
            document.getElementById('output').value = code;
            document.getElementById('copyBtn').style.display = 'block';
        };
        img.src = e.target.result;
    };
    reader.readAsDataURL(file);
}

function copyCode() {
    document.getElementById('output').select();
    document.execCommand('copy');
    alert("تم النسخ!");
}
</script>

<style id="fire-style">
    /* الحاوية الكبيرة */
    .product-grid { display: flex; flex-wrap: wrap; gap: 20px; padding: 20px; justify-content: center; width: 100%; box-sizing: border-box; }
    
    /* الكرت مستطيل طولي بالألوان الجديدة */
    .fire-card { 
        width: calc(25% - 20px); 
        min-width: 200px; 
        aspect-ratio: 4/6; 
        background: #000; 
        border: 2px solid #885cfb; /* إطار بنفسجي ثابت وواضح */
        border-radius: 15px; 
        padding: 15px; 
        text-align: center; 
        box-shadow: 0 4px 12px rgba(136, 92, 251, 0.2); /* توهج بنفسجي خفيف خلف الكرت */
        transition: 0.4s ease-in-out; 
        display: flex; flex-direction: column; justify-content: space-between;
        box-sizing: border-box;
    }
    
    /* عند تمرير الماوس يتحول الإطار والتوهج بالكامل للسمائي المضيء */
    .fire-card:hover { 
        transform: scale(1.05); 
        border-color: #36e5f5; 
        box-shadow: 0 0 25px rgba(54, 229, 245, 0.6); 
    }
    
    /* الصورة المربعة داخل المستطيل */
    .fire-card img { width: 100%; aspect-ratio: 1/1; object-fit: cover; border-radius: 10px; }
    
    .fire-card h3 { color: #fff; font-size: 18px; margin: 10px 0; }
    .fire-card p { color: #36e5f5; font-weight: bold; font-size: 16px; margin-bottom: 10px; } /* تعديل لون السعر للسمائي ليكون مريح للعين بارز */
    
    /* زر الواتساب بتدرج انسيابي ممتاز من السمائي للبنفسجي ونص أبيض واضح */
    .fire-btn { 
        display: block; 
        background: linear-gradient(to right, #36e5f5, #885cfb); 
        color: #fff; 
        padding: 10px; 
        border-radius: 5px; 
        text-decoration: none; 
        font-weight: bold; 
        font-size: 14px; 
        margin-top: auto;
        transition: 0.3s;
    }
    .fire-btn:hover { opacity: 0.9; box-shadow: 0 2px 8px rgba(54, 229, 245, 0.4); }
</style>
</body>
</html>
