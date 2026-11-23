# PRODUCT REQUIREMENTS DOCUMENT (PRD)

# AddictCoin 🪙
**Tagline:** Earn Your Screen Time.

| Document Detail | Information |
|---|---|
| Product Name | AddictCoin |
| Document Type | Product Requirements Document (PRD) |
| Version | 1.0 |
| Platform | Android (First Priority) |
| Product Category | Digital Wellbeing / Productivity / Gamification |
| Development Stage | MVP Planning |
| Status | Initial Requirements |

---

# 1. Product Overview

## 1.1 Product Description

AddictCoin ek Android-first Gamified Digital Wellbeing application hai, jiska purpose users ko unnecessary mobile usage aur social media addiction ko control karne mein help karna hai.

Traditional screen-time management applications sirf usage track karti hain ya time limits set karti hain. AddictCoin ek reward-based system use karega, jisme users productive activities complete karke virtual coins earn karenge.

In coins ka use karke users apne selected mobile applications access kar sakenge.

**Core Philosophy:**

"Earn Your Screen Time."

Users ko entertainment access karne ke liye pehle productive activities complete karni hongi.

## 1.2 Problem Statement

Aaj ke time mein bahut se users apna unnecessary time social media aur entertainment applications par spend karte hain.

Major problems:

- Excessive social media usage.
- Productivity aur focus ki kami.
- Study ke time distractions.
- Mobile usage par control maintain karne mein difficulty.
- Traditional screen-time limits ko easily ignore kar dena.

AddictCoin in problems ko gamification aur reward-based access system ke through solve karne ka aim rakhta hai.

## 1.3 Proposed Solution

AddictCoin users ko four productive activities provide karega:

1. Step Counter.
2. Camera-Based Study Mode.
3. Snake Game.
4. Phone-Free Challenge.

Users in activities ko complete karke coins earn karenge.

Earned coins selected applications ko access karne mein spend honge.

---

# 2. Product Goals and Objectives

## 2.1 Primary Goals

- Unnecessary mobile usage reduce karna.
- Users ko physical activity ke liye motivate karna.
- Study aur focus habits improve karna.
- Phone-free time encourage karna.
- Productivity ko gamified experience mein convert karna.
- Users ko apne app usage par better control dena.

## 2.2 Success Metrics

MVP ki success evaluate karne ke liye following metrics track kiye ja sakte hain:

| Metric | Purpose |
|---|---|
| Daily Active Users (DAU) | Daily active users measure karna |
| Challenge Completion Rate | Challenges kitne complete hote hain |
| Average Daily Screen Time | Screen usage mein change |
| Coins Earned | Productive activity participation |
| Coins Spent | Controlled apps ka usage |
| Study Sessions Completed | Study mode engagement |
| Phone-Free Challenge Completion | Phone-free habit engagement |
| Day 7 Retention | Users kitne regularly return karte hain |

Initial launch ke baad in metrics ke liye measurable targets define kiye jayenge.

---

# 3. Target Audience

Primary target audience:

- Students.
- College students.
- Competitive exam aspirants.
- Working professionals.
- Users jo social media usage reduce karna chahte hain.
- Users jo apni productivity aur focus improve karna chahte hain.

---

# 4. User Roles

## 4.1 Registered User

User application mein account create karke:

- Coins earn kar sakta hai.
- Coins spend kar sakta hai.
- Applications manage kar sakta hai.
- Challenges complete kar sakta hai.
- Apni settings customize kar sakta hai.
- Multiple devices par account access kar sakta hai.

## 4.2 Device-Level Configuration

Har device par user ko separately:

- Installed apps select karni hongi.
- Required Android permissions deni hongi.
- App access-control setup complete karna hoga.

Account ka wallet aur cloud data synchronized rahega, lekin device-specific permissions automatically transfer nahi hongi.

---

# 5. Core Product Features

MVP mein exactly four coin-earning features honge.

## 5.1 Feature 1: Step Counter

### Objective

Users ko daily walking aur physical activity ke liye motivate karna.

### A. Random Steps Challenge

System daily ek random step target generate karega.

Example targets:

- 3,000 steps.
- 5,000 steps.
- 8,000 steps.
- 10,000 steps.

### Functional Requirements

- User daily step target dekh sakega.
- Steps automatically track honge.
- Progress dashboard par show hogi.
- Target complete hone par coins award honge.
- Ek daily challenge ke liye duplicate rewards prevent hone chahiye.
- Daily challenge ka completion record save hoga.

### B. Random Location Challenge

System user ko ek random destination assign karega.

### Functional Requirements

- User location challenge start kar sakega.
- User Settings mein destination distance configure kar sakega.
- System eligible public destination select karega.
- Map par destination aur walking route display hoga.
- GPS ke through destination arrival verify hoga.
- Valid completion par coins award honge.
- Challenge history save hogi.

### Safety Requirements

