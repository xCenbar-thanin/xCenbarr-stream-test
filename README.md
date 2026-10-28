# xCenbar Stream Alerts

គេហទំព័រ alert សម្រាប់ streamer ថ្មី៖ donation alert + TTS, leaderboard, ticker, subathon timer, donation goal និងទំព័រ tip (`/@username`)។

## ១. រចនាសម្ព័ន្ធថត (folder)

```
xcenbar-stream/            <- ថតគម្រោង (ឈ្មោះអ្វីក៏បាន)
├─ package.json            <- បញ្ជី library ដែលត្រូវការ
├─ .env.example            <- ចម្លងទៅជា .env រួចបំពេញ
├─ server/                 <- BACKEND (Node.js + Express)
│  ├─ index.js             <- ចាប់ផ្តើម server
│  ├─ db.js                <- DATABASE (SQLite): បង្កើតតារាង
│  ├─ defaults.js          <- ការកំណត់ដើមសម្រាប់គណនីថ្មី
│  ├─ store.js             <- អាន/សរសេរ database
│  ├─ auth.js              <- LOGIN: register, login, Google
│  ├─ api.js               <- settings, donations, tip page, webhook
│  └─ events.js            <- ផ្ញើ alert ទៅ OBS ភ្លាមៗ
└─ public/                 <- FRONTEND (អ្វីដែល browser បើក)
   ├─ index.html           <- ទំព័រស្វាគមន៍ (welcome + Register now)
   ├─ login.html           <- ទំព័រ Login / Sign up (+ Google)
   ├─ app.html             <- Dashboard (ក្រោយ login)
   ├─ tip.html             <- ទំព័រ tip សាធារណៈ (/@username)
   ├─ css/style.css        <- CSS
   ├─ js/common.js         <- ជំនួយទូទៅ
   ├─ js/render.js         <- គូរ alert/ticker/timer/goal/leaderboard
   ├─ js/home.js           <- JavaScript របស់ទំព័រស្វាគមន៍
   ├─ js/auth.js           <- JavaScript របស់ទំព័រ login / sign up
   ├─ js/app.js            <- JavaScript របស់ dashboard
   ├─ js/tip.js            <- JavaScript របស់ទំព័រ tip
   ├─ js/overlay.js        <- JavaScript របស់ OBS overlay
   └─ overlays/overlay.html <- ទំព័រតែមួយសម្រាប់ OBS overlay ទាំង ៦
```

ឈ្មោះ `index` ប្រើសម្រាប់ឯកសារ `index.html` (ទំព័រដំបូងដែល browser បើកដោយស្វ័យប្រវត្តិ) មិនមែនសម្រាប់ folder ទេ។

## ២. ដំឡើងក្នុង VS Code

1. ដំឡើង **Node.js LTS** (v18 ឬថ្មីជាង): https://nodejs.org
2. VS Code → **File → Open Folder…** → ជ្រើស `xcenbar-stream`
3. បើក Terminal (**Terminal → New Terminal**) រួចវាយ៖
   ```
   npm install
   ```
4. ចម្លង `.env.example` ទៅជា `.env` (ក្នុង VS Code: ចុចស្តាំលើឯកសារ → Copy → Paste → Rename) រួចប្តូរ `JWT_SECRET` ទៅជាអក្សរវែងៗចៃដន្យ។
5. ចាប់ផ្តើម server៖
   ```
   npm start
   ```
6. បើក browser ទៅ http://localhost:3000 → ចុច **Register now** → Sign up → ចូល Dashboard។

Database ត្រូវបានបង្កើតដោយស្វ័យប្រវត្តិនៅ `data/xcenbar.db` (ឯកសារតែមួយ)។

## ៣. ប្រើជាមួយ OBS

1. Dashboard → ទំព័រ **Alert Settings** (ឬ Ticker, Timer, Goal, Leaderboard) → ចុច **Copy Link**
2. OBS → Sources **+** → **Browser** → ដាក់ link → ដាក់ Width/Height តាមដែលបង្ហាញ
3. ចុច **Send test donation** ក្នុង Dashboard ដើម្បីសាកល្បង alert

OBS លើកុំព្យូទ័រផ្សេង ឬ stream ពិត៖ link ត្រូវតែចូលដល់ server បាន ដូច្នេះត្រូវ host server លើអ៊ីនធឺណិត (ឧទាហរណ៍ Render, Railway, VPS) ដោយប្រើ https។ `localhost` ដំណើរការតែលើកុំព្យូទ័រតែមួយ។

TTS: OBS Browser Source អាចអានសំឡេងបាន ឬមិនបាន អាស្រ័យលើកំណែ OBS និងសំឡេងដែលមាននៅលើកុំព្យូទ័រ។ សាកល្បងជាមុន។

## ៤. ការទូទាត់ (សំខាន់)

- Tip page ប្រើ **link ABA PayWay របស់អ្នកផ្ទាល់** (ដាក់ក្នុង Payments & Tip)។ អ្នកទិញចុចបង់ប្រាក់នៅ ABA រួចចុច "I have paid"។
- Donation នោះទៅជា **Waiting for your confirmation** ក្នុង Dashboard។ អ្នកពិនិត្យក្នុងកម្មវិធី ABA ហើយចុច **Confirm paid** ទើប alert លេង, goal និង timer កើនឡើង។
- ការបញ្ជាក់ស្វ័យប្រវត្តិ (webhook) នៅ `server/api.js` ជាគ្រាន់តែគំរូ។ មុនប្រើ ត្រូវផ្គូផ្គងវាជាមួយឯកសារ callback ពិតរបស់ ABA PayWay (ការ sign និងការត្រួតពិនិត្យ) ហើយសាកល្បងក្នុង sandbox របស់ ABA។ កុំបើកការបញ្ជាក់ស្វ័យប្រវត្តិដោយមិនទាន់ធ្វើដូច្នេះ។

## ៥. Login with Google (ជម្រើស)

បង្កើត OAuth Client ប្រភេទ Web នៅ console.cloud.google.com ហើយដាក់ `GOOGLE_CLIENT_ID` ក្នុង `.env`។ បន្ថែម `http://localhost:3000` ក្នុង Authorized JavaScript origins។ ប៊ូតុង Google នឹងបង្ហាញដោយស្វ័យប្រវត្តិ។

## ៦. មុនដាក់ជាសាធារណៈ

- ប្រើ **https** ហើយដាក់ `NODE_ENV=production` ក្នុង `.env`
- `JWT_SECRET` ត្រូវវែង និងសម្ងាត់
- Backup ឯកសារ `data/xcenbar.db` ជាប្រចាំ
- ពាក្យសម្ងាត់ត្រូវបាន hash ដោយ bcrypt ហើយ cookie ជា httpOnly
