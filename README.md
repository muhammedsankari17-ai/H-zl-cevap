# H-zl-cevap
Hızlı cevap.web
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>HızlıCevap</title>
<style>
body{font-family:Arial,sans-serif;background:#f1f5f9;margin:0;color:#172033}
header{background:#172554;color:white;padding:24px;border-radius:0 0 20px 20px}
main{padding:16px;max-width:650px;margin:auto}
section,.item{background:white;padding:16px;border-radius:14px;margin:12px 0}
input,textarea,select{box-sizing:border-box;width:100%;padding:12px;margin:7px 0;border:1px solid #cbd5e1;border-radius:9px;font-size:16px}
textarea{min-height:110px}
button{padding:11px 14px;border:0;border-radius:9px;background:#2563eb;color:white;font-weight:bold;margin:4px;cursor:pointer}
.item p{white-space:pre-wrap;overflow-wrap:anywhere;line-height:1.5}
.small{background:#64748b}
</style>
</head>
<body>
<header>
<h1>✓ HızlıCevap</h1>
<p>Hazır mesajların, tek dokunuş uzağında.</p>
</header>
<main>
<section>
<h3 id="formTitle">Yeni hazır mesaj</h3>
<input id="title" placeholder="Mesaj başlığı">
<select id="category">
<option>Fiyat</option><option>Sipariş</option>
<option>Müşteri hizmetleri</option><option>Randevu</option>
<option>Teşekkür</option><option>Diğer</option>
</select>
<textarea id="message" placeholder="Mesajını buraya yaz..."></textarea>
<button onclick="save()">Mesajı kaydet</button>
<button class="small" onclick="resetForm()">Temizle</button>
</section>
<section>
<h3>Kayıtlı mesajlar</h3>
<input id="search" placeholder="Mesajlarda ara..." oninput="render()">
<div id="list"></div>
</section>
</main>
<script>
let messages=[];
let editing=-1;
try{messages=JSON.parse(localStorage.getItem("hc-messages")||"[]")}catch(e){}
if(!messages.length)messages.push({
title:"Fiyat bilgisi",category:"Fiyat",
message:"Merhaba, fiyatlarımız ürün veya hizmet türüne göre değişiklik göstermektedir. İlgilendiğiniz ürün veya hizmeti belirtirseniz güncel fiyat bilgimizi memnuniyetle iletebiliriz. İlginiz için teşekkür ederiz."
});
function persist(){
try{localStorage.setItem("hc-messages",JSON.stringify(messages))}
catch(e){alert("Tarayıcı kayıt yapamadı.")}
}
function save(){
let title=document.getElementById("title").value.trim();
let message=document.getElementById("message").value.trim();