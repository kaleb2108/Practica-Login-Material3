# Práctica de Interfaces Móviles — Login con Material 3 y Material You

Prototipo funcional de interfaz de inicio de sesión (*Login*) desarrollado en **Android Studio** con **Kotlin** y **Jetpack Compose**, implementando **Material Design 3** y **Material You (Dynamic Color)**.

## Características Implementadas
- **Material You (Dynamic Color):** Extracción automática de paleta tonal desde el fondo de pantalla en Android 12+ (`API 31+`) con soporte dual **Modo Claro** y **Modo Oscuro**.
- **Componentes Material 3:** `Surface`, `OutlinedTextField` con validación en tiempo real y visibilidad de contraseña, `FilledButton` estilo píldora, `FilledTonalIconButton` biométrico y `AlertDialog`.
- **Arquitectura Modular:** Separación clara entre tema (`ui/theme/Theme.kt`, `Type.kt`) y vista declarativa (`MainActivity.kt`).

## Cómo ejecutar en Android Studio
1. Abre **Android Studio** y selecciona **File → Open**.
2. Elige la carpeta raíz `Practica-Login-Material3`.
3. Espera a que finalice la sincronización de **Gradle** y ejecuta en un emulador con **Android 12 (API 31) o superior**.
