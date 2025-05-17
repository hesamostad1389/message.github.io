<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>بازیابی رمز عبور</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Vazirmatn&display=swap');
    body {
      font-family: 'Vazirmatn', sans-serif;
      background: linear-gradient(135deg, #6a11cb, #2575fc);
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      padding: 20px;
      text-align: center;
    }
    .container {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 15px;
      padding: 40px 30px;
      max-width: 450px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.25);
      width: 100%;
    }
    h1 {
      font-size: 2.5rem;
      margin-bottom: 20px;
    }
    input[type="password"] {
      width: 100%;
      padding: 12px;
      border-radius: 10px;
      border: none;
      margin-bottom: 25px;
      font-size: 1.1rem;
    }
    button {
      background-color: #fff;
      color: #2575fc;
      border: none;
      padding: 15px 30px;
      border-radius: 30px;
      font-weight: bold;
      font-size: 1.2rem;
      cursor: pointer;
      transition: background-color 0.3s ease;
      width: 100%;
    }
    button:hover {
      background-color: #e0e0e0;
    }
    #message {
      margin-top: 30px;
      font-size: 1.1rem;
      min-height: 24px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>بازیابی رمز عبور</h1>
    <input type="password" id="newPassword" placeholder="رمز عبور جدید را وارد کنید" />
    <button onclick="resetPassword()">تغییر رمز عبور</button>
    <div id="message"></div>
  </div>

  <!-- اینجا SDK لوکال یا CDN خودت رو قرار بده -->
  <script src="PlayFabClientApi.js"></script>
  <script>
    // گرفتن کوئری استرینگ از URL
    function getQueryParam(param) {
      const urlParams = new URLSearchParams(window.location.search);
      return urlParams.get(param);
    }

    const token = getQueryParam('token');
    const titleId = "88FCD";  // تایتل آیدی خودت رو اینجا بزار

    // مقدار دهی تنظیمات PlayFab
    PlayFab.settings.titleId = titleId;

    function resetPassword() {
      const newPassword = document.getElementById('newPassword').value.trim();
      const messageEl = document.getElementById('message');

      if (!newPassword) {
        messageEl.textContent = "لطفاً رمز عبور جدید را وارد کنید.";
        return;
      }
      if (!token) {
        messageEl.textContent = "توکن بازیابی پیدا نشد.";
        return;
      }

      const request = {
        Password: newPassword,
        Token: token
      };

      PlayFabClientApi.ResetPassword(request,
        function (result) {
          messageEl.style.color = "#b5f27f";
          messageEl.textContent = "رمز عبور با موفقیت تغییر کرد!";
        },
        function (error) {
          messageEl.style.color = "#ff6b6b";
          messageEl.textContent = "خطا: " + error.errorMessage;
          console.error("ResetPassword error:", error);
        });
    }
  </script>
</body>
</html>
