
name: NEXUS CRM APK Build

on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Java 17
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Setup Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extract project
        run: unzip -o NEXUS_CRM_Starter.zip -d extracted

      - name: Build APK
        working-directory: extracted/android
        run: gradle assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: NEXUS-CRM-APK
          path: extracted/android/app/build/outputs/apk/debug/app-debug.apk
          
