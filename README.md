# 📱 SmartQuiz Pro — Complete Setup Guide
### Flutter + Firebase + Android Studio

---

## 📁 Project Structure

```
smartquiz_pro/
├── lib/
│   ├── main.dart                    ← App entry point
│   ├── firebase_options.dart        ← Firebase config (YOU MUST EDIT THIS)
│   ├── theme/
│   │   └── app_theme.dart           ← Colors, fonts, light/dark theme
│   ├── models/
│   │   └── models.dart              ← User, Quiz, Question, Result models
│   ├── services/
│   │   ├── auth_service.dart        ← Firebase Auth (login, signup, Google)
│   │   ├── quiz_service.dart        ← Firestore CRUD operations
│   │   └── app_provider.dart        ← State management (Provider)
│   └── screens/
│       ├── splash_screen.dart       ← Splash with logo animation
│       ├── login_screen.dart        ← Login + Google Sign-In
│       ├── signup_screen.dart       ← Registration
│       ├── home_screen.dart         ← Dashboard + Bottom Nav
│       ├── quiz_screen.dart         ← Quiz with timer + progress
│       ├── result_screen.dart       ← Score + badges + AI recommendation
│       ├── other_screens.dart       ← Analytics, Leaderboard, Chatbot, Profile
│       ├── admin_panel_screen.dart  ← Admin: add/edit/delete questions & users
│       └── settings_screen.dart     ← Dark mode, notifications, logout
├── android/
│   ├── app/build.gradle             ← Android build config
│   └── build.gradle                 ← Project-level gradle
├── firestore.rules                  ← Firestore security rules
└── pubspec.yaml                     ← All Flutter packages
```

---

## ✅ STEP 1 — Install Flutter & Android Studio

### Flutter Install:
1. Download Flutter SDK: https://flutter.dev/docs/get-started/install
2. Extract and add `flutter/bin` to your PATH
3. Run: `flutter doctor` — fix any issues shown

### Android Studio Install:
1. Download: https://developer.android.com/studio
2. Install Android SDK (API level 33+)
3. Install Flutter plugin:
   - Android Studio → Settings → Plugins → Search "Flutter" → Install
   - Also install "Dart" plugin
4. Restart Android Studio

---

## ✅ STEP 2 — Setup Firebase Project

### A. Create Firebase Project:
1. Go to: https://console.firebase.google.com
2. Click **"Add Project"**
3. Enter name: `SmartQuizPro` → Continue
4. Disable Google Analytics (optional) → Create Project

### B. Enable Authentication:
1. Firebase Console → **Authentication** → Get Started
2. Sign-in method → Enable:
   - ✅ **Email/Password**
   - ✅ **Google**

### C. Create Firestore Database:
1. Firebase Console → **Firestore Database** → Create Database
2. Choose **"Start in test mode"** (for development)
3. Select your region → Done

### D. Add Android App to Firebase:
1. Firebase Console → Project Overview → **Add App** → Android icon
2. Android package name: `com.example.smartquiz_pro`
3. App nickname: `SmartQuiz Pro`
4. Click **Register App**
5. Download **`google-services.json`**
6. Place it in: `android/app/google-services.json`
7. Click Next → Next → Continue to Console

### E. Copy Firebase Config to flutter:
Open `lib/firebase_options.dart` and replace the placeholder values
with values from: Firebase Console → Project Settings → General → Your Apps

```dart
// Example (replace with YOUR values):
static const FirebaseOptions android = FirebaseOptions(
  apiKey: 'AIzaSyABC123...',          // ← your key
  appId: '1:123456789:android:abc',   // ← your app id
  messagingSenderId: '123456789',     // ← your sender id
  projectId: 'smartquizpro-abc12',    // ← your project id
  storageBucket: 'smartquizpro-abc12.appspot.com',
);
```

### F. Enable Firebase Storage:
1. Firebase Console → **Storage** → Get Started → Done

---

## ✅ STEP 3 — Open Project in Android Studio

