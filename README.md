# IRIS — Daily Journal Android App

IRIS is a warm, privacy-first journaling companion designed to help people process their days with compassion. Each entry captures feelings, highlights, challenges, and photos, while the in-app "IRIS" friend reflects back with encouraging insights. Everything lives locally on the device, with optional exports when you’re ready to take your memories with you.

## ✨ Key Concepts
- **Supportive journaling**: Capture text, moods, highlights, challenges, and attach images to remember the details. New entries are saved locally with soft prompts that encourage gentle reflection.
- **IRIS reflections**: Generate gentle summaries (what went well, what felt heavy, emotional insight) for every entry using on-device heuristics. Regenerate anytime from the IRIS companion screen.
- **Warm, vibrant UI**: Sunset oranges, soft corals, and twilight purples paired with rounded cards, soft shadows, and thoughtful animations across the timeline, editor, and companion screens.
- **Privacy-first storage**: Entries, reflections, and media are stored locally via Room and app-specific storage. Attachments are copied into private storage and cleaned up when entries change.
- **Flexible exports** *(early preview)*: A background WorkManager task prepares monthly text archives you can manually export today; PDF + zipped history remain on the roadmap.

## 🧱 Tech Stack
- **Language**: Kotlin 1.9+
- **UI**: Jetpack Compose + Material 3
- **Architecture**: MVVM + Repository pattern, Navigation Compose
- **DI**: Hilt
- **Persistence**: Room + app-specific storage
- **Async**: Coroutines & Flow
- **Media & exports**: Coil for images, PdfDocument + Zip utilities for backups
- **Work scheduling**: WorkManager (for background exports)

## 📂 Project Structure
```
IRIS/
├── build.gradle.kts           # Root Gradle config (AGP 8.5, Kotlin, Hilt)
├── gradle.properties          # JVM args, AndroidX flags
├── gradlew / gradlew.bat      # Gradle wrapper scripts
├── gradle/wrapper/...
├── settings.gradle.kts        # Includes :app module
├── docs/architecture.md       # Vision, stack, data model sketch
└── app/
   ├── build.gradle.kts       # Module config with Compose, Room, Hilt, WorkManager, Coil
   ├── proguard-rules.pro
   └── src/main/
      ├── AndroidManifest.xml
      ├── java/com/example/irismark2/
      │   ├── IrisApplication.kt        # @HiltAndroidApp + WorkManager configuration
      │   ├── MainActivity.kt           # Hosts IrisApp composable
      │   ├── data/                     # Room entities/DAO, repository, attachment storage
      │   ├── domain/                   # Models, reflection engine, repository contract
      │   ├── di/                       # Hilt modules (database, dispatchers, bindings)
      │   ├── ui/                       # Compose navigation graph, screens, components, theme
      │   └── worker/                   # Monthly export worker
      └── res/values,xml                # Strings, backup rules
```

## 🚀 Getting Started

