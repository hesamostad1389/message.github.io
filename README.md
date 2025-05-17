<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>بازیابی رمز عبور</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://download.playfab.com/PlayFabClientApi.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Vazirmatn&display=swap');
    body {
      font-family: 'Vazirmatn', sans-serif;
      background: linear-gradient(135deg, #ff416c, #ff4b2b);
      color: #fff;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      padding: 20px;
    }

    .container {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 15px;
      padding: 40px 30px;
      max-width: 450px;
      width: 100%;
      box-shadow: 0 8px 16px rgba(0,0,0,0.25);
      text-align: center;
    }

    h1 {
      font-size: 2rem;
      margin-bottom: 10px;
    }

    p {
      font-size: 1.1rem;
      margin-bottom: 25px;
    }

    input[type="password"] {
      width: 100%;
      padding: 12px;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      margin-bottom: 20px;
    }

    button {
      background-color: #fff;
      color: #ff4b2b;
      padding: 12px 25px;
      border: none;
      border-radius: 30px;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #f2f2f2;
    }

    .success, .error {
      display: none;
      margin-top: 20px;
    }

    .success { color: #b2ff59; }
    .error { color: #ffcccb; }
  </style>
</head>
<body>
  <div class="container">
    <h1>بازیابی رمز عبور</h1>
    <p>رمز عبور جدید خود را وارد کنید:</p>
    <input type="password" id="newPassword" placeholder="رمز جدید">
    <button onclick="resetPassword()">تغییر رمز</button>
    <div class="success" id="successMessage">رمز عبور با موفقیت تغییر کرد ✅</div>
    <div class="error" id="errorMessage">خطا در تغییر رمز! 😢</div>
  </div>

  <script>
    function getQueryParam(name) {
      const urlParams = new URLSearchParams(window.location.search);
      return urlParams.get(name);
    }

    function resetPassword() {
      const newPassword = document.getElementById("newPassword").value;
      const token = getQueryParam("token");
      const titleId = getQueryParam("titleId");

      if (!newPassword || newPassword.length < 6) {
        alert("رمز عبور باید حداقل ۶ کاراکتر باشد.");
        return;
      }

      if (!token || !titleId) {
        alert("توکن یا titleId یافت نشد!");
        return;
      }

      PlayFab.settings.titleId = titleId;

      const request = {
        Token: token,
        Password: newPassword
      };

      PlayFab.ClientApi.ResetPassword(request, function(result) {
        document.getElementById("successMessage").style.display = "block";
        document.getElementById("errorMessage").style.display = "none";
      }, function(error) {
        console.error("Reset error:", error);
        document.getElementById("successMessage").style.display = "none";
        document.getElementById("errorMessage").style.display = "block";
      });
    }
  </script>
</body>
</html>