1. Open Android Studio
2. **File → Open** → Select the `smartquiz_pro` folder
3. Wait for Gradle to sync (may take 2-5 minutes)
4. If Gradle sync fails, go to **File → Sync Project with Gradle Files**

---

## ✅ STEP 4 — Install Flutter Packages

Open Terminal in Android Studio (bottom bar) and run:

```bash
flutter pub get
```

If you see errors, try:
```bash
flutter clean
flutter pub get
```

---

## ✅ STEP 5 — Setup Android Emulator or Physical Device

### Option A — Android Emulator:
1. Android Studio → **Device Manager** (right sidebar)
2. Click **"Create Device"**
3. Choose: **Pixel 6** → Next
4. Select System Image: **API 33** (download if needed) → Next
5. Click **Finish**
6. Press ▶ Play button to start emulator

### Option B — Physical Android Phone:
1. Phone Settings → About Phone → Tap "Build Number" 7 times
2. Settings → Developer Options → Enable **USB Debugging**
3. Connect phone to PC via USB cable
4. Allow USB debugging when prompted
5. Phone should appear in Android Studio device list

---

## ✅ STEP 6 — Run the App

1. Select your device from the dropdown at the top of Android Studio
2. Click the **▶ Run** button (or press `Shift + F10`)
3. Wait 1-2 minutes for first build
4. App will open on device/emulator!

---

## ✅ STEP 7 — Upload Sample Questions to Firestore

In Firebase Console → Firestore Database → Start a Collection:

### Collection: `questions`
Add documents with these fields:
```
questionId : "q1"
quizId     : "cs_001"
question   : "Which data structure uses LIFO?"
options    : ["Queue", "Stack", "Linked List", "Tree"]
correctIndex: 1
difficulty : "medium"
explanation: "Stack follows Last-In-First-Out principle."
```

Repeat for more questions. The app also has built-in sample questions
as fallback so it works even without Firestore data.

---

## ✅ STEP 8 — Make Someone Admin

In Firestore → `users` collection → find the user document → add field:
```
role : "admin"
```

That user can now access Admin Panel via Settings screen.

---

## 🗄️ Firestore Database Structure

```
Firestore/
├── users/
│   └── {userId}/
│       ├── userId         : string
│       ├── name           : string
│       ├── email          : string
│       ├── profileImage   : string (URL)
│       ├── totalScore     : number
│       ├── quizzesAttempted: number
│       ├── rank           : number
│       ├── streak         : number
│       ├── badges         : array
│       ├── role           : string ("user" or "admin")
│       └── createdAt      : timestamp
│
├── quizzes/
│   └── {quizId}/
│       ├── quizId         : string
│       ├── category       : string
│       ├── difficulty     : string ("easy"/"medium"/"hard")
│       ├── title          : string
│       ├── totalQuestions : number
│       ├── timePerQuestion: number (seconds)
│       ├── totalPoints    : number
│       └── iconEmoji      : string
│
├── questions/
│   └── {questionId}/
│       ├── questionId     : string
│       ├── quizId         : string
│       ├── question       : string
│       ├── options        : array [A, B, C, D]
│       ├── correctIndex   : number (0-3)
│       ├── explanation    : string
│       └── difficulty     : string
│
└── results/
    └── {resultId}/
        ├── resultId       : string
        ├── userId         : string
        ├── quizId         : string
        ├── category       : string
        ├── score          : number
        ├── totalQuestions : number
        ├── correctAnswers : number
        ├── wrongAnswers   : number
        ├── percentage     : number
        ├── grade          : string
        ├── timeTaken      : number (seconds)
        └── date           : timestamp
```

---

## 📱 App Screens & Features

