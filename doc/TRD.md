# ADDICTCOIN
## Technical Requirements Document (TRD)

**Version:** 1.0  
**Platform:** Android  
**Architecture:** React Native + Kotlin + Node.js + MongoDB  
**Status:** Proposed Technical Design

---

## 1. Technical Architecture

AddictCoin ko modular architecture par develop kiya jayega.

### 1.1 High-Level Architecture

```text
             ANDROID MOBILE APP
                     |
        React Native + TypeScript
                     |
          Native Android Modules
                 (Kotlin)
                     |
                REST APIs
                     |
            Node.js + Express
                     |
              Service Layer
                     |
             MongoDB Database
```

### 1.2 Architecture Components

**A. Mobile Application**

Responsibilities:
- User interface.
- Authentication screens.
- Wallet dashboard.
- Challenge management.
- App selection.
- Settings.
- Local state management.
- API communication.

**B. Native Android Modules**

Responsibilities:
- Step sensor integration.
- Camera integration.
- Location and GPS tracking.
- Installed application discovery.
- App usage monitoring.
- App access restriction.
- Phone call state detection.
- Background task management.

**C. Backend**

Responsibilities:
- Authentication.
- User management.
- Wallet management.
- Coin calculations.
- Transaction processing.
- Activity verification.
- App configuration.
- Cloud synchronization.

**D. Database**

MongoDB account data, wallet records, transactions, configurations aur activity history store karega.

---

## 2. Frontend Technical Requirements

### 2.1 Technology

- React Native.
- TypeScript.
- React Navigation.
- TanStack Query for server-state management.
- Zustand for lightweight application state.
- Axios for HTTP requests.
- React Hook Form for forms.

Libraries ki final selection compatibility testing ke baad hogi.

### 2.2 Main Application Screens

1. Splash Screen.
2. Login Screen.
3. Signup Screen.
4. Email Verification Screen.
5. Dashboard.
6. Coin Wallet.
7. Step Counter.
8. Location Challenge.
9. Study Mode.
10. Snake Game.
11. Phone-Free Challenge.
12. App Management.
13. Application Details.
14. Transaction History.
15. Settings.
16. User Profile.

### 2.3 Frontend Requirements

- Responsive Android UI.
- Reusable components.
- Centralized API client.
- Secure token storage.
- Loading and error states.
- Offline-state handling.
- Server-state synchronization.
- Permission onboarding.

### 2.4 Local Storage

Local storage ka use following information ke liye ho sakta hai:

- Authentication tokens.
- Cached user settings.
- Active challenge state.
- Pending synchronization records.
- Device-specific configuration.

Sensitive tokens ko encrypted platform-backed storage mein rakhna hoga.

---

## 3. Native Android Technical Requirements

Native functionality Kotlin modules ke through React Native ke saath integrate hogi.

### 3.1 Step Counter Module

**Technology:**
- Android SensorManager.
- TYPE_STEP_COUNTER.
- Health Connect, where supported.

Requirements:
- Step count read karna.
- Daily step progress calculate karna.
- Sensor availability check karna.
- Permission status check karna.
- Daily challenge progress maintain karna.
- Sensor reset aur device restart cases handle karna.

Step count ko daily baseline ke against calculate karna hoga.

### 3.2 Location Challenge Module

**Technology:**
- Google Maps SDK.
- Fused Location Provider.
- GPS.

Requirements:
- Current location obtain karna.
- Destination display karna.
- Walking route display karna.
- Location updates receive karna.
- Destination arrival verify karna.
- GPS accuracy check karna.
- Permission denial handle karna.

Location challenge ke dauran location access sirf required duration aur purpose ke liye use hoga.

### 3.3 Camera Study Mode

**Technology:**
- CameraX.
- Kotlin.
- Android lifecycle APIs.

Requirements:
- User consent ke baad camera initialize karna.
- Study session start/end control karna.
- Camera session active hone par indicator display karna.
- Camera data locally process karna.
- Recordings store ya upload nahi karna.

**Important Android limitation:**

Screen completely off hone par normal Android camera access ko continuously maintain karna reliable ya generally supported nahi hai.

MVP mein screen dimmed ya minimal display ke saath active foreground camera session ko fallback approach maana jayega.

Exact screen-off behavior ko implementation se pehle proof of concept se verify karna hoga.

### 3.4 Snake Game Module

**Technology:**
- React Native Canvas/Skia or native custom rendering.

Requirements:
- Game state management.
- Score calculation.
- Game start, pause and restart.
- Collision detection.
- Score submission.
- Reward limit enforcement.

Game result ko backend par validate karna hoga. Sirf client ke submitted score par unlimited rewards nahi diye jayenge.

### 3.5 Phone-Free Challenge Module

**Technology:**
- Android telephony state APIs, where available.
- UsageStatsManager.
- Android lifecycle and background scheduling APIs.

