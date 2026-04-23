<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Auth Demo</title>
    <style>
        body { font-family: Arial; background:#f4f4f4; display:flex; justify-content:center; align-items:center; height:100vh; }
        .box { background:#fff; padding:20px; border-radius:12px; width:350px; box-shadow:0 5px 20px rgba(0,0,0,0.1);} 
        input, button { width:100%; padding:10px; margin:8px 0; border-radius:8px; border:1px solid #ccc; }
        button { background:#6a5cff; color:#fff; border:none; cursor:pointer; }
        button:hover { background:#5848d6; }
        .hidden { display:none; }
    </style>
</head>
<body>

<div class="box">
    <h2>Ro'yxatdan o'tish</h2>
    <input type="text" id="name" placeholder="Ism">
    <input type="email" id="email" placeholder="Email">
    <input type="text" id="phone" placeholder="Telefon (+998...)">
    <input type="password" id="password" placeholder="Parol">
    <button onclick="register()">Ro'yxatdan o'tish</button>

    <div id="smsBox" class="hidden">
        <input type="text" id="smsCode" placeholder="SMS kod">
        <button onclick="verifySMS()">SMS tasdiqlash</button>
    </div>

    <div id="emailBox" class="hidden">
        <input type="text" id="emailCode" placeholder="Email kod">
        <button onclick="verifyEmail()">Email tasdiqlash</button>
    </div>

    <div id="loginBox" class="hidden">
        <h3>Login</h3>
        <input type="email" id="loginEmail" placeholder="Email">
        <input type="password" id="loginPass" placeholder="Parol">
        <button onclick="login()">Kirish</button>
    </div>
</div>

<script>
let userDB = {};
let generatedSMS = "";
let generatedEmail = "";

function register(){
    const name = document.getElementById('name').value;
    const email = document.getElementById('email').value;
    const phone = document.getElementById('phone').value;
    const password = document.getElementById('password').value;

    if(!name || !email || !phone || !password){
        alert("Hammasini to'ldir");
        return;
    }

    generatedSMS = Math.floor(1000 + Math.random()*9000).toString();
    alert("SMS kod (demo): " + generatedSMS);

    userDB = { name, email, phone, password, smsVerified:false, emailVerified:false };

    document.getElementById('smsBox').classList.remove('hidden');
}

function verifySMS(){
    const code = document.getElementById('smsCode').value;
    if(code === generatedSMS){
        alert("SMS tasdiqlandi");
        userDB.smsVerified = true;

        generatedEmail = Math.floor(1000 + Math.random()*9000).toString();
        alert("Email kod (demo): " + generatedEmail);

        document.getElementById('emailBox').classList.remove('hidden');
    } else {
        alert("Xato kod");
    }
}

function verifyEmail(){
    const code = document.getElementById('emailCode').value;
    if(code === generatedEmail){
        alert("Email tasdiqlandi");
        userDB.emailVerified = true;

        document.getElementById('loginBox').classList.remove('hidden');
    } else {
        alert("Xato email kod");
    }
}

function login(){
    const email = document.getElementById('loginEmail').value;
    const pass = document.getElementById('loginPass').value;

    if(email === userDB.email && pass === userDB.password){
        if(userDB.smsVerified && userDB.emailVerified){
            alert("Muvaffaqiyatli login");
        } else {
            alert("Tasdiqlash tugallanmagan");
        }
    } else {
        alert("Login xato");
    }
}

// Qo'shimcha xavfsizlik funksiyalari
function hashPassword(password){
    return btoa(password); // demo hash
}

function checkPasswordStrength(password){
    if(password.length < 6){
        return "Zaif";
    } else if(password.length < 10){
        return "O'rtacha";
    } else {
        return "Kuchli";
    }
}

// Token yaratish
function generateToken(){
    return Math.random().toString(36).substr(2);
}

// Session saqlash
function saveSession(token){
    localStorage.setItem("token", token);
}

function logout(){
    localStorage.removeItem("token");
    alert("Chiqildi");
}

// Fake API chaqiriq
function fakeAPI(){
    return new Promise((resolve)=>{
        setTimeout(()=> resolve("OK"),1000);
    });
}

// Rate limit (oddiy)
let attempts = 0;
function rateLimit(){
    attempts++;
    if(attempts > 5){
        alert("Ko'p urinish");
        return false;
    }
    return true;
}

// CSRF token demo
let csrf = generateToken();
console.log("CSRF:", csrf);

// Init
console.log("Auth system ready");
</script>

</body>
</html>
