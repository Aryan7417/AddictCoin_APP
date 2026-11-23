# ADDICTCOIN
## MongoDB Backend Database Schema

**Version:** 1.0  
**Database:** MongoDB  
**ODM:** Mongoose  
**Backend:** Node.js + Express.js + TypeScript

---

## 1. Database Design Overview

Database ka design user-centric hoga.

Har registered user ka:
- Ek wallet.
- Multiple device sessions.
- Multiple controlled applications.
- Ek settings document.
- Multiple activity records.
- Multiple coin transactions.

honge.

### Proposed Collections

| Collection | Purpose |
|---|---|
| users | User accounts |
| wallets | Current coin balance |
| wallet_transactions | Coin earning and spending |
| device_sessions | Logged-in devices |
| controlled_apps | User app charging policies |
| device_apps | Device installed app records |
| user_settings | Custom settings |
| step_challenges | Daily step challenges |
| location_challenges | Location challenges |
| study_sessions | Study records |
| snake_sessions | Snake game records |
| phone_free_sessions | Phone-free challenge records |

---

## 2. Users Collection

Collection: `users`

```javascript
{
  _id: ObjectId,

  name: String,

  email: {
    type: String,
    unique: true,
    lowercase: true
  },

  passwordHash: String,

  isEmailVerified: Boolean,

  profileImage: String,

  accountStatus: {
    type: String,
    enum: ["active", "suspended", "deleted"]
  },

  createdAt: Date,
  updatedAt: Date
}
```

### Indexes

- Unique index on email.
- Index on accountStatus.

---

## 3. Wallets Collection

Collection: `wallets`

Har user ka ek wallet hoga.

```javascript
{
  _id: ObjectId,

  userId: {
    type: ObjectId,
    ref: "User",
    unique: true
  },

  balance: Number,

  totalEarned: Number,

  totalSpent: Number,

  currency: {
    type: String,
    default: "COIN"
  },

  createdAt: Date,
  updatedAt: Date
}
```

### Business Rules

- Balance negative nahi ho sakta.
- Balance backend par update hoga.
- Client directly wallet balance change nahi kar sakta.
- Wallet transactions ke saath atomic update hoga.

---

## 4. Wallet Transactions Collection

Collection: `wallet_transactions`

Ye collection financial-style ledger ki tarah work karegi.

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  walletId: ObjectId,

  type: {
    type: String,
    enum: ["EARN", "SPEND", "ADJUSTMENT"]
  },

  source: {
    type: String,
    enum: [
      "STEP_CHALLENGE",
      "LOCATION_CHALLENGE",
      "STUDY_MODE",
      "SNAKE_GAME",
      "PHONE_FREE",
      "APP_USAGE",
      "ADMIN_ADJUSTMENT"
    ]
  },

  amount: Number,

  balanceBefore: Number,

  balanceAfter: Number,

  referenceId: ObjectId,

  idempotencyKey: String,

  description: String,

  createdAt: Date
}
```

### Indexes

- `{ userId: 1, createdAt: -1 }`
- Unique `{ userId: 1, idempotencyKey: 1 }`
- `{ referenceId: 1 }`

### Example: Coin Earn

```javascript
{
  type: "EARN",
  source: "STUDY_MODE",
  amount: 20,
  balanceBefore: 50,
  balanceAfter: 70
}
```

### Example: Coin Spend

```javascript
{
  type: "SPEND",
  source: "APP_USAGE",
  amount: 5,
  balanceBefore: 70,
  balanceAfter: 65
}
```

### Important Rules

- Transactions immutable honge.
- Existing transaction ko directly edit nahi karna.
- Correction ke liye separate adjustment transaction create hogi.
- Wallet aur ledger update ek database transaction mein honge.

---

## 5. Device Sessions Collection

Collection: `device_sessions`

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  deviceId: String,

  deviceName: String,

  platform: "ANDROID",

  androidVersion: String,

  appVersion: String,

  refreshTokenHash: String,

  lastActiveAt: Date,

  isActive: Boolean,

  createdAt: Date,
  expiresAt: Date
}
```

### Purpose

- Multiple device login.
- Session management.
- Token revocation.
- Device-level synchronization.

Refresh token ka raw value database mein store nahi hoga.

---

## 6. Controlled Apps Collection

Collection: `controlled_apps`

