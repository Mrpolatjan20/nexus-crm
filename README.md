name: Build NEXUS CRM APK

on:
  workflow_dispatch:
  push:
    paths:
      - "NEXUS_CRM_Starter.zip"
      - ".github/workflows/android.yml"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Extract project
        run: unzip NEXUS_CRM_Starter.zip -d extracted

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17

      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: "8.9"

      - name: Build APK
        working-directory: extracted/android
        run: gradle assembleDebug

      - uses: actions/upload-artifact@v4
        with:
          name: NEXUS-CRM-APK
          path: extracted/android/app/build/outputs/apk/debug/app-debug.apk