| Screen | File | Features |
|--------|------|----------|
| Splash | `splash_screen.dart` | Animated logo, auto-navigate |
| Login | `login_screen.dart` | Email + Google Sign-In, Forgot Password |
| Signup | `signup_screen.dart` | Full registration with validation |
| Home | `home_screen.dart` | Dashboard, Daily Challenge, Streak, Categories |
| Quiz | `quiz_screen.dart` | Timer, Progress bar, MCQ options, Nav |
| Result | `result_screen.dart` | Score, Grade, Badges, AI Recommendation |
| Analytics | `other_screens.dart` | Bar chart, Subject bars, Pie chart |
| Leaderboard | `other_screens.dart` | Podium (Top 3), Full ranking list |
| AI Chatbot | `other_screens.dart` | Chat UI, Quick suggestions |
| Profile | `other_screens.dart` | Stats, Badges, Settings links |
| Settings | `settings_screen.dart` | Dark mode, Notifications, Admin access |
| Admin Panel | `admin_panel_screen.dart` | Add/Edit/Delete questions, User management |

---

## 🎨 Design System

| Element | Value |
|---------|-------|
| Primary Color | `#6C5CE7` (Purple) |
| Secondary Color | `#00CEC9` (Teal) |
| Accent Color | `#FD79A8` (Pink) |
| Success | `#00B894` (Green) |
| Danger | `#D63031` (Red) |
| Warning | `#E17055` (Orange) |
| Font | Poppins |
| Border Radius | 14–20px |
| Design System | Material 3 |

---

## 🏆 Achievement Badges

| Badge | Condition |
|-------|-----------|
| 🎯 Perfect Score | Score 100% |
| ⭐ High Score | Score ≥ 80% |
| 🏆 Quiz Master | Complete 50 quizzes |
| 🔥 7-Day Streak | 7 consecutive days |
| ⚡ Speed King | Finish quiz in under 2 minutes |
| 📚 Knowledge Champion | 90%+ in 3 categories |

---

## 🐛 Common Problems & Solutions

### ❌ "google-services.json not found"
→ Download from Firebase Console and place in `android/app/` folder

### ❌ "Gradle build failed"
→ Run `flutter clean` then `flutter pub get`, then try again

### ❌ "minSdkVersion" error
→ Open `android/app/build.gradle` → change `minSdkVersion` to `21`

### ❌ "Firebase not initialized"
→ Make sure `firebase_options.dart` has your real project values

### ❌ Google Sign-In not working
→ Add SHA-1 fingerprint to Firebase:
```bash
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```
Copy SHA-1 → Firebase Console → Project Settings → Add fingerprint

### ❌ App runs but no questions
→ Normal! App uses built-in sample data. Add questions via Admin Panel
   or directly in Firestore Console.

---

## 📦 Packages Used

| Package | Purpose |
|---------|---------|
| `firebase_core` | Firebase initialization |
| `firebase_auth` | User authentication |
| `cloud_firestore` | Database |
| `firebase_storage` | Image uploads |
| `google_sign_in` | Google OAuth |
| `provider` | State management |
| `fl_chart` | Analytics charts |
| `animate_do` | Screen animations |
| `lottie` | Lottie animations |
| `shimmer` | Loading skeletons |
| `cached_network_image` | Profile pictures |
| `image_picker` | Pick profile photo |
| `shared_preferences` | Local settings |
| `intl` | Date formatting |
| `connectivity_plus` | Network check |

---

## 🚀 Future Enhancements

- [ ] Voice-based quiz (text-to-speech)
- [ ] PDF certificate download
- [ ] Push notifications (FCM)
- [ ] Offline mode support
- [ ] Social sharing of scores
- [ ] In-app study notes/PDF viewer
- [ ] Real AI chatbot integration (OpenAI API)
- [ ] Quiz timer with sound effects

---

## 👨‍💻 Developer Notes

- All screens use **StatelessWidget** or **StatefulWidget** with **Provider** for state
- Firebase calls have try-catch with fallback sample data
- Quiz results auto-save to Firestore after completion
- Dark mode is toggled via `AppProvider.toggleTheme()`
- Admin access is role-based (field `role: "admin"` in Firestore)

---

*SmartQuiz Pro — Built with Flutter 3.x + Firebase + Material 3*
*Class Project | Final Year Submission Ready*
