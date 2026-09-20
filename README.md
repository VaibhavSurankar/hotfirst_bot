# FirstCry Hot Wheels Telegram Alert Bot

FirstCry.com par "hot wheels" search karta hai, real car-brand models ko filter karta hai
(fantasy/bike models ko chhod ke), aur naye stock milne par Telegram pe alert bhejta hai.
Har 2 minute mein check karta hai.

## Files
- `bot.py` — main script
- `requirements.txt` — Python dependencies
- `railway.toml` — Railway deployment config

## Setup (Step by step)

### 1. Apna khud ka Telegram bot banao (agar nahi bana hua)
1. Telegram mein `@BotFather` ko message karo
2. `/newbot` bhejo, naam aur username do
3. Jo token milega wahi tumhara `BOT_TOKEN` hai

### 2. Apna Chat ID pata karo
1. Apne bot ko Telegram pe koi message bhejo (e.g. "hi")
2. Browser mein kholo: `https://api.telegram.org/bot<TOKEN>/getUpdates`
3. Response ke JSON mein `"chat":{"id": ...}` dikhega — wahi `CHAT_ID` hai

### 3. Local test (optional, deploy se pehle sanity check ke liye)
```bash
git clone <tumhara-forked-repo-url>
cd firstcry-hotwheels-bot

python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

pip install -r requirements.txt
playwright install chromium

export BOT_TOKEN="tumhara_bot_token"
export CHAT_ID="tumhara_chat_id"

python bot.py
```
Pehli run pe sirf baseline save hota hai (`seen.json` banegi), koi alert nahi aata — ye normal hai.
Dusri run se agle naye items par Telegram alert aayega.

### 4. Railway pe 24x7 deploy karo
1. [railway.app](https://railway.app) pe sign up/login karo (GitHub se ho sakta hai)
2. **New Project → Deploy from GitHub repo** → apna forked repo select karo
3. Project ke **Variables** tab mein jao aur add karo:
   - `BOT_TOKEN` = tumhara telegram bot token
   - `CHAT_ID` = tumhara chat id
4. Deploy trigger hoga — `railway.toml` khud dependencies install karega
   (playwright + chromium browser bhi automatically)
5. **Deployments → Logs** mein check karo ki bot start ho gaya
   ("🤖 Hot Wheels FirstCry bot STARTED..." message tumhare Telegram pe aayega)

### 5. Confirm karo ki chal raha hai
- Deploy hote hi ek "started" message Telegram pe milega
- Uske baad jab bhi koi naya "real brand" Hot Wheels product FirstCry pe list ho,
  tumhe alert milega

## Security note
`BOT_TOKEN` aur `CHAT_ID` kabhi bhi code mein hardcode mat karo ya GitHub pe commit mat karo —
sirf environment variables (Railway dashboard ya local `export`) ke through set karo.

## Customization
- `REAL_BRANDS`, `FANTASY_KEYWORDS`, `BIKE_KEYWORDS` lists ko `bot.py` mein edit karke
  apni filtering preferences adjust kar sakte ho
- Check interval badalne ke liye `time.sleep(120)` line ko edit karo (seconds mein)
