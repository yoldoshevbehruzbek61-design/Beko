<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Stars Premium Buy</title>

  <script src="https://telegram.org/js/telegram-web-app.js"></script>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      margin: 0;
      padding: 20px;
    }
    .card {
      background: white;
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    button {
      width: 100%;
      padding: 14px;
      font-size: 16px;
      background: #0088cc;
      color: white;
      border: none;
      border-radius: 8px;
      margin-top: 15px;
    }
  </style>
</head>
<body>

<div class="card">
  <h2>⭐ Stars Premium</h2>
  <p>Telegram Premium / Stars sotib olish</p>

  <button onclick="buy()">Sotib olish</button>
</div>

<script>
  const tg = window.Telegram.WebApp;
  tg.expand();

  function buy() {
    tg.showAlert("To‘lov tez orada qo‘shiladi");
  }
</script>

</body>
</html>
