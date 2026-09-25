# --- SKRIP OTOMATISASI BUILD APK ---
name: Build Android APK
on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Java
        uses: actions/setup-java@v3
        with:
          distribution: 'zulu'
          java-version: '17'
          
      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          channel: 'stable'
          
      - name: Fetch Dependencies
        run: |
          if [ ! -f pubspec.yaml ]; then
            flutter create --org com.aduhlup . --overwrite
          fi
          flutter pub get

      - name: Build APK File
        run: flutter build apk --release

      - name: Upload APK Release
        uses: actions/upload-artifact@v4
        with:
          name: app-release-apk
          path: build/app/outputs/flutter-apk/app-release.apk
# panel-aicoach