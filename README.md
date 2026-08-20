# NEXA AI Signal Overlay

**Premium real-time AI/statistical market-analysis and signal overlay for Android.**

This is a production-structured Android application implementing a system-wide floating overlay with multi-layer deterministic analysis, signal locking, result verification, history, analytics, and configurable API data sources.

> **Important disclaimer**  
> This application is an analytical / probability-based tool only.  
> It never claims guaranteed future results, guaranteed profit, or certainty.  
> Predictions are clearly labeled as predictions. Verified outcomes are historical results only.  
> Trade / act at your own risk.

---

## Features

- **Native floating overlay** via `WindowManager` + Foreground Service (`SYSTEM_ALERT_WINDOW`)
- Draggable premium AI orb icon with pulse & scanning animation
- Compact glassmorphic analysis panel
- Multi-layer statistical engine (Frequency, Trend, Pattern, Momentum, Volatility, Streak, Transition, Confidence)
- Signal lock per period ID (never regenerates different signal for same period)
- Result verification → WIN / LOSS / PENDING
- Period-aware notifications
- Local Room database for history & analytics
- Configurable confidence threshold, API endpoint, refresh interval, market mode (CALL/PUT or UP/DOWN)
- Premium dark glassmorphism UI (Compose + custom Views)
- Strict data integrity: no fabricated market data, no forced signals

---

## Project Structure

```
NexaAISignalOverlay/
├── app/
│   ├── build.gradle.kts
│   ├── proguard-rules.pro
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/com/nexa/ai/signaloverlay/
│       │   ├── NexaApplication.kt
│       │   ├── analysis/AnalysisEngine.kt
│       │   ├── data/
│       │   │   ├── api/ApiService.kt
│       │   │   ├── local/ (Room entities, DAOs, Database)
│       │   │   ├── model/MarketData.kt
│       │   │   ├── preferences/SettingsRepository.kt
│       │   │   └── repository/MarketRepository.kt
│       │   ├── engine/SignalEngine.kt
│       │   ├── service/OverlayService.kt, BootReceiver.kt
│       │   └── ui/
│       │       ├── MainActivity.kt
│       │       ├── MainViewModel.kt
│       │       ├── overlay/FloatingIconView.kt, OverlayPanelView.kt
│       │       └── theme/Theme.kt
│       └── res/
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
└── README.md
```

---

## Requirements

- Android Studio Hedgehog (2023.1.1) or newer recommended
- JDK 17
- minSdk 26, targetSdk 34
- Kotlin 1.9.x
- Android Gradle Plugin 8.2+

---

## Setup & Build

1. Open the project root (`NexaAISignalOverlay`) in Android Studio.
2. Let Gradle sync (internet required for dependencies).
3. If prompted, install missing SDK components.
4. Connect a device or start an emulator (API 26+).
5. Run the `app` configuration.

### Command line

```bash
./gradlew assembleDebug
# APK: app/build/outputs/apk/debug/app-debug.apk
```

---

## First Run

1. Launch the app.
2. Grant **Display over other apps** permission.
3. Go to **Settings → Data Source** → select **Binance (Recommended)**.
4. Choose Symbol (default `BTCUSDT`) and Interval (`1m`, `3m`, `5m`, `15m`).
5. Tap **Save Binance Settings**.
6. Go back to Home → **START OVERLAY**.
7. Floating NEXA orb appears. Tap it for live analysis.

### Binance Built-in Support (No API Key)

- Uses official Binance Public API (`/api/v3/klines`)
- Real closed candles → verified UP / DOWN history
- Automatic current candle countdown
- Works out of the box with zero configuration beyond choosing symbol/interval

### Custom API (Optional)

You can still switch to Custom API. Expected JSON shape:

```json
{
  "success": true,
  "currentPeriod": {
    "periodId": "202608201234",
    "startTime": 1692500000000,
    "endTime": 1692500060000,
    "remainingSeconds": 23,
    "mode": "UPDOWN",
    "dataStatus": "OK"
  },
  "history": [
    {
      "periodId": "202608201233",
      "timestamp": 1692499940000,
      "result": "UP",
      "isVerified": true
    }
  ]
}
```

---

## Signal Integrity Rules (enforced in code)

1. Never generate random signals.
2. Never claim guaranteed accuracy.
3. Never modify historical results.
4. Never change a saved signal after verification.
5. Always bind signal to a specific period ID.
6. Always verify actual results from valid data.
7. Clearly distinguish prediction from verified result.
8. Show **NO SIGNAL** when data quality / confidence is insufficient.
9. Do not fabricate API responses.
10. Do not present simulated data as real-time data.

If the API is unavailable or history is insufficient, the UI shows **DATA UNAVAILABLE** or **NO SIGNAL**.

---

## Architecture Notes

- **Overlay**: `OverlayService` (LifecycleService) + `WindowManager` floating views.
- **Analysis**: Pure deterministic Kotlin (`AnalysisEngine`). No ML model weights that invent outcomes.
- **Persistence**: Room (signals, verifications, market history) + DataStore (settings).
- **Networking**: Retrofit + OkHttp, HTTPS preferred, configurable base URL.
- **UI**: Jetpack Compose for main screens; custom `View` for lightweight overlay panel/icon.

---

## Permissions

- `SYSTEM_ALERT_WINDOW` – floating overlay
- `FOREGROUND_SERVICE` / `FOREGROUND_SERVICE_SPECIAL_USE`
- `INTERNET` / `ACCESS_NETWORK_STATE`
- `POST_NOTIFICATIONS`
- `VIBRATE`

---

## Customization

- Confidence threshold (Settings)
- Notification timing (seconds before result)
- Refresh interval
- Market mode: BINARY (CALL/PUT) or UPDOWN
- API endpoint path

---

## License & Responsibility

You are responsible for compliance with local laws and platform terms regarding overlays, financial tools, and data sources.  
This project is provided as a technical implementation of the requested specification.
