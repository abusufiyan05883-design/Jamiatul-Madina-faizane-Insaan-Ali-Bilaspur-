<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Institute Settings</title>

  <style>
    body{
      font-family: Arial;
      background:#f5f5f5;
      padding:30px;
    }

    .box{
      max-width:600px;
      margin:auto;
      background:white;
      padding:25px;
      border-radius:12px;
      box-shadow:0 0 10px rgba(0,0,0,0.1);
    }

    h2{
      color:#0b5d3b;
    }

    label{
      display:block;
      margin-top:15px;
      font-weight:bold;
    }

    input{
      width:100%;
      padding:12px;
      margin-top:5px;
      border:1px solid #ccc;
      border-radius:8px;
    }

    button{
      margin-top:20px;
      background:#0b5d3b;
      color:white;
      border:none;
      padding:12px 20px;
      border-radius:8px;
      cursor:pointer;
    }

    button:hover{
      background:#08492f;
    }

    .msg{
      margin-top:15px;
      color:green;
      font-weight:bold;
    }
  </style>
</head>

<body>

<div class="box">
  <h2>Institute Contact & Details</h2>

  <label>Helpline Number</label>
  <input type="text" id="fancy" placeholder="1800-123-4567">

  <label>Phone Number</label>
  <input type="text" id="phone" placeholder="+91 99999 88888">

  <label>Email</label>
  <input type="email" id="email" placeholder="info@jamiatulmadina.in">

  <label>Address</label>
  <input type="text" id="address" placeholder="Bilaspur">

  <button onclick="saveData()">Save Details</button>

  <div class="msg" id="msg"></div>
</div>

<script>
function saveData(){

  const data = {
    helpline: document.getElementById('fancy').value,
    phone: document.getElementById('phone').value,
    email: document.getElementById('email').value,
    address: document.getElementById('address').value
  };

  localStorage.setItem('instituteData', JSON.stringify(data));

  document.getElementById('msg').innerHTML =
    'Institute details saved successfully!';
}

window.onload = function(){

  const saved = JSON.parse(localStorage.getItem('instituteData'));

  if(saved){
    document.getElementById('fancy').value = saved.helpline || '';
    document.getElementById('phone').value = saved.phone || '';
    document.getElementById('email').value = saved.email || '';
    document.getElementById('address').value = saved.address || '';
  }
}
</script>

</body>
</html>