Requirements:
- Challenge start timestamp.
- Challenge duration.
- Phone call state handling.
- Call duration exclusion.
- Challenge recovery after app restart.
- Completion and failure detection.

Incoming aur outgoing calls ke dauran timer pause hoga aur call end hone ke baad resume hoga.

Call audio, messages ya conversation content access nahi kiya jayega.

Call-state API availability aur permissions Android version ke hisaab se validate karni hongi.

---

## 4. Application Access Control

### 4.1 Usage Monitoring

**Technology:** UsageStatsManager.

Requirements:
- Selected package names monitor karna.
- Foreground application detect karna.
- App usage duration calculate karna.
- Usage intervals ko process karna.
- Coin deductions calculate karna.

UsageStatsManager ke liye user ko Usage Access permission manually grant karni hogi.

### 4.2 App Blocking

Android par ordinary applications ko doosri applications ko completely disable karne ka unrestricted access nahi hota.

Isliye access-control implementation Android-supported mechanisms aur user-granted permissions ke through design karni hogi.

Possible approach:
- Foreground app monitoring.
- User-consented accessibility service, if justified and policy-compliant.
- Blocking screen or overlay, where permitted.
- Background monitoring and recovery.

App blocking ko tamper-proof guarantee nahi maana jayega.

### 4.3 Important Charging Limitation

Instagram ke liye `1 Reel = 1 Coin` jaise rules third-party app ke internal content ko reliably read kiye bina enforce karna possible nahi ho sakta.

UsageStatsManager generally app usage duration provide karta hai, individual Reel viewing events nahi.

Isliye MVP mein reliable charging ke liye:

- Instagram: per-minute charging.
- YouTube: per-minute charging.
- Other apps: configurable time-based charging.

Reel-based charging ko future feasibility investigation ke liye defer kiya jayega.

---

## 5. Backend Technical Requirements

### 5.1 Technology

- Node.js.
- Express.js.
- TypeScript.
- MongoDB.
- Mongoose.
- JWT.
- bcrypt or Argon2.
- Zod for request validation.

### 5.2 Backend Architecture

```text
src/
│
├── config/
│   ├── database.ts
│   ├── environment.ts
│   └── mail.ts
│
├── modules/
│   ├── auth/
│   ├── users/
│   ├── wallets/
│   ├── transactions/
│   ├── applications/
│   ├── settings/
│   ├── steps/
│   ├── locations/
│   ├── study/
│   ├── snake/
│   ├── phoneFree/
│   └── devices/
│
├── middleware/
│   ├── authenticate.ts
│   ├── authorize.ts
│   ├── validate.ts
│   ├── rateLimiter.ts
│   └── errorHandler.ts
│
├── services/
├── utils/
├── routes/
├── types/
└── server.ts
```

### 5.3 Backend Requirements

- REST API architecture.
- Centralized error handling.
- Request validation.
- Authentication middleware.
- Authorization middleware.
- Rate limiting.
- Logging.
- Database transaction support.
- Idempotent reward processing.
- Secure environment variables.

---

## 6. API Requirements

Base URL:

`/api/v1`

### 6.1 Authentication APIs

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /auth/register | Register user |
| POST | /auth/login | Login |
| POST | /auth/logout | Logout |
| POST | /auth/verify-email | Verify email |
| POST | /auth/refresh | Refresh token |
| POST | /auth/forgot-password | Forgot password |
| POST | /auth/reset-password | Reset password |

### 6.2 User APIs

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /users/me | Get profile |
| PATCH | /users/me | Update profile |

### 6.3 Wallet APIs

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /wallet | Get wallet balance |
| GET | /wallet/transactions | Transaction history |
| GET | /wallet/summary | Wallet summary |

### 6.4 Application APIs

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /apps/available | Get device app list |
| GET | /apps/controlled | Get controlled apps |
| POST | /apps/controlled | Add controlled app |
| PATCH | /apps/controlled/:id | Update app settings |
| DELETE | /apps/controlled/:id | Remove controlled app |
| POST | /apps/sync | Sync installed apps |

### 6.5 Settings APIs

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /settings | Get settings |
| PATCH | /settings | Update settings |

### 6.6 Step Challenge APIs

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /steps/challenges | Create daily challenge |
| GET | /steps/challenges/current | Current challenge |
| POST | /steps/challenges/:id/progress | Submit progress |
| POST | /steps/challenges/:id/complete | Complete challenge |

### 6.7 Location Challenge APIs

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /locations/challenges | Generate challenge |
| GET | /locations/challenges/current | Current challenge |
| POST | /locations/challenges/:id/progress | Submit progress |
| POST | /locations/challenges/:id/complete | Submit completion |

### 6.8 Study Mode APIs

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /study/sessions | Start session |
| GET | /study/sessions/:id | Get session |
| POST | /study/sessions/:id/complete | Complete session |
| POST | /study/sessions/:id/cancel | Cancel session |

