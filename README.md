name: Build Andy Foodservice APK

on:
workflow_dispatch:
push:
branches: [main, master]

permissions:
contents: read

jobs:
build:
runs-on: ubuntu-latest
steps:
- name: Descargar archivos del repositorio
uses: actions/checkout@v4

  - name: Preparar Java
    uses: actions/setup-java@v4
    with:
      distribution: temurin
      java-version: '17'

  - name: Descomprimir proyecto
    run: |
      unzip -o AndyFoodservice_V1_ConCompilacionAPK.zip
      cd andyfoodservice_v1
      echo "PROJECT_DIR=$PWD" >> $GITHUB_ENV

  - name: Preparar Gradle
    uses: gradle/actions/setup-gradle@v4
    with:
      gradle-version: '8.7'

  - name: Compilar APK
    run: cd "$PROJECT_DIR" && gradle --no-daemon assembleDebug

  - name: Guardar APK
    uses: actions/upload-artifact@v4
    with:
      name: AndyFoodservice-debug-APK
      path: andyfoodservice_v1/app/build/outputs/apk/debug/app-debug.apk
