<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>بازیابی رمز عبور</title>
  <style>
    body {
      font-family: Vazirmatn, Tahoma, Arial, sans-serif;
      background: linear-gradient(135deg, #6a11cb, #2575fc);
      color: #fff;
      height: 100vh;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      text-align: center;
    }
    .container {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 15px;
      padding: 40px 30px;
      max-width: 400px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.25);
    }
    input[type="password"] {
      width: 100%;
      padding: 12px;
      margin: 15px 0;
      border-radius: 8px;
      border: none;
      font-size: 16px;
    }
    button {
      background-color: #fff;
      color: #2575fc;
      border: none;
      padding: 12px 25px;
      border-radius: 30px;
      font-weight: bold;
      cursor: pointer;
      font-size: 16px;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #e0e0e0;
    }
    .message {
      margin-top: 20px;
      font-weight: bold;
    }
  </style>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/playfab-sdk/2.121.211014/PlayFabClientApi.min.js"></script>
</head>
<body>
  <div class="container">
    <h1>بازیابی رمز عبور</h1>
    <p>رمز عبور جدید خود را وارد کنید:</p>
    <input type="password" id="newPassword" placeholder="رمز عبور جدید" />
    <button onclick="resetPassword()">ثبت و تغییر رمز</button>
    <div id="message" class="message"></div>
  </div>

  <script>
    // تابع گرفتن پارامتر از URL
    function getQueryParam(param) {
      const urlParams = new URLSearchParams(window.location.search);
      return urlParams.get(param);
    }

    // مقدارهای مهم
    const token = getQueryParam("token");
    const titleId = "88FCD"; // اینجا TitleID بازی خودت رو بگذار

    // چک کردن وجود توکن
    if (!token) {
      document.getElementById("message").textContent = "توکن بازیابی رمز یافت نشد!";
    }

    function resetPassword() {
      const newPassword = document.getElementById("newPassword").value.trim();
      if (newPassword.length < 6) {
        document.getElementById("message").textContent = "رمز عبور باید حداقل 6 کاراکتر باشد.";
        return;
      }

      PlayFab.settings.titleId = titleId;

      const request = {
        Token: token,
        Password: newPassword
      };

      PlayFabClient.ResetPassword(request, function(result) {
        document.getElementById("message").textContent = "رمز عبور شما با موفقیت تغییر کرد!";
      }, function(error) {
        document.getElementById("message").textContent = "خطا در تغییر رمز: " + error.errorMessage;
      });
    }
  </script>
</body>
</html>