### 6.9 Snake Game APIs

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /snake/sessions | Start game |
| POST | /snake/sessions/:id/complete | Submit score |
| GET | /snake/history | Game history |

### 6.10 Phone-Free APIs

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /phone-free/sessions | Start challenge |
| GET | /phone-free/sessions/current | Current challenge |
| POST | /phone-free/sessions/:id/pause | Pause challenge |
| POST | /phone-free/sessions/:id/resume | Resume challenge |
| POST | /phone-free/sessions/:id/complete | Complete challenge |

API endpoints proposed hain. Final request/response contracts API design phase mein define honge.

---

## 7. Authentication and Authorization

### Authentication Flow

1. User signup karega.
2. Password securely hash hoga.
3. Verification email send hogi.
4. Email verify hone ke baad login allow hoga.
5. Backend access token aur refresh token issue karega.
6. Protected APIs access token verify karengi.
7. Logout par refresh session revoke hogi.

### Security Requirements

- Password plaintext store nahi hoga.
- Refresh tokens securely manage honge.
- Token rotation implement hogi.
- Login rate limiting hogi.
- Email verification required hogi.
- Password reset tokens expire honge.

---

## 8. Coin Economy and Transaction Processing

### 8.1 Wallet Logic

Wallet mein:
- balance
- totalEarned
- totalSpent

Maintain honge.

### 8.2 Earning Flow

```text
User Completes Activity
          |
          v
Backend Receives Request
          |
          v
Validate Activity
          |
          v
Check Duplicate Reward
          |
          v
Calculate Reward
          |
          v
Create Transaction
          |
          v
Update Wallet Balance
          |
          v
Return Updated Balance
```

### 8.3 Spending Flow

```text
User Uses Controlled App
          |
          v
Usage Duration Calculated
          |
          v
Calculate Coin Cost
          |
          v
Check Wallet Balance
          |
       +--+--+
       |     |
   Enough   Not Enough
       |     |
   Deduct    Restrict
    Coins    Access
       |
       v
Update Wallet
```

### 8.4 Transaction Requirements

Wallet balance update aur transaction record creation ek MongoDB transaction mein perform hone chahiye.

MongoDB deployment ko transaction support karne wale replica set configuration par run karna hoga.

Duplicate reward requests ke liye unique idempotency key use hogi.

---

## 9. Multi-Device Synchronization

### Cloud Data

- User profile.
- Wallet.
- Transactions.
- App charging policies.
- Settings.
- Activity history.

### Device Data

- Installed applications.
- Device permissions.
- Local monitoring state.
- Local access-control status.

### Requirements

- Each device ke paas unique device ID hoga.
- Same account ke multiple device sessions allowed honge.
- Wallet backend ka single source of truth hoga.
- Concurrent spending atomic operations se handle hogi.
- Offline pending events server validation ke baad hi rewards earn karenge.

---

## 10. Performance and Reliability

- API responses predictable honi chahiye.
- Database indexes required queries ke according define honge.
- Background processing battery-efficient honi chahiye.
- Network failure handle hona chahiye.
- Active challenge state recoverable honi chahiye.
- Duplicate transactions prevent honi chahiye.
- Logs mein passwords aur access tokens store nahi hone chahiye.

---

## 11. Testing Requirements

### Frontend Testing
- Component testing.
- Navigation testing.
- Form validation.
- API integration testing.

### Backend Testing
- Authentication tests.
- Wallet transaction tests.
- Reward validation tests.
- API integration tests.
- Concurrent spending tests.

### Android Testing
- Sensor availability.
- GPS accuracy.
- Camera lifecycle.
- Usage Access permissions.
- App access restriction.
- Call interruption.
- Background execution.
- Battery consumption.

### Device Testing
Different Android versions aur manufacturers par testing required hogi.

---

## 12. Development Priorities

**Priority 1:**
- Project setup.
- Authentication.
- Database.
- Wallet.

**Priority 2:**
- App management.
- Usage tracking.
- Coin deduction.

**Priority 3:**
- Step Counter.
- Snake Game.

**Priority 4:**
- Study Mode.
- Phone-Free Challenge.
- Location Challenge.

**Priority 5:**
- Multi-device synchronization.
- Security.
- Integration testing.
- Release preparation.

Har major Android-dependent feature ka proof of concept full implementation se pehle banaya jayega.

---

## 13. Technical Completion Criteria

Technical implementation tab complete maani jayegi jab:

- Frontend aur backend successfully communicate karein.
- Authentication secure ho.
- Wallet transactions consistent hon.
- Duplicate rewards reject hon.
- Four earning features functional hon.
- App usage tracking work kare.
- Selected apps ke access restrictions work karein.
- Multiple devices par wallet sync ho.
- User permissions correctly handle hon.
- Critical Android compatibility tests pass hon.

**Final Technical Objective:** AddictCoin ko modular, secure, reliable aur scalable Android application ke roop mein implement karna.