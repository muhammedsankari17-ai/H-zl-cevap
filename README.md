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
let category=document.getElementById("category").value;
if(!title||!message){alert("Başlık ve mesajı doldur.");return}
let obj={title,category,message};
if(editing<0)messages.unshift(obj);else messages[editing]=obj;
persist();resetForm();render();
}
function resetForm(){
editing=-1;
document.getElementById("title").value="";
document.getElementById("message").value="";
document.getElementById("category").value="Fiyat";
document.getElementById("formTitle").textContent="Yeni hazır mesaj";
}
function render(){
let q=document.getElementById("search").value.toLocaleLowerCase("tr");
let list=document.getElementById("list");
list.innerHTML="";
messages.forEach((m,i)=>{
if(!(m.title+" "+m.category+" "+m.message).toLocaleLowerCase("tr").includes(q))return;
let item=document.createElement("div");item.className="item";
let h=document.createElement("h3");h.textContent=m.title;
let tag=document.createElement("small");tag.textContent=m.category;
let p=document.createElement("p");p.textContent=m.message;
let copy=document.createElement("button");copy.textContent="Kopyala";
copy.onclick=async()=>{
try{await navigator.clipboard.writeText(m.message);alert("Mesaj kopyalandı!")}
catch(e){let t=document.createElement("textarea");t.value=m.message;document.body.appendChild(t);t.select();let ok=document.execCommand("copy");t.remove();if(ok)alert("Mesaj kopyalandı!");else prompt("Mesajı kopyala:",m.message)}
};
let edit=document.createElement("button");edit.className="small";edit.textContent="Düzenle";
edit.onclick=()=>{editing=i;document.getElementById("title").value=m.title;document.getElementById("category").value=m.category;document.getElementById("message").value=m.message;document.getElementById("formTitle").textContent="Mesajı düzenle";window.scrollTo(0,0)};
let del=document.createElement("button");del.className="small";del.textContent="Sil";
del.onclick=()=>{if(confirm("Bu mesaj silinsin mi?")){messages.splice(i,1);persist();render()}};
item.append(h,tag,p,copy,edit,del);list.appendChild(item);
});
}
render();
</script>
</body>
</html>