- Private properties avoid honi chahiye.
- Restricted areas avoid hone chahiye.
- Highways aur unsafe walking routes avoid hone chahiye.
- Destination accessible public location honi chahiye.
- Location permission ka use sirf required functionality ke liye hona chahiye.

---

## 5.2 Feature 2: Camera-Based Study Mode

### Objective

Users ko distraction-free study sessions complete karne ke liye encourage karna.

### Functional Requirements

- User study session start kar sakega.
- Study session ki duration configurable hogi.
- Camera session ke dauran active rahega.
- Study duration track hogi.
- Eligible session complete hone par coins award honge.
- User session end kar sakega.
- Completed aur interrupted sessions ka record maintain hoga.

### Privacy Requirements

- Camera access sirf active study session ke liye.
- Camera recordings store nahi honi chahiye.
- Camera recordings backend par upload nahi honi chahiye.
- Camera permission user consent se li jayegi.

### Screen-Off Requirement

User chahta hai ki study ke time screen off ho sake aur camera monitoring continue rahe.

Lekin Android background camera restrictions ke karan normal screen-off camera monitoring directly possible nahi bhi ho sakti.

Development se pehle Android compatibility validate karna zaroori hai.

Agar exact functionality possible na ho, toh practical alternative propose karna hoga.

---

## 5.3 Feature 3: Snake Game

### Objective

Users ko ek built-in game ke through coins earn karne ka option dena.

### Functional Requirements

- App ke andar Snake Game available hoga.
- User game start aur restart kar sakega.
- Game score calculate hoga.
- Score ke basis par eligible coins calculate honge.
- Completed game ka record save hoga.
- Reward limits apply hongi.
- Duplicate rewards aur unlimited coin farming prevent honi chahiye.

### Reward Rules

- Reward calculation predefined rules ke according hogi.
- Maximum reward per game configurable hoga.
- Daily reward cap configurable hoga.
- Invalid game submissions reject hone chahiye.

---

## 5.4 Feature 4: Phone-Free Challenge

### Objective

Users ko phone se break lene aur uninterrupted time spend karne ke liye motivate karna.

### Default Challenge

2 Hours Phone-Free Challenge.

### Functional Requirements

- User challenge start kar sakega.
- Challenge duration track hogi.
- User ko remaining time show hoga.
- Challenge complete hone par coins award honge.
- Completed aur failed challenges ka record save hoga.

### Phone Call Exception

Phone calls ko phone usage nahi maana jayega.

Requirements:

- Incoming calls challenge fail nahi karengi.
- Outgoing calls challenge fail nahi karengi.
- Call duration phone-free usage calculation se exclude hogi.
- Call disconnect hone ke baad challenge resume hoga.
- Call ke time challenge reset nahi hona chahiye.

Call audio ya call content access nahi kiya jayega.

Android call-state APIs aur available permissions ke through feasibility verify karni hogi.

---

# 6. Coin Wallet and Economy System

## 6.1 Wallet

Har registered user ka ek central coin wallet hoga.

Wallet mein following information maintain hogi:

- Current coin balance.
- Total coins earned.
- Total coins spent.
- Transaction history.

## 6.2 Coin Earning

Users four approved activities complete karke coins earn karenge.

Coin rewards predefined rules aur configurable settings ke according calculate honge.

## 6.3 Coin Spending

Users selected applications access karne ke liye coins spend karenge.

Example:

| Application | Example Charging Rule |
|---|---|
| Instagram | 1 Reel = 1 Coin |
| YouTube | 1 Minute = 1 Coin |
| Facebook | User-defined rate |
| Other Apps | User-defined rate |

Ye example rates hain, final rates user customize kar sakega.

## 6.4 Coin Deduction Rules

- App usage ke according coins deduct honge.
- Har application ka individual rate hoga.
- Wallet balance real-time ya near real-time update hona chahiye.
- Insufficient balance hone par controlled app access block hona chahiye.
- Duplicate deductions prevent honi chahiye.
- Transaction history maintain honi chahiye.

## 6.5 Zero Balance Rule

Jab wallet balance zero ho:

- Selected controlled apps lock ho jayengi.
- User un applications ko access nahi kar payega.
- User available earning activities complete karke coins earn kar sakega.
- Sufficient balance hone par app access restore ho sakega.

## 6.6 Wallet Security

- Wallet balance ka source of truth backend hoga.
- Coin earning aur spending server-side validate hogi.
- Atomic transactions use honge.
- Duplicate reward requests prevent hongi.
- Transaction IDs unique hone chahiye.
- Client-side balance modification accept nahi honi chahiye.

---

# 7. Application Management

## 7.1 Installed Applications

User device mein installed eligible applications ki list dekh sakega.

## 7.2 Application Selection

User:

- Applications add kar sakega.
- Applications remove kar sakega.
- Individual application ka coin rate set kar sakega.
- Application access control enable/disable kar sakega.

## 7.3 Application Usage Tracking

System selected applications ka usage track karega.

Usage tracking ka purpose:

