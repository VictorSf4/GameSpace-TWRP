# 🔨 Guía de Compilación (Para Desarrolladores)

Esta guía es para quien quiera compilar GameSpace desde el código fuente y crear su propio paquete TWRP.

---

## 📋 Requisitos

### Hardware
- PC/Mac con mínimo 8GB RAM
- 10GB de espacio libre en disco
- Conexión a internet estable

### Software
- **Git:** `git --version`
- **Java JDK 17+:** `java -version`
- **Android SDK:** Incluido en Android Studio
- **Gradle:** Incluido en el proyecto
- **TWRP Recovery** (para testear)

---

## 🚀 Paso 1: Clonar el Repositorio

```bash
# Clona el repo original de IRedDragonICY
git clone https://github.com/IRedDragonICY/GameSpace.git
cd GameSpace

# Verifica la estructura
ls -la
```

Estructura esperada:
```
GameSpace/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── AndroidManifest.xml
│   │   │   ├── java/
│   │   │   └── res/
│   │   └── test/
│   ├── build.gradle.kts
│   └── platform.jks
├── gradle/
├── build.gradle.kts
├── settings.gradle.kts
├── gradlew
└── README.md
```

---

## 🔧 Paso 2: Configurar el Ambiente

### 2.1 Instalar Android Studio (Recomendado)

