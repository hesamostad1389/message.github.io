<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>بازیابی رمز عبور</title>
  <style>
    body {
      font-family: 'Tahoma', sans-serif;
      background: #f5f7fa;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
    .card {
      background: white;
      padding: 2rem;
      border-radius: 16px;
      box-shadow: 0 6px 20px rgba(0,0,0,0.1);
      width: 100%;
      max-width: 400px;
    }
    .card h2 {
      margin-bottom: 1rem;
      color: #333;
    }
    input[type="password"] {
      width: 100%;
      padding: 0.75rem;
      margin: 0.5rem 0 1rem;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 1rem;
    }
    button {
      background: #4A90E2;
      color: white;
      border: none;
      padding: 0.75rem;
      border-radius: 8px;
      font-size: 1rem;
      cursor: pointer;
      width: 100%;
    }
    button:hover {
      background: #357ABD;
    }
    .message {
      margin-top: 1rem;
      font-size: 0.95rem;
      color: #333;
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="card">
    <h2>بازیابی رمز عبور</h2>
    <input id="password" type="password" placeholder="رمز عبور جدید را وارد کنید" />
    <button onclick="resetPassword()">بازیابی رمز</button>
    <div class="message" id="msg"></div>
  </div>

  <script>
    const urlParams = new URLSearchParams(window.location.search);
    const token = urlParams.get('token');
    const titleId = urlParams.get('titleId');

    async function resetPassword() {
      const password = document.getElementById("password").value;
      const msg = document.getElementById("msg");

      if (!password || password.length < 6) {
        msg.textContent = "رمز باید حداقل ۶ کاراکتر باشد.";
        return;
      }

      try {
        const response = await fetch(`https://${titleId}.playfabapi.com/Client/ConfirmPasswordReset`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "X-PlayFabSDK": "JavaScriptSDK-1.0.0"
          },
          body: JSON.stringify({
            Password: password,
            ConfirmPassword: password,
            Token: token,
            TitleId: titleId
          })
        });

        const data = await response.json();

        if (data.code === 200) {
          msg.style.color = "green";
          msg.textContent = "رمز با موفقیت تغییر کرد! حالا می‌تونی وارد حساب بشی.";
        } else {
          msg.style.color = "red";
          msg.textContent = data.errorMessage || "مشکلی پیش آمد. لطفاً دوباره امتحان کن.";
        }
      } catch (e) {
        msg.style.color = "red";
        msg.textContent = "ارتباط با سرور برقرار نشد.";
      }
    }
  </script>
</body>
</html>
