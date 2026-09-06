# 🎮 GameSpace TWRP Flashable - APK Listo

**¡APK compilado y listo para instalar en tu dispositivo!**

---

## 📥 Descargas

### GameSpace v0.2 TWRP Flashable
- **Archivo:** `GameSpace-v0.2-TWRP-Flashable.zip`
- **Tamaño:** ~9-10 MB
- **Tipo:** Paquete flasheable para TWRP Recovery
- **Instalación:** Directa en TWRP, sin necesidad de root adicional

[⬇️ Descargar ZIP Flasheable](https://github.com/VictorSf4/GameSpace-TWRP/releases/download/v0.2/GameSpace-v0.2-TWRP-Flashable.zip)

---

### También disponibles:

**APK Suelto (para ADB)**
- `app-release.apk` - Para instalar con `adb install`
- Tamaño: ~7-8 MB

[⬇️ Descargar APK](https://github.com/VictorSf4/GameSpace-TWRP/releases/download/v0.2/app-release.apk)

**APK Debug**
- `app-debug.apk` - Versión de desarrollo
- Tamaño: ~8-10 MB

[⬇️ Descargar APK Debug](https://github.com/VictorSf4/GameSpace-TWRP/releases/download/v0.2/app-debug.apk)

---

## 🚀 Instalación Rápida

### Opción 1: TWRP (Recomendado)
```bash
# 1. Descarga el ZIP
# 2. Transfiere a tu dispositivo
adb push GameSpace-v0.2-TWRP-Flashable.zip /sdcard/

# 3. Reinicia en TWRP
adb reboot recovery

# 4. En TWRP: Install → Selecciona el ZIP → Desliza
# 5. Reboot System
```

### Opción 2: ADB directo
```bash
# Requiere USB Debugging habilitado
adb install app-release.apk
```

---

## 📋 Requisitos Mínimos

- **Android 15** (API 36) o superior
- **TWRP Recovery** (para flashear ZIP)
- **USB Debugging** habilitado (para ADB)
- **~100 MB** de espacio libre

---

## ✨ Características

✅ **Jetpack Compose UI** - Interfaz moderna  
✅ **GPU Hardware Interception** - Control de MSAA, Anisotropic Filtering  
✅ **Herramientas Avanzadas** - FPS Stats, Thermal Profiles, Diagnostics  
✅ **Optimización de Juegos** - Perfiles por app, Control de rendimiento  
✅ **Monitoreo en Tiempo Real** - Estadísticas detalladas  

---

## 🔐 Permisos Otorgados

Se instala como **app de sistema**, obteniendo automáticamente:
- `WRITE_SECURE_SETTINGS`
- `MANAGE_GAME_MODE`
- `MONITOR_INPUT`
- `MODIFY_AUDIO_ROUTING`
- Y otros permisos privilegiados

**No requiere root adicional**, pero algunos features funcionan mejor con Magisk.

---

## 🛠️ Troubleshooting

### "No se puede instalar"
```bash
# Desinstala versión anterior
adb uninstall com.ireddragonicy.gamespace

# Reinstala
adb install app-release.apk
```

### "Error en TWRP"
- Verifica que TWRP esté actualizado
- Intenta flashear desde `/sdcard/` en lugar de USB
- Comprueba que el ZIP no está corrupto

### "La app no aparece después de flashear"
- Reinicia el dispositivo
- Ve a Settings → Apps y busca "GameSpace"
- Limpia cache: Settings → Storage → Clear Cache

---

## 📱 Información Técnica

| Campo | Valor |
|-------|-------|
| Nombre del Paquete | `com.ireddragonicy.gamespace` |
| Versión | 0.2 |
| Código de Versión | 100 |
| Min SDK | Android 15 (API 36) |
| Target SDK | Android 15 (API 36) |
| Tipo | System App (priv-app) |
| Tamaño APK | ~7-8 MB |

---

## 📖 Guías Completas

- 📄 **[INSTALLATION.md](INSTALLATION.md)** - Tutorial paso a paso
- 🔨 **[BUILD.md](BUILD.md)** - Cómo compilar desde código fuente

---

## 🔗 Enlaces

- 🏗️ **Repo Original:** [IRedDragonICY/GameSpace](https://github.com/IRedDragonICY/GameSpace)
- 📱 **TWRP Recovery:** [twrp.me](https://twrp.me/)
- 🔓 **Magisk:** [magisk.me](https://magisk.me/)

---

## 📄 Licencia

**Apache License 2.0** - Consulta [LICENSE.md](LICENSE.md)

---

## 💡 Notas Importantes

⚠️ **Backup:** Haz backup de tu dispositivo antes de flashear  
⚠️ **Garantía:** Puede anular la garantía de tu dispositivo  
✅ **Compatible:** Android 15+, AOSP basadas  
✅ **Seguro:** Código abierto, auditado  

---

**¿Problemas?** Crea un [issue](https://github.com/VictorSf4/GameSpace-TWRP/issues)

**Última actualización:** 2026-09-06  
**Compilado con:** Gradle 8.13.2 | Kotlin 2.3.0 | Android Studio