### Prerequisites
1. **Install Android Studio**
   - Download and install [Android Studio](https://developer.android.com/studio) (latest stable version recommended)
   
2. **Configure SDK Components**
   - Open Android Studio > Settings > Appearance & Behavior > System Settings > Android SDK
   - In SDK Platforms tab, ensure you have:
     - Android API 34 or newer (Android 14+)
   - In SDK Tools tab, ensure you have:
     - Android SDK Build-Tools (latest)
     - Android SDK Platform-Tools
     - Android SDK Command-line Tools (latest)
     - Android Emulator (optional, for testing without a physical device)

### Project Setup

1. **Clone the repository**
   ```powershell
   git clone <your-fork-or-repo-url> IRIS
   cd IRIS
   ```

2. **Configure API Key for IRIS Reflections**
   
   IRIS uses OpenRouter API to generate thoughtful reflections on your journal entries. You need to set up your API key:

   **Step 1: Get your OpenRouter API Key**
   - Visit [OpenRouter](https://openrouter.ai/)
   - Sign up or log in to your account
   - Navigate to your API Keys section
   - Create a new API key and copy it

   **Step 2: Configure the API Key**
   - Locate the `openrouter.properties` file in the project root directory
   - Open it and replace `enter your key here` with your actual API key:
     ```properties
     OPENROUTER_API_KEY=your-actual-api-key-here
     OPENROUTER_MODEL=openai/gpt-3.5-turbo
     ```
   - Save the file
   
   **Important Security Notes:**
   - ⚠️ Never commit your actual API key to version control
   - The `openrouter.properties` file should be added to `.gitignore`
   - Keep your API key private and secure
   
   **Alternative: Environment Variable (Optional)**
   
   You can also set the API key as an environment variable instead:
   ```powershell
   # Windows PowerShell
   $env:OPENROUTER_API_KEY="your-actual-api-key-here"
   $env:OPENROUTER_MODEL="openai/gpt-3.5-turbo"
3. **Open in Android Studio**
   - Launch Android Studio
   - Select "Open" and navigate to the cloned IRIS directory
   - Click OK and let Studio sync Gradle
   - Wait for dependency downloads and indexing to complete (first sync may take a few minutes)

4. **Run the App**
   - Connect a physical Android device (API 31+) via USB with USB debugging enabled, OR
   - Create an Android Virtual Device (AVD) in Device Manager (API 31+)
   - Click the "Run" button (green play icon) or press Shift+F10
   - Select your target device and wait for the app to build and launch

5. **Build from Command Line (Optional)**
   ```powershell
   # Clean build
   .\gradlew.bat clean
   
   # Run tests
   .\gradlew.bat test
   
   # Build debug APK
   .\gradlew.bat assembleDebug
   ```
   The APK will be generated at `app\build\outputs\apk\debug\app-debug.apk`

### Verifying the Setup

1. **Check API Configuration**
   - After building, check the build logs for API key warnings
   - If you see "OpenRouter API key not found", verify your `openrouter.properties` file

2. **Test IRIS Reflections**
   - Create a new journal entry with some text
   - Navigate to the IRIS companion screen
   - Verify that reflections are generated successfully
   - If reflections fail, check your API key and internet connection

### Troubleshooting

**Build Failures:**
- Ensure you have Java 17 or newer installed
- Clear Gradle cache: `.\gradlew.bat clean --refresh-dependencies`
- Invalidate caches in Android Studio: File > Invalidate Caches > Invalidate and Restart

**API Key Issues:**
- Verify the key is correctly pasted without extra spaces
- Check that `openrouter.properties` exists in the project root
- Ensure the file is not empty and follows the proper format
- Verify your OpenRouter account has available credits

**Runtime Errors:**
- Check logcat in Android Studio for detailed error messages
- Ensure minimum SDK version (API 31 / Android 12) is met on your device
- Grant necessary permissions when prompted

## 🛣️ Roadmap
- [x] Room database for entries, reflections, attachments metadata
- [x] Repository & storage helpers for saving media privately
- [x] Compose screens: monthly timeline, entry editor, IRIS companion view
- [x] Reflection engine (heuristics now, AI-friendly design for later)
- [x] Photo picker integration (gallery, app-specific storage)
- [ ] Monthly PDF export and full-history compressed backup
- [ ] Unit & UI testing, WorkManager verification, lint checks *(added first unit spec for reflection heuristics)*
- [ ] Optional biometric lock & sync explorations

## 🤝 Contributing
1. Fork or branch from `main`.
2. Implement your feature following the MVVM + Compose architecture.
3. Add/Update tests where appropriate.
4. Run quality checks:
   ```powershell
   .\gradlew.bat lint test assembleDebug
   ```
5. Submit a pull request with clear notes on changes and testing.
---
## Credits

* *Developed by:* Veer Tatiya
* *LinkedIn:* [www.linkedin.com/in/veer-tatiya](www.linkedin.com/in/veer-tatiya)  
* *GitHub:* [https://github.com/veertatiya](https://github.com/veertatiya)  
* *Version:* v1.0.0  
* *Contact:* veertatiya2004@gmail.com
---
Feel free to adapt or extend. PRs / refinements welcome.

## License

This project is for educational/demo purposes. You may adapt and extend as needed.


