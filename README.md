# 1. Termuxni yangilash
pkg update -y && pkg upgrade -y

# 2. Python va pip o'rnatish
pkg install python -y
pkg install python-pip -y

# 3. Git va unzip o'rnatish
pkg install git -y
pkg install unzip -y

# 4. Aiogram o'rnatish
pip install -U aiogram

# 5. Bot papkasini yaratish
mkdir -p ~/stars_bot/web
cd ~/stars_bot

# 6. config.py tayyorlash
cat > config.py <<EOL
BOT_TOKEN = "SENING_BOT_TOKEN"
ADMIN_ID = 123456789  # SENING ID
EOL

# 7. bot.py faylini yaratish
cat > bot.py <<'EOL'
import json, asyncio
from aiogram import Bot, Dispatcher
from aiogram.types import Message, InlineKeyboardMarkup, InlineKeyboardButton, WebAppInfo
from config import BOT_TOKEN

bot = Bot(token=BOT_TOKEN)
dp = Dispatcher()

users = {}

def load_users():
    global users
    try:
        with open("users.json", "r") as f:
            users = json.load(f)
    except:
        users = {}

def save_users():
    with open("users.json", "w") as f:
        json.dump(users, f)

load_users()

@dp.message()
async def start(message: Message):
    keyboard = InlineKeyboardMarkup(inline_keyboard=[
        [InlineKeyboardButton(text="💳 Stars sotib olish", web_app=WebAppInfo(url="https://SENING_USERNAME.github.io/Beko/"))]
    ])
    await message.answer("⭐ Stars Premium bot\nStars sotib olish uchun tugmani bosing.", reply_markup=keyboard)

@dp.message()
async def webapp_handler(message: Message):
    if message.web_app_data:
        data = json.loads(message.web_app_data.data)
        stars = int(data.get("stars", 0))
        user_id = str(message.from_user.id)
        users[user_id] = users.get(user_id, 0) + stars
        save_users()
        await message.answer(f"{stars} Stars muvaffaqiyatli qo'shildi! Sizda jami {users[user_id]} Stars bor.")

async def main():
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
EOL

# 8. Mini App fayllari
cd web

# index.html
cat > index.html <<'EOL'
<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Stars Premium</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
body {font-family: 'Roboto', sans-serif; text-align:center; padding:50px 20px; background: linear-gradient(to bottom, #f5f7fa, #c3cfe2);}
h1 {color:#0088cc; font-size:28px; margin-bottom:20px;}
p {font-size:16px; color:#333; margin-bottom:30px;}
.btn {display:inline-block; padding:18px 40px; font-size:18px; font-weight:bold; border-radius:12px; border:none; margin:10px; cursor:pointer; color:white; background:linear-gradient(to right,#00c6ff,#0072ff); transition: transform 0.2s, box-shadow 0.2s;}
.btn:hover {transform:scale(1.05); box-shadow:0 4px 15px rgba(0,0,0,0.2);}
.btn-small {background: linear-gradient(to right,#ff416c,#ff4b2b);}
footer {margin-top:50px; font-size:12px; color:#666;}
</style>
</head>
<body>
<h1>⭐ Stars Premium</h1>
<p>Telegram Stars sotib olish uchun quyidagi tugmani bosing:</p>
<button class="btn" onclick="buyStars(10)">💎 10 Stars</button>
<button class="btn" onclick="buyStars(25)">💎 25 Stars</button>
<button class="btn btn-small" onclick="buyStars(50)">💎 50 Stars</button>
<footer>© 2026 Stars Premium. Barcha huquqlar himoyalangan.</footer>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<script src="script.js"></script>
</body>
</html>
EOL

# script.js
cat > script.js <<'EOL'
function buyStars(amount){
    Telegram.WebApp.sendData(JSON.stringify({stars: amount}));
    alert(amount + " Stars muvaffaqiyatli sotildi!");
}
EOL