- App access duration calculate karna.
- Coins deduct karna.
- Usage limits enforce karna.
- Wallet update karna.

## 7.4 App Locking

Insufficient coin balance hone par selected apps ka access restrict hoga.

Android OS restrictions aur Play Store policies ko consider karke supported implementation select karna hoga.

App ko guaranteed tamper-proof blocking ka promise nahi karna chahiye.

---

# 8. Settings and Customization

User ko application ke important parameters customize karne ka option milna chahiye.

### App Management Settings

- Controlled apps add/remove.
- Per-app coin rate.
- App access control.

### Challenge Settings

- Daily step challenge preferences.
- Location challenge distance.
- Study session duration.
- Phone-free challenge duration.
- Snake game reward limits.

### Wallet Settings

- Coin earning rules.
- Coin spending rules.
- Daily reward limits.

### General Settings

- Notifications.
- Dark mode / Light mode.
- Privacy preferences.
- Permission management.

Har setting ke liye valid limits aur server-side validation define honi chahiye.

---

# 9. Authentication and User Profile

## 9.1 Authentication Requirements

Application mein email-based authentication hogi.

Required functionality:

- Signup.
- Login.
- Logout.
- Email verification.
- Forgot password.
- Reset password.
- User profile.

## 9.2 Security Requirements

- Passwords securely hashed hone chahiye.
- JWT-based authentication.
- Access token aur refresh token management.
- Protected API routes.
- Secure session invalidation.
- Rate limiting on authentication endpoints.

---

# 10. Backend and Database Requirements

## 10.1 Backend Responsibilities

Backend following operations manage karega:

- User accounts.
- Authentication.
- Wallet balance.
- Coin transactions.
- App configurations.
- User settings.
- Activity records.
- Challenge records.
- Study session records.
- Game scores.
- Multi-device synchronization.

## 10.2 Proposed Database Collections

| Collection | Responsibility |
|---|---|
| users | User account information |
| wallets | Coin balance |
| transactions | Earning and spending history |
| controlled_apps | Selected apps and rates |
| user_settings | User preferences |
| study_sessions | Study records |
| step_challenges | Step challenge history |
| location_challenges | Location challenge history |
| phone_free_sessions | Phone-free records |
| game_scores | Snake game records |
| device_sessions | Device authentication sessions |

Database schema aur relationships development architecture phase mein finalize honge.

---

# 11. Multi-Device Cloud Synchronization

## Objective

User same account ko multiple Android devices par access kar sake.

### Cloud-Synchronized Data

- User profile.
- Coin wallet.
- Transaction history.
- Controlled app configurations.
- Coin rates.
- User settings.
- Activity history.
- Challenge history.

### Device-Specific Data

Har device par separately manage hoga:

- Installed applications.
- Android permissions.
- Camera permissions.
- Location permissions.
- Step sensor availability.
- Local app access-control configuration.

### Synchronization Requirements

- Same account ka wallet sabhi devices par consistent rahe.
- Coin transactions backend par atomic hon.
- Concurrent spending ke cases handle hon.
- Offline activity ke liye safe synchronization strategy ho.
- Duplicate rewards prevent hon.
- Device-specific configuration cloud account data ko overwrite na kare.

---

# 12. Non-Functional Requirements

## 12.1 Performance

- Application responsive honi chahiye.
- Dashboard efficiently load hona chahiye.
- Coin balance updates reliable hone chahiye.
- Background tracking battery-efficient honi chahiye.

## 12.2 Security

- Secure authentication.
- Protected API endpoints.
- Server-side reward validation.
- Secure wallet transactions.
- Input validation.
- Abuse prevention.
- Sensitive data protection.

## 12.3 Privacy

- Minimum required permissions.
- Camera recordings store nahi hongi.
- Location data sirf required challenge functionality ke liye use hoga.
- User ko permission usage clearly explain karna hoga.
- Personal data unauthorized third parties ke saath share nahi hona chahiye.

## 12.4 Scalability

- Modular backend architecture.
- Maintainable database design.
- Reusable frontend components.
- Feature-wise separation.
- Future updates ke liye extensible structure.

## 12.5 Reliability

- Wallet balance consistent hona chahiye.
- Failed transactions safely handle honi chahiye.
- Duplicate reward claims reject honi chahiye.
- App restart hone par active challenge state recover ho sake.

---

# 13. Proposed Technology Stack

| Layer | Technology |
|---|---|
| Mobile Frontend | React Native |
| Programming Language | TypeScript |
| Native Android | Kotlin |
| Android SDK | Android SDK |
| Camera | CameraX |
| Step Tracking | Android Sensors / Health Connect |
| Location | Google Maps SDK / Fused Location Provider |
| App Usage | UsageStatsManager |
| Backend | Node.js |
| Backend Framework | Express.js |
| Database | MongoDB |
| ODM | Mongoose |
| Authentication | JWT |
| API Style | REST API |
| Version Control | Git / GitHub |
| API Testing | Postman |
| UI Design