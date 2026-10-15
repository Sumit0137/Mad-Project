QueueLess - Smart Queue Management System

**QueueLess** is an intelligent, Android-based Smart Queue Management System designed to eliminate long waiting lines and optimize counter operations across public and private institutions, including Hospitals, Banks, Colleges, and Government Offices.

By leveraging a dynamic priority aging algorithm, real-time queue position tracking, QR code verification, and location-based mapping, QueueLess ensures fair, efficient, and transparent queue scheduling for both service providers and visitors.

📱 Features

👤 User Features
- **Virtual Token Generation**: Easily book and generate queue tokens for various departments and service centers.
- **Dynamic Priority & Aging**: Automatic prioritization based on category (Emergency, Senior Citizen, Appointment, General) with continuous dynamic aging (+1 priority every 5 minutes in waiting queue).
- **Live Queue Tracking & ETA**: Real-time position tracking in the queue and estimated waiting time calculation.
- **QR Code Integration**: Instant token verification via embedded QR codes and direct counter QR scanner.
- **Interactive Service Map**: View nearest service centers, hospitals, banks, and offices using integrated Google Maps & Location services.
- **Token History & Status Tracking**: Detailed log of past and active tokens with complete audit history.
- **Local Secure Authentication**: Password hashing with PBKDF2/SHA-256 and salt for local account security.

 🛡️ Admin & Counter Management
- **Dashboard & Token Calling**: Efficient queue calling mechanism obeying priority and FCFS tie-breaking rules.
- **Counter & Service Management**: Add, update, or remove services, department details, and average service durations.
- **Status Lifecycle Handling**: Progress tokens through status states (`WAITING` ➔ `SERVING` ➔ `COMPLETED` / `CANCELLED` / `SKIPPED`).

---

⚙️ Queue Scheduling Algorithm (`QueueScheduler`)

QueueLess replaces traditional First-Come-First-Served (FCFS) queues with a **Dynamic Priority Aging Scheduler**:

 1. Base Priority Assignment
| Priority Category | Base Priority | Typical Use Cases |
| :--- | :---: | :--- |
| **Emergency** | `4` | Urgent medical cases, critical requests |
| **Senior Citizen / Special Assistance** | `3` | Elderly, disabled, or priority assistance |
| **Appointment** | `2` | Pre-scheduled appointments |
| **General / Standard** | `1` | Regular walk-in tickets |

 2. Dynamic Priority Aging Formula
To prevent lower-priority tokens from waiting indefinitely, the system continuously recalculates effective priority based on wait duration:

$$\text{Effective Priority} = \text{Base Priority} + \left\lfloor \frac{\text{Current Time} - \text{Arrival Time}}{5 \text{ minutes}} \right\rfloor$$

- Every **5 minutes** spent in the `WAITING` state adds **+1** to the token's effective priority.
- Tokens with equal effective priorities are resolved using **FCFS** (earliest arrival time first).



 📂 Project Architecture & Directory Structure

text
Project/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/queueless/app/
│   │   │   │   ├── activities/      # App screens (Splash, Login, Main, Token, Admin, QR)
│   │   │   │   ├── adapters/        # RecyclerView adapters for Services, Tokens, and History
│   │   │   │   ├── algorithm/       # QueueScheduler dynamic priority engine
│   │   │   │   ├── auth/            # LocalAuthManager with PBKDF2 hashing
│   │   │   │   ├── database/        # DatabaseHelper (SQLite database & migrations)
│   │   │   │   ├── fragments/       # Home, Queue, Map, Profile UI fragments
│   │   │   │   ├── models/          # User, ServiceModel, Token, QueueHistory
│   │   │   │   └── utils/           # AssetReader, QrCodeUtils, NotificationHelper, PreferenceUtils
│   │   │   ├── assets/              # Default service catalog (services.csv) & queue_rules.txt
│   │   │   ├── res/                 # Layouts, drawables, navigation, values, themes
│   │   │   └── AndroidManifest.xml
│   │   └── test/                    # Robolectric & JUnit test suite for QueueScheduler
│   └── build.gradle.kts
├── build.gradle.kts
└── README.md


🛠️ Tech Stack & Dependencies

- **Language**: Java 11
- **Target SDK**: 34 | **Min SDK**: 24
- **Architecture**: Single-Activity Multiple-Fragments pattern (`MainActivity` + `BottomNavigationView`)
- **Database**: Local SQLite database with transaction safety and version upgrades
- **Libraries & SDKs**:
  - `AndroidX AppCompat`, `RecyclerView`, `CardView`, `ConstraintLayout`
  - `Google Play Services Maps` (`18.2.0`) & `Location` (`21.1.0`)
  - `ZXing Android Embedded` (`4.3.0`) & `ZXing Core` (`3.5.3`) for QR generation & scanning
  - `Material Design 3`
- **Testing**:
  - `JUnit 4`
  - `Robolectric` (`4.12.2`) - Headless Android environment unit testing with JDK 21 launcher toolchain
  - `Mockito` (`5.11.0`)
  - `Espresso` (`3.5.1`)

 🚀 Getting Started
 Prerequisites
- **Android Studio** (Hedgehog 2023.1.1 or newer recommended)
- **JDK 17 or JDK 21** configured in Android Studio
- Android SDK 34 installed via SDK Manager
 Setup & Installation
1. **Clone or Open the Project**:
   Open Android Studio and select **File ➔ Open**, then navigate to the `Project` directory.

2. **Configure Google Maps API Key**:
   Replace the placeholder key in `app/src/main/res/values/strings.xml`:
   ```xml
   <string name="google_maps_key">YOUR_GOOGLE_MAPS_API_KEY</string>
   ```

3. **Build & Sync Gradle**:
   Gradle will automatically sync and download all required dependencies.

4. **Run Unit Tests**:
   To run unit tests for the scheduling algorithm and database rules:
   ```bash
   ./gradlew test
   ```

5. **Deploy & Run**:
   Select an Android Emulator or connected physical device (Android 7.0 / API 24+) and click **Run 'app'**.


 📝 Usage Guide

 User Flow
1. **Register / Login**: Sign up as a regular user or log in with existing credentials.
2. **Select Service**: Choose a service counter from Hospitals, Banks, Colleges, or Government Offices on the **Home** tab or scan a desk QR code.
3. **Generate Token**: Select priority category (General, Senior Citizen, Appointment, Emergency) to receive a virtual token card with QR code.
4. **Track Live Queue**: View live position, people ahead, and calculated ETA on the **Queue** fragment.
5. **Map Navigation**: Switch to the **Map** tab to locate service counters on Google Maps.
 Admin Flow
1. **Register / Login as Admin**: Select the **Admin** role during registration or log in with admin credentials.
2. **Manage Services**: Add, edit, or remove service counters and adjust average handling times.
3. **Call Next Token**: Call waiting tokens in priority order from the **Admin Dashboard**.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