1. Descarga desde [developer.android.com](https://developer.android.com/studio)
2. Instala y ejecuta
3. Abre `GameSpace/` como proyecto
4. Espera a que se sincronice Gradle (~5-10 minutos)

### 2.2 Instalar JDK 17

**Windows/macOS:**
```bash
# Descarga de https://www.oracle.com/java/technologies/downloads/
# O usa Homebrew (macOS):
brew install openjdk@17
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt install openjdk-17-jdk
java -version
```

### 2.3 Verificar Gradle

```bash
./gradlew --version
# Expected: Gradle 8.x.x
```

---

## 📦 Paso 3: Compilar el APK

### 3.1 APK Debug (Desarrollo)

```bash
# Limpia builds anteriores (opcional)
./gradlew clean

# Compila Debug
./gradlew assembleDebug

# ✅ Salida:
# app/build/outputs/apk/debug/app-debug.apk
```

**Tamaño:** ~8-10 MB  
**Tiempo de compilación:** 3-5 minutos

### 3.2 APK Release (Producción)

```bash
# Compila Release optimizado
./gradlew assembleRelease

# ✅ Salida:
# app/build/outputs/apk/release/app-release.apk
```

**Tamaño:** ~7-8 MB (minificado)  
**Tiempo de compilación:** 5-8 minutos

---

## 📱 Paso 4: Testear en Dispositivo

### 4.1 Instalar APK Debug via ADB

```bash
# Conecta tu dispositivo
adb devices

# Instala
adb install app/build/outputs/apk/debug/app-debug.apk

# Abre la app
adb shell am start -n com.ireddragonicy.gamespace/.hub.GameHubActivity
```

### 4.2 Ver logs en tiempo real

```bash
# Monitorea logcat
adb logcat | grep GameSpace

# O todos los logs
adb logcat
```

### 4.3 Desinstalar

```bash
adb uninstall com.ireddragonicy.gamespace
```

---

## 📦 Paso 5: Crear Paquete TWRP Flasheable

### 5.1 Estructura necesaria

```bash
# Desde la carpeta GameSpace/

# Crear directorios
mkdir -p GameSpace-TWRP/system/priv-app/GameSpace
mkdir -p GameSpace-TWRP/META-INF/com/google/android

# Copiar APK Release
cp app/build/outputs/apk/release/app-release.apk \
   GameSpace-TWRP/system/priv-app/GameSpace/GameSpace.apk
```

### 5.2 Crear scripts META-INF

**Archivo: `GameSpace-TWRP/META-INF/com/google/android/updater-script`**

```bash
cat > GameSpace-TWRP/META-INF/com/google/android/updater-script << 'EOF'
ui_print("╔════════════════════════════════════════╗");
ui_print("║     Installing GameSpace v0.2          ║");
ui_print("║  IRedDragonICY's Advanced Game Space  ║");
ui_print("╚════════════════════════════════════════╝");
ui_print(" ");

ui_print("Installing system app...");
package_extract_dir("system", "/system");

ui_print(" ");
ui_print("Setting permissions...");
set_perm_recursive(0, 0, 0755, 0644, "/system/priv-app/GameSpace");
set_perm(0, 0, 0644, "/system/priv-app/GameSpace/GameSpace.apk");

ui_print(" ");
ui_print("Installation complete!");
ui_print("Rebooting device...");
ui_print(" ");

set_progress(1.000000);
EOF
```

**Archivo: `GameSpace-TWRP/META-INF/com/google/android/update-binary`**

```bash
cat > GameSpace-TWRP/META-INF/com/google/android/update-binary << 'EOF'
#!/sbin/sh

OUTFD=$2
ZIPFILE=$3

ui_print() {
  echo "ui_print $1" >> /proc/self/fd/$OUTFD
  echo "ui_print" >> /proc/self/fd/$OUTFD
}

ui_print "TWRP Flashable Installer"
EOF

chmod +x GameSpace-TWRP/META-INF/com/google/android/update-binary
```

### 5.3 Crear el ZIP

```bash
cd GameSpace-TWRP

# Empaquetar
zip -r -q ../GameSpace-v0.2-TWRP-Flashable.zip . -x "*.DS_Store"

cd ..

# Verifica el ZIP
ls -lh GameSpace-v0.2-TWRP-Flashable.zip
unzip -l GameSpace-v0.2-TWRP-Flashable.zip | head -20
```

**Salida esperada:**
```
Archive:  GameSpace-v0.2-TWRP-Flashable.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     1234  2026-09-06 12:34   META-INF/com/google/android/update-binary
     5678  2026-09-06 12:34   META-INF/com/google/android/updater-script
  8945120  2026-09-06 12:34   system/priv-app/GameSpace/GameSpace.apk
---------                     -------
  8952032                     3 files
```

---

## 🧪 Paso 6: Testear en TWRP

### 6.1 Transferir ZIP

```bash
adb push GameSpace-v0.2-TWRP-Flashable.zip /sdcard/
```

### 6.2 Bootear en TWRP

```bash
adb reboot recovery
```

### 6.3 Flashear

1. TWRP Home → **Install**
2. Selecciona `/sdcard/GameSpace-v0.2-TWRP-Flashable.zip`
3. **Desliza para confirmar**
4. Espera a que complete
5. **Reboot System**

### 6.4 Verificar instalación

```bash
adb shell pm list packages | grep gamespace
# Debe mostrar: com.ireddragonicy.gamespace
```

---

## 📤 Paso 7: Publicar en GitHub

```bash
# Navega a tu repo GameSpace-TWRP
cd /ruta/a/GameSpace-TWRP

# Sube el ZIP
git add GameSpace-v0.2-TWRP-Flashable.zip
git commit -m "Add GameSpace v0.2 TWRP flashable release"
git push

# Crea un Release en GitHub
# 1. Ve a https://github.com/VictorSf4/GameSpace-TWRP/releases
# 2. New Release
# 3. Tag: v0.2
# 4. Sube el ZIP
# 5. Publish
```

---

## 🔧 Troubleshooting Compilación

### Error: "Java not found"
```bash
# Instala JDK 17
# Verifica:
java -version
$JAVA_HOME
```

### Error: "Gradle sync failed"
```bash
# Limpia cache
./gradlew clean

# O descarga dependencias manualmente
./gradlew build --refresh-dependencies
```

### Error: "compileSdk 36 not found"
```bash
# Necesitas Android SDK 36
# Instala vía Android Studio:
# Android Studio → SDK Manager → SDK Platforms → Android 15 (API 36)
```

### El APK no se instala en dispositivo
```bash
# Desinstala versión anterior
adb uninstall com.ireddragonicy.gamespace

# Reinstala con permisos forzados
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

---

## 🎯 Customización

### Cambiar versión

Edita `app/build.gradle.kts`:

```kotlin
android {
    defaultConfig {
        versionCode = 101  // Incrementa este
        versionName = "0.3"  // Cambia este
    }
}
```

### Cambiar nombre del paquete

Edita `app/build.gradle.kts`:

```kotlin
android {
    namespace = "com.tuempresa.gamespace"
    
    defaultConfig {
        applicationId = "com.tuempresa.gamespace"
    }
}
```

También actualiza `app/src/main/AndroidManifest.xml`:

```xml
<manifest
    package="com.tuempresa.gamespace">
```

### Cambiar icono/nombre

- Icono: `app/src/main/res/mipmap/ic_launcher.png`
- Nombre: `app/src/main/res/values/strings.xml` → `<string name="app_name">Tu Nombre</string>`

---

## 📚 Recursos

- [Android Developer Docs](https://developer.android.com/)
- [Gradle Documentation](https://gradle.org/docs/)
- [TWRP Flashable Zip Format](https://twrp.me/faq/dev/index.html)
- [Kotlin Android Development](https://developer.android.com/kotlin)

---

## ✅ Checklist Final

- ✅ Git instalado
- ✅ JDK 17 instalado
- ✅ Android Studio sincronizado
- ✅ APK Release compilado
- ✅ Estructura TWRP creada
- ✅ Scripts META-INF agregados
- ✅ ZIP empaquetado
- ✅ ZIP testeado en TWRP
- ✅ ZIP publicado en GitHub

---

**¡Ya puedes crear tus propios paquetes TWRP! 🎉**
