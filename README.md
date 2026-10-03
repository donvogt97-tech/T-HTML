<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>میرا بل - Invoice Maker</title>

<style>
*{box-sizing:border-box}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:#f2f2f2;
  color:#222;
}
.container{
  max-width:900px;
  margin:20px auto;
  padding:15px;
}
.card{
  background:white;
  padding:20px;
  border-radius:15px;
  box-shadow:0 3px 15px #0002;
}
h1{text-align:center;margin-top:0}
input{
  width:100%;
  padding:12px;
  margin:6px 0;
  border:1px solid #ccc;
  border-radius:8px;
  font-size:16px;
}
button{
  border:0;
  padding:12px 18px;
  margin:5px;
  border-radius:8px;
  cursor:pointer;
  font-size:16px;
}
.add{background:#168b45;color:white}
.print{background:#222;color:white}
.clear{background:#d33;color:white}
table{
  width:100%;
  border-collapse:collapse;
  margin-top:15px;
}
th,td{
  border:1px solid #ddd;
  padding:10px;
  text-align:center;
}
th{background:#eee}
.total{
  text-align:left;
  font-size:22px;
  font-weight:bold;
  margin-top:20px;
}
.remove{
  background:#d33;
  color:white;
  padding:6px 10px;
}
.header{
  display:flex;
  justify-content:space-between;
  gap:15px;
}
.header div{width:50%}

@media(max-width:600px){
  .header{display:block}
  .header div{width:100%}
  table{font-size:13px}
  th,td{padding:6px}
}

@media print{
  body{background:white}
  .no-print{display:none!important}
  .card{box-shadow:none}
}
</style>
</head>

<body>

<div class="container">
<div class="card">

<h1>🧾 میرا بل</h1>

<div class="header">
<div>
<label>دکان / کاروبار کا نام</label>
<input id="shop" placeholder="مثلاً میری کریانہ اسٹور">
</div>

<div>
<label>انوائس نمبر</label>
<input id="invoice" value="INV-001">
</div>
</div>

<label>گاہک کا نام</label>
<input id="customer" placeholder="گاہک کا نام">

<label>تاریخ</label>
<input id="date" type="date">

<div class="no-print">
<h3>سامان شامل کریں</h3>

<input id="product" placeholder="سامان کا نام">

<input id="qty" type="number" placeholder="تعداد" value="1" min="1">

<input id="price" type="number" placeholder="قیمت" min="0">

<button class="add" onclick="addItem()">+ سامان شامل کریں</button>
</div>

<table>
<thead>
<tr>
<th>سامان</th>
<th>تعداد</th>
<th>قیمت</th>
<th>کل</th>
<th class="no-print">حذف</th>
</tr>
</thead>

<tbody id="items"></tbody>
</table>

<div class="total">
کل رقم: Rs <span id="total">0</span>
</div>

<div class="no-print" style="text-align:center;margin-top:20px">
<button class="print" onclick="window.print()">🖨️ بل پرنٹ / PDF</button>
<button class="clear" onclick="clearBill()">🗑️ نیا بل</button>
</div>

</div>
</div>

<script>

let items = [];

document.getElementById("date").value =
new Date().toISOString().split("T")[0];

function addItem(){

  const product =
  document.getElementById("product").value.trim();

  const qty =
  Number(document.getElementById("qty").value);

  const price =
  Number(document.getElementById("price").value);

  if(!product || qty <= 0 || price < 0){
    alert("براہ کرم سامان، تعداد اور قیمت درج کریں۔");
    return;
  }

  items.push({
    product: product,
    qty: qty,
    price: price
  });

  document.getElementById("product").value="";
  document.getElementById("qty").value=1;
  document.getElementById("price").value="";

  renderItems();
}

function renderItems(){

  const tbody =
  document.getElementById("items");

  tbody.innerHTML="";

  let total=0;

  items.forEach((item,index)=>{

    const subtotal =
    item.qty * item.price;

    total += subtotal;

    const row=document.createElement("tr");

    row.innerHTML=`
      <td>${item.product}</td>
      <td>${item.qty}</td>
      <td>Rs ${item.price}</td>
      <td>Rs ${subtotal}</td>
      <td class="no-print">
        <button class="remove"
        onclick="removeItem(${index})">X</button>
      </td>
    `;

    tbody.appendChild(row);
  });

  document.getElementById("total").textContent=total;
}

function removeItem(index){

  items.splice(index,1);

  renderItems();
}

function clearBill(){

  if(!confirm("کیا آپ نیا بل بنانا چاہتے ہیں؟")) return;

  items=[];

  document.getElementById("customer").value="";
  document.getElementById("product").value="";
  document.getElementById("price").value="";
  document.getElementById("qty").value=1;

  renderItems();
}

</script>

</body>
</html># T-HTML
invoice-maker
