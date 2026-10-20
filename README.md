
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>mohmmed_mony | أسعار العملات</title>
<style>
body{font-family:Arial,sans-serif;margin:0;background:#f1f5f9;color:#172b3a}
header{background:#087f70;color:white;padding:25px 15px;text-align:center}
main{max-width:700px;margin:20px auto;padding:0 12px}
.box{background:white;padding:16px;border-radius:14px;margin-bottom:15px}
select,input,button{padding:12px;border-radius:8px;border:1px solid #ddd;font-size:16px}
select{width:100%}
table{width:100%;border-collapse:collapse;margin-top:15px}
th,td{padding:12px 5px;text-align:center;border-bottom:1px solid #eee}
th{background:#e6f4f1}
input{width:90px;max-width:100%;margin:4px}
button{background:#087f70;color:white;border:0;cursor:pointer}
.note{font-size:13px;color:#a16207}
</style>
</head>
<body>
<header>
<h1>mohmmed_mony</h1>
<p>أسعار العملات مقابل الريال اليمني</p>
</header>
<main>
<div class="box">
<label for="city">اختر السوق:</label>
<select id="city" onchange="showRates()">
<option value="aden">عدن</option>
<option value="sanaa">صنعاء</option>
</select>
<table>
<thead><tr><th>العملة</th><th>شراء</th><th>بيع</th></tr></thead>
<tbody id="rates"></tbody>
</table>
<p class="note">تنبيه: الأسعار تحتاج إلى تحديث من مصدر موثوق، وليست أسعارًا مباشرة.</p>
</div>

<div class="box">
<h3>تحديث الأسعار</h3>
<p>أدخل الأسعار الصحيحة لكل سوق ثم اضغط حفظ.</p>
<div id="editor"></div>
<button onclick="saveRates()">حفظ الأسعار</button>
<p id="msg"></p>
</div>
</main>
<script>
const currencies=[
{id:"usd",name:"الدولار الأمريكي"},
{id:"sar",name:"الريال السعودي"},
{id:"aed",name:"الدرهم الإماراتي"}
];
let data;
try{
data=JSON.parse(localStorage.getItem("mohmmed_rates"))||{};
}catch(e){data={};}
function getRate(city,id){
return (data[city]&&data[city][id])||{buy:"",sell:""};
}
function showRates(){
let city=document.getElementById("city").value;
document.getElementById("rates").innerHTML=currencies.map(c=>{
let r=getRate(city,c.id);
return `<tr><td>${c.name}</td><td>${r.buy||"غير مدخل"}</td><td>${r.sell||"غير مدخل"}</td></tr>`;
}).join("");
document.getElementById("editor").innerHTML=currencies.map(c=>{
let r=getRate(city,c.id);
return `<div><b>${c.name}</b><br>
شراء <input type="number" min="0" id="${c.id}buy" value="${r.buy}">
بيع <input type="number" min="0" id="${c.id}sell" value="${r.sell}"></div><hr>`;
}).join("");
}
function saveRates(){
let city=document.getElementById("city").value;
if(!data[city])data[city]={};
currencies.forEach(c=>{
data[city][c.id]={
buy:document.getElementById(c.id+"buy").value,
sell:document.getElementById(c.id+"sell").value
};
});
try{
localStorage.setItem("mohmmed_rates",JSON.stringify(data));
document.getElementById("msg").textContent="تم حفظ الأسعار على هذا الجهاز.";
showRates();
}catch(e){
document.getElementById("msg").textContent="تعذر الحفظ في المتصفح.";
}
}
showRates();
</script>
</body>
</html>