Ye user ke cloud-based app charging rules maintain karegi.

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  packageName: String,

  appName: String,

  isControlled: Boolean,

  chargingRule: {
    unit: {
      type: String,
      enum: ["MINUTE", "SESSION"]
    },

    coinsPerUnit: Number,

    intervalSeconds: Number
  },

  dailyUsageLimitMinutes: Number,

  createdAt: Date,
  updatedAt: Date
}
```

### Example

```javascript
{
  packageName: "com.instagram.android",

  appName: "Instagram",

  isControlled: true,

  chargingRule: {
    unit: "MINUTE",
    coinsPerUnit: 1,
    intervalSeconds: 60
  },

  dailyUsageLimitMinutes: 120
}
```

### Important

Charging rules account-level hongi. Installed application ki actual availability device-specific hogi.

---

## 7. Device Apps Collection

Collection: `device_apps`

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  deviceId: String,

  packageName: String,

  appName: String,

  isInstalled: Boolean,

  localAccessControlEnabled: Boolean,

  permissionStatus: {
    usageAccess: Boolean,
    accessibility: Boolean
  },

  lastSyncedAt: Date,

  createdAt: Date,
  updatedAt: Date
}
```

### Purpose

Same user ke different devices par installed apps ka status separately maintain karna.

---

## 8. User Settings Collection

Collection: `user_settings`

```javascript
{
  _id: ObjectId,

  userId: {
    type: ObjectId,
    unique: true
  },

  theme: {
    type: String,
    enum: ["LIGHT", "DARK", "SYSTEM"]
  },

  notificationsEnabled: Boolean,

  challenges: {
    stepChallenge: {
      enabled: Boolean,
      allowedTargets: [Number],
      dailyReward: Number
    },

    locationChallenge: {
      enabled: Boolean,
      destinationDistanceMeters: Number,
      reward: Number
    },

    studyMode: {
      enabled: Boolean,
      sessionDurationMinutes: Number,
      rewardPerMinute: Number
    },

    snakeGame: {
      enabled: Boolean,
      maxRewardPerGame: Number,
      dailyRewardLimit: Number
    },

    phoneFree: {
      enabled: Boolean,
      durationMinutes: Number,
      reward: Number
    }
  },

  createdAt: Date,
  updatedAt: Date
}
```

### Validation

Settings ke liye backend par allowed ranges define honi chahiye.

Client arbitrary rewards ya unlimited earning caps set nahi kar sakta.

---

## 9. Step Challenges Collection

Collection: `step_challenges`

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  deviceId: String,

  challengeDate: String,

  targetSteps: Number,

  currentSteps: Number,

  rewardCoins: Number,

  status: {
    type: String,
    enum: [
      "ACTIVE",
      "COMPLETED",
      "FAILED",
      "EXPIRED"
    ]
  },

  startedAt: Date,

  completedAt: Date,

  rewardTransactionId: ObjectId,

  createdAt: Date,
  updatedAt: Date
}
```

### Important Rules

- Daily challenge ke duplicate records prevent hon.
- Step target server-side generate hoga.
- Progress validation required hogi.
- Completed challenge par reward sirf ek baar milega.

---

## 10. Location Challenges Collection

Collection: `location_challenges`

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  deviceId: String,

  destination: {
    latitude: Number,
    longitude: Number,
    name: String
  },

  startingPoint: {
    latitude: Number,
    longitude: Number
  },

  configuredDistanceMeters: Number,

  rewardCoins: Number,

  status: {
    type: String,
    enum: [
      "ACTIVE",
      "COMPLETED",
      "FAILED",
      "EXPIRED"
    ]
  },

  startedAt: Date,

  completedAt: Date,

  rewardTransactionId: ObjectId,

  createdAt: Date,
  updatedAt: Date
}
```

### Privacy Rules

- Unnecessary location history store nahi hogi.
- Location data required verification purpose ke liye hi maintain hogi.
- Location retention policy define hogi.

---

## 11. Study Sessions Collection

Collection: `study_sessions`

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  deviceId: String,

  plannedDurationMinutes: Number,

  actualDurationSeconds: Number,

  status: {
    type: String,
    enum: [
      "ACTIVE",
      "COMPLETED",
      "CANCELLED",
      "INTERRUPTED"
    ]
  },

  startedAt: Date,

  completedAt: Date,

  rewardCoins: Number,

  rewardTransactionId: ObjectId,

  createdAt: Date,
  updatedAt: Date
}
```

### Important Rules

- Session start server par register hoga.
- Session duration validate hogi.
- Camera recordings database mein store nahi hongi.
- Duplicate reward claims prevent honge.

---

## 12. Snake Game Sessions Collection

Collection: `snake_sessions`

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  deviceId: String,

  score: Number,

  durationSeconds: Number,

  rewardCoins: Number,

  status: {
    type: String,
    enum: ["STARTED", "COMPLETED", "REJECTED"]
  },

  startedAt: Date,

  completedAt: Date,

  rewardTransactionId: ObjectId,

  createdAt: Date
}
```

### Important Rules

