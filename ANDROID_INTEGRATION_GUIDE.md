# RevaMine Native Android App Integration Guide

> **To the Android Developer:**
> This document outlines the architectural contract, Firebase database schema, survey/offerwall integration flows, and security rules required to integrate RevaMine into the native Android application (Kotlin / Jetpack Compose) without database mismatches or security vulnerabilities.

---

## 1. Core Architecture Principles

1. **Shared Backend (`api.revamine.com` / Node.js Express)**:
   - All survey API fetching (TimeWall, CPX Research, TheoremReach) and postback processing happen **server-side** in Node.js.
   - The Android app **NEVER** credits points directly from client code. All points crediting is handled automatically via secure server-to-server webhook postbacks (`/api/timewall-postback`, `/api/cpx-postback`).
2. **Shared Database (Firebase Firestore & Auth)**:
   - The Android app must connect to the **exact same Firebase Project** (`google-services.json`) as the web platform.
   - User sessions (`FirebaseAuth`) and Firestore documents (`users/{userId}`) are fully shared between Web and Android.

---

## 2. Firebase Configuration & Authentication

- **`google-services.json`**: Place the official Firebase configuration file in the Android app's `app/` module.
- **Auth State**: Use `FirebaseAuth.getInstance().currentUser` to retrieve the authenticated `uid`. This `uid` is the primary key used across Firestore documents and survey provider callbacks (`ext_user_id` / `userId`).

---

## 3. Database Schema Contract (Firestore)

To prevent database mismatches, adhere strictly to these Firestore collections and field structures:

### A. User Document (`users/{userId}`)
```kotlin
data class UserProfile(
    val uid: String,
    val email: String,
    val pointsBalance: Long,          // Withdrawable points
    val pendingSurveyBalance: Long,   // 72-hour security escrow balance
    val totalSurveysCompleted: Long,
    val isAdmin: Boolean,
    val isAdvertiser: Boolean,
    val updatedAt: Timestamp
)
```

### B. Pending Survey Holds (`pending_survey_holds/{holdId}`)
- Created automatically by backend postback handlers.
- Automatically unlocks after 72 hours.
```kotlin
data class SurveyHold(
    val id: String,
    val transId: String,
    val userId: String,
    val points: Long,
    val partner: String,              // "TimeWall", "CPX Research", etc.
    val status: String,               // "pending", "unlocked", "reversed"
    val unlockAt: Timestamp,
    val createdAt: Timestamp
)
```

---

## 4. Survey & Offerwall Integration (WebViews / Custom Tabs)

When a user taps a survey card in the Android app:

1. **Construct the Survey URL**:
   - Fetch the survey link from the backend or construct it using the user's Firebase UID:
     ```kotlin
     val surveyUrl = "https://web.timesurveys.com/privacy-policy/cus_.../sur_...?uid=${firebaseUser.uid}"
     ```
2. **Open in Custom Tabs / WebView**:
   - Open the URL in Android **Chrome Custom Tabs** (recommended for best cookie and redirect handling) or an internal WebView with JavaScript enabled.
3. **Handling Return Redirects**:
   - Listen for redirects back to your app's deep link or return URL (`https://revamine.com/surveys?status=success` or custom app scheme like `revamine://surveys`).
   - When detected, trigger a UI refresh of the user's wallet balance from Firestore.

---

## 5. Security & Fraud Prevention Checklist

- **No Client-Side Point Crediting**: The Android app must never increment `pointsBalance` locally upon survey completion. Always wait for the server-side webhook postback to update Firestore, and observe balance changes via real-time Firestore Snapshot Listeners (`users/{userId}`).
- **SHA-256 Postback Signature**: Ensure all postbacks hitting your backend (`server.ts`) validate the cryptographic secret key (`TIMEWALL_SECRET_KEY`) to prevent URL parameter tampering or fake postback injections.
