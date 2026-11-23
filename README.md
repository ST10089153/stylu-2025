# Stylu: Smart Wardrobe App for Android

[![Android Build](https://github.com/ST10089153/stylu-2025/actions/workflows/android_build.yml/badge.svg)](https://github.com/ST10089153/stylu-2025/actions/workflows/android_build.yml)
![Kotlin](https://img.shields.io/badge/Kotlin-2.0-7F52FF?logo=kotlin&logoColor=white)
![Android](https://img.shields.io/badge/Android-8.1%2B%20(API%2027)-3DDC84?logo=android&logoColor=white)
![Room](https://img.shields.io/badge/Room-offline%20cache-4285F4)
![Supabase](https://img.shields.io/badge/Supabase-auth%20%2B%20storage-3ECF8E?logo=supabase&logoColor=white)

Stylu turns your closet into a digital wardrobe. You photograph your clothes, the app cuts out the background, and from there you can build outfits on a drag-and-drop canvas, plan what to wear on a calendar, and check the week's weather before you pick.

It was built in 2025 by a team of three for the PROG7314 module at Emeris University. This repository is a portfolio copy of the team repo, [ST10320489/stylu](https://github.com/ST10320489/stylu), with the full commit history kept.

**Demo video:** [youtube.com/watch?v=vY-s_k3i2YE](https://www.youtube.com/watch?v=vY-s_k3i2YE)

<p>
  <img src="screenshots/weather_home.jpg" width="200" alt="Home screen with 7-day forecast">
  <img src="screenshots/item_grid.jpg" width="200" alt="Wardrobe item grid">
  <img src="screenshots/create_outfit.jpg" width="200" alt="Outfit canvas">
  <img src="screenshots/calender.jpg" width="200" alt="Outfit calendar">
</p>

## What it does

- **Add clothing items.** Take a photo or pick one from the gallery, remove the background (remove.bg API), then crop, rotate or flip it with uCrop. Each item gets a category, subcategory, colour, size and brand.
- **Build outfits.** Drag items onto a canvas, resize, rotate and layer them, then save the result as a snapshot. The Android Palette API pulls the outfit's main colours.
- **Plan ahead.** Schedule one or more outfits per day on a calendar, with an event name and notes.
- **Dress for the weather.** The home screen shows a 7-day forecast from Open-Meteo for your location, and a WorkManager job sends a daily weather reminder.
- **Clear out the closet.** A swipe interface (keep or discard) helps you sort through items, starting with the least worn. Discarded items can be restored.
- **Search and filter** by name, colour, size and category.
- **Sign in** with email and password, Google, or fingerprint/face unlock once you've logged in before.
- **Use it in 10 languages,** including Afrikaans, isiXhosa, isiZulu, Setswana, isiNdebele and Tshivenda.
- **Keep working offline.** Items, outfits and calendar entries are cached in Room and synced back when the connection returns.

## Tech stack

| Area | Tools |
|---|---|
| Language and UI | Kotlin, XML layouts with ViewBinding, Material Components, Navigation component with Safe Args |
| Architecture | MVVM (ViewModel + LiveData), repository pattern, coroutines |
| Local data | Room (items, outfits, outfit items, calendar), EncryptedSharedPreferences for tokens |
| Backend | Supabase (Postgres, Auth, Storage) and a team-built REST API hosted on Render |
| Networking | Retrofit, OkHttp, `HttpURLConnection` |
| Background work | WorkManager, Firebase Cloud Messaging |
| Images | Glide, uCrop, remove.bg, Palette API |
| Testing | JUnit 4, Truth, MockK, Robolectric, AndroidX Test, JaCoCo |
| CI | GitHub Actions: unit tests and a debug APK on every push |

## Project layout

```
app/src/main/java/com/iie/st10320489/stylu/
├── data/          Room database, DAOs, entities and API models
├── network/       Supabase client and REST API services
├── repository/    Repositories for items, outfits, calendar, auth, profile, weather
├── sync/          Calendar sync between Room and the API
├── workers/       WorkManager job for weather notifications
├── service/       Firebase messaging and the Open-Meteo client
├── ui/            Activities and fragments, grouped by feature
└── utils/         Location, permissions, language, notification helpers
```

## Tests

There are 25 JVM unit tests and 13 instrumented tests.

- **Unit tests** (`app/src/test`) cover sign-up validation, data models, Room entity conversions, API response parsing and the calendar sync service.
- **Instrumented tests** (`app/src/androidTest`) run on a device or emulator against an in-memory Room database, checking item, outfit and calendar DAOs and the offline sync flow.

```bash
./gradlew testDebugUnitTest          # unit tests
./gradlew connectedDebugAndroidTest  # instrumented tests (needs a device or emulator)
./gradlew jacocoTestReport           # coverage report in app/build/reports/jacoco
```

## Running it locally

You need Android Studio (or JDK 17 and the Android SDK, API 36).

```bash
git clone https://github.com/ST10089153/stylu-2025.git
cd stylu-2025
./gradlew assembleDebug
```

Then open the project in Android Studio and run it on an emulator or phone (Android 8.1 or newer).

> **Backend status:** the Supabase project and the Render-hosted API were student-tier services that have since been shut down. The app still builds and the tests pass, but signing in and syncing need a backend. To run it end to end, point `SupabaseClient.kt`, `ItemApiService.kt` and `ApiServices.kt` at your own Supabase project and an instance of the team's API ([ST10252644/stylu_api](https://github.com/ST10252644/stylu_api)). The weather forecast uses the public Open-Meteo API and needs no key.

## Team

| Name | Student number |
|---|---|
| Cherika Bodde | ST10252644 |
| Marene van der Merwe | ST10320489 |
| Charné Janse van Rensburg | ST10089153 |

My branches in the team repo covered the weather forecast on the home screen, the outfit creation canvas, the calendar and outfit scheduling feature, and the unit and instrumented test suites.

## More detail

- [Original project report](docs/project-report.md) with a feature-by-feature write-up and references
- [Release notes](docs/RELEASE_NOTES.md)
- [Privacy policy](docs/privacy-policy.md) and [terms of service](docs/terms-of-service.md)

---

Portfolio copy maintained by Charné Janse van Rensburg (ST10089153).