- Game session server-generated ID ke saath start hoga.
- Maximum reward limit apply hogi.
- Score validation required hogi.
- Invalid submissions reject hongi.

---

## 13. Phone-Free Sessions Collection

Collection: `phone_free_sessions`

```javascript
{
  _id: ObjectId,

  userId: ObjectId,

  deviceId: String,

  plannedDurationMinutes: Number,

  accumulatedPhoneCallSeconds: Number,

  activeDurationSeconds: Number,

  status: {
    type: String,
    enum: [
      "ACTIVE",
      "PAUSED",
      "COMPLETED",
      "FAILED",
      "CANCELLED"
    ]
  },

  startedAt: Date,

  completedAt: Date,

  rewardCoins: Number,

  rewardTransactionId: ObjectId,

  createdAt: Date,
  updatedAt: Date
}
```

### Important Rules

- Call duration challenge duration se exclude hogi.
- Incoming aur outgoing calls ke dauran timer pause hoga.
- Call end hone ke baad challenge resume hoga.
- App restart hone par challenge recover hona chahiye.
- Reward duplicate nahi milna chahiye.

---

# 14. Database Relationships

```text
USER
 |
 |---- WALLET
 |       |
 |       |---- WALLET_TRANSACTIONS
 |
 |---- DEVICE_SESSIONS
 |
 |---- CONTROLLED_APPS
 |
 |---- DEVICE_APPS
 |
 |---- USER_SETTINGS
 |
 |---- STEP_CHALLENGES
 |
 |---- LOCATION_CHALLENGES
 |
 |---- STUDY_SESSIONS
 |
 |---- SNAKE_SESSIONS
 |
 |---- PHONE_FREE_SESSIONS
```

---

# 15. Important Database Indexes

| Collection | Index |
|---|---|
| users | Unique email |
| wallets | Unique userId |
| wallet_transactions | userId + createdAt |
| wallet_transactions | Unique userId + idempotencyKey |
| controlled_apps | Unique userId + packageName |
| device_apps | Unique deviceId + packageName |
| device_sessions | userId + deviceId |
| user_settings | Unique userId |
| step_challenges | userId + challengeDate |
| location_challenges | userId + status |
| study_sessions | userId + startedAt |
| snake_sessions | userId + createdAt |
| phone_free_sessions | userId + status |

Unique indexes aur query indexes implementation se pehle actual access patterns ke according finalize honge.

---

# 16. Wallet Transaction Workflow

Wallet balance aur transaction record ko hamesha ek atomic operation mein update karna hai.

### Example: User Earns 20 Coins

Initial balance: 100 coins.

1. Backend activity validate karega.
2. Duplicate reward check karega.
3. Wallet document read karega.
4. 20 coins add karega.
5. Transaction document create karega.
6. Database transaction commit karega.
7. Updated balance return karega.

Final balance: 120 coins.

### Example: User Spends 10 Coins

Initial balance: 120 coins.

1. Backend current balance check karega.
2. Insufficient balance hone par request reject hogi.
3. Wallet se 10 coins deduct honge.
4. Spending transaction create hogi.
5. Database transaction commit hoga.

Final balance: 110 coins.

### Critical Rule

Wallet aur transaction records ke updates ke liye MongoDB sessions aur transactions use karne hain.

---

# 17. Data Validation Rules

- Coin amount positive integer hona chahiye.
- Wallet balance negative nahi ho sakta.
- User sirf apna data access kar sakta hai.
- Duplicate challenge reward reject hoga.
- Duplicate transaction reject hogi.
- Invalid package names reject honge.
- Challenge duration allowed range mein honi chahiye.
- Location coordinates valid honi chahiye.
- Game score configured limits ke andar hona chahiye.
- User-provided reward amount directly trust nahi karna hai.

---

# 18. Database Security

- Password hashes hi store honge.
- Refresh tokens hashed form mein store honge.
- MongoDB credentials environment variables mein honge.
- Database network access restricted hoga.
- Sensitive data logs mein expose nahi hogi.
- User ownership checks har protected query par honge.
- Wallet updates backend-only honge.
- Database backups aur retention policy define hogi.

---

# 19. Final Database Design Principles

1. Wallet balance backend ka source of truth hoga.
2. Transactions immutable ledger records hongi.
3. User data userId ke through isolated rahega.
4. Account-level configurations aur device-level configurations separate rahengi.
5. Activity records future analysis ke liye maintain honge.
6. Reward calculation server-side hogi.
7. MongoDB transactions financial consistency maintain karengi.
8. Camera recordings database mein kabhi store nahi hongi.

**Final Objective:** Ek secure, consistent aur multi-device compatible MongoDB backend develop karna jo AddictCoin ki four earning activities aur coin economy ko support kare.