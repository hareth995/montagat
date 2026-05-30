<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>DATHIUN Generator Pro</title>
    <style>
        body { background: #121212; color: #fff; font-family: sans-serif; padding: 20px; }
        .box { max-width: 500px; margin: auto; background: #1e1e1e; padding: 20px; border-radius: 15px; border: 2px solid #885cfb; }
        input, select, button { width: 100%; padding: 12px; margin: 8px 0; border-radius: 8px; border: none; box-sizing: border-box; }
        button { background: #885cfb; color: #fff; font-weight: bold; cursor: pointer; transition: 0.3s; }
        button:hover { background: #36e5f5; color: #000; }
        #output { width: 100%; height: 150px; margin-top: 10px; background: #000; color: #36e5f5; padding: 10px; border: 1px solid #444; display: none; }
    </style>
</head>
<body>

<div class="box">
    <h2>مولد كروت  (3D VERSE)</h2>
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

            // تم إضافة حاوية الـ product-grid هنا لضمان الترتيب الأفقي وظهور الإطارات بشكل صحيح دائماً
            const code = `<div class="product-grid">
    <div class="fire-card" data-category="${cat}">
        <img src="${dataUrl}" loading="lazy">
        <h3>${name}</h3>
        <p>${price} د.ع</p>
        <a href="https://wa.me/9647710705445?text=اريد طلب ${name} بسعر ${price} د.ع" class="fire-btn">اطلبه الآن من الواتساب 💬</a>
    </div>
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
    /* الحاوية الكبيرة التي تمنع الترتيب العمودي العشوائي */
    .product-grid { 
        display: flex; 
        flex-wrap: wrap; 
        gap: 20px; 
        padding: 20px; 
        justify-content: center; 
        width: 100%;
        box-sizing: border-box;
    }
    
    /* الكرت مستطيل طولي بالألوان الجديدة وإطار واضح وثابت */
    .fire-card { 
        width: calc(25% - 20px); 
        min-width: 220px; 
        max-width: 280px;
        aspect-ratio: 4/6; 
        background: #1a1a1a; /* خلفية داكنة لتبرز الإطار الملون */
        border: 2px solid #885cfb; /* إطار بنفسجي أساسي واضح */
        border-radius: 15px; 
        padding: 15px; 
        text-align: center; 
        box-shadow: 0 4px 15px rgba(136, 92, 251, 0.2); 
        transition: all 0.4s ease; 
        display: flex; 
        flex-direction: column; 
        justify-content: space-between;
        box-sizing: border-box;
    }
    
    /* تأثير التمرير المضيء بالسمائي */
    .fire-card:hover { 
        transform: translateY(-5px); 
        border-color: #36e5f5; 
        box-shadow: 0 0 25px rgba(54, 229, 245, 0.5); 
    }
    
    /* الصورة المربعة داخل المستطيل */
    .fire-card img { width: 100%; aspect-ratio: 1/1; object-fit: cover; border-radius: 10px; }
    
    .fire-card h3 { color: #fff; font-size: 18px; margin: 10px 0; }
    .fire-card p { color: #36e5f5; font-weight: bold; font-size: 16px; margin-bottom: 10px; }
    
    /* زر الواتساب بالتدرج اللوني الجديد */
    .fire-btn { 
        display: block; 
        background: linear-gradient(135deg, #885cfb, #36e5f5); 
        color: #fff; 
        padding: 12px; 
        border-radius: 8px; 
        text-decoration: none; 
        font-weight: bold; 
        font-size: 14px; 
        margin-top: auto; 
        transition: 0.3s; 
    }
    .fire-btn:hover { opacity: 0.9; box-shadow: 0 4px 10px rgba(54, 229, 245, 0.3); }
</style>
</body>
</html>
