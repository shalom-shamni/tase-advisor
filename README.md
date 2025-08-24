# TASE Advisor — סורק מניות לבחירה ידנית (IL)

פרויקט פתוח שמקבל **רשימת מניות נבחרות** מהמשתמש, מושך נתונים מספק (TASE Open API / ספק חלופי),
מחשב פיצ'רים בסיסיים (מומנטום 12-1, מרחק משיא 52ש׳, תנודתיות 63 ימים, נזילות), מדרג ויוצר **המלצות**.

> ⚠️ שימוש למטרות לימוד/POC. לא ייעוץ השקעות. ודאו רישוי נתונים ושימוש ב-API הרשמי של הבורסה (TASE) או ספק חוקי אחר.

## התקנה מהירה
```bash
pip install -e .
cp .env.example .env   # מלאו מפתחות
```

## הרצה בסיסית
```bash
# ספק נתונים 'fake' משתמש ב-data/prices_template.csv לצורך ניסוי מהיר
tase-advisor run --provider fake --tickers config/tickers.yml --settings config/settings.yml --out out

# לשימוש ב-TASE Open API (לאחר קבלת API Key והגדרת endpoints בקובץ המחבר):
tase-advisor run --provider tase --tickers config/tickers.yml --settings config/settings.yml --out out
```

## קבצים חשובים
- `config/tickers.yml` — רשימת המניות לסריקה (סימבול/ISIN/מזהה TASE אם יש)
- `config/settings.yml` — פרמטרי מודל (מספר ניירות לבחור, חלון זמן, פילטר נזילות וכו')
- `src/tase_advisor/connectors/tase.py` — מחבר ל־TASE Open API (יש להתאים endpoints + Authentication לפי המוצר שקניתם)
- `src/tase_advisor/connectors/fake.py` — ספק נתונים חלופי לקריאה מ-CSV (לבדיקות)
- `src/tase_advisor/rules/screens.py` — חישוב פיצ'רים, ניקוד ומיון
- `out/` — תוצרי ריצה: features.csv, picks.csv, logs.json (ואפשר להרחיב)

## GitHub Actions
פעולת CI יומית (UTC 14:30 ≈ 17:30 Asia/Jerusalem בימים א׳–ה׳) תריץ את הסורק ותבצע commit לתוצאות.
ראו `.github/workflows/schedule.yml` והגדירו `TASE_API_KEY` ב-Secrets של GitHub.

## אחריות ושימוש
- אין התחייבות לדיוק/שלמות הנתונים. 
- אל תגרדו את אתר הבורסה; השתמשו ב-Open API/Data Hub לפי תנאי שימוש/רישוי.
- זה **לא** ייעוץ השקעות. קבלו החלטות באחריותכם ופנו ליועץ מורשה במידת הצורך.
