# Práctica: Diseño y Prototipado de Interfaz Móvil (Login) con Material 3 y Material You

* **Institución:** `Tecnologico De Estudios Superiores del Oriente Del Estado De Mexico `
* **Asignatura:** Desarrollo de Interfaces Móviles
* **Estudiante:** `Kaleb Guadalupe Hernandez Torres `
* **Grupo:** `7S1`
* **Fecha de Entrega:** 17 de Noviembre
* **Repositorio Público en GitHub:** `https://github.com/kaleb2108/Practica-Login-Material3`

---

## 1. Fase de Investigación y Arquitectura

### 1.1 Investigación de Diseño: Material Design 3 y Material You
El desarrollo de la interfaz de inicio de sesión (*Login*) se fundamenta en las especificaciones oficiales de **Material Design 3 (M3)** y el motor de personalización **Material You**, introducido en Android 12 (API 31):

1. **Color Dinámico (*Dynamic Color*) y Espacio Perceptual HCT:**
   Material You utiliza el algoritmo *Monet* para extraer los colores dominantes del fondo de pantalla del dispositivo y generar cinco paletas tonales completas (`primary`, `secondary`, `tertiary`, `neutral` y `neutralVariant`). Estas paletas se calculan bajo el espacio de color **HCT (*Hue, Chroma, Tone*)**, asegurando que la relación de contraste entre texto y fondo cumpla con los estándares de accesibilidad WCAG AA (relación mínima de $4.5:1$ para texto normal) tanto en tema claro como en tema oscuro.
2. **Jerarquía Visual y Elevación Tonal:**
   En Material 3 se reducen las sombras paralelas tradicionales en favor de la **elevación tonal**, diferenciando contenedores mediante variaciones de luminosidad en la superficie (`background`, `surface`, `primaryContainer`, `secondaryContainer`). Esto permite que el logotipo institucional y el botón de acción principal destaquen de forma natural sin ruido visual.
3. **Ergonomía Móvil y Prevención de Errores:**
   * **Ley de Fitts:** Todos los controles interactivos mantienen un área táctil mínima de $48\text{dp} \times 48\text{dp}$ y se ubican en la zona central e inferior de la pantalla para facilitar la interacción a una mano.
   * **Retroalimentación Inmediata:** Implementación de campos `OutlinedTextField` con etiquetas flotantes, validación de formato de correo electrónico, interruptor de visibilidad de contraseña y soporte para autenticación biométrica.

### 1.2 Arquitectura de Software (MVVM + Jetpack Compose)
Para la construcción del prototipo en **Android Studio**, se emplea una arquitectura declarativa y reactiva basada en **Jetpack Compose** y el patrón **MVVM (*Model-View-ViewModel*)**:

```text
+-------------------------------------------------------------------+
|                   CAPA DE PRESENTACIÓN (UI Layer)                 |
|  MainActivity.kt / LoginScreen (Jetpack Compose + Material 3)     |
|  - Renderiza componentes M3 (OutlinedTextField, FilledButton)     |
|  - Aplica LoginMaterialYouTheme (dynamicLight/DarkColorScheme)    |
+---------------------------------+---------------------------------+
                                  | Observa estados reactivos
                                  v
+-------------------------------------------------------------------+
|                 CAPA DE ESTADO (State / ViewModel)                |
|  Gestión de Estado de Interfaz (LoginUiState)                     |
|  - Controla credenciales, visibilidad de clave y carga            |
|  - Ejecuta reglas de validación y dispara eventos de sesión       |
+---------------------------------+---------------------------------+
                                  | Invoca validación / autenticación
                                  v
+-------------------------------------------------------------------+
|                  CAPA DE DOMINIO / DATOS (Model)                  |
|  AuthRepository / Servicios Biométricos                           |
|  - Procesa credenciales y respuesta de autenticación              |
+-------------------------------------------------------------------+
```

### 1.3 Catálogo de Componentes Material 3 Utilizados

| Componente Material 3 | Función en la Pantalla | Tokens y Propiedades Aplicadas |
| :--- | :--- | :--- |
| `Surface` | Contenedor raíz de pantalla completa | `MaterialTheme.colorScheme.background` |
| `Box` + `Icon` | Contenedor circular de identidad/logo | `primaryContainer` y `onPrimaryContainer` |
| `Text` | Encabezado principal y subtítulos | `headlineMedium` (Bold) y `bodyMedium` |
| `OutlinedTextField` | Entradas de correo y contraseña | Esquinas de `16.dp`, iconos `leading`/`trailing` |
| `TextButton` | Enlace de recuperación de contraseña | Estilo textual con acento `primary` |
| `FilledButton` | Botón principal de acción (*Entrar*) | Forma de píldora (`CircleShape`), altura `52.dp` |
| `FilledTonalIconButton` | Acceso alternativo por huella dactilar | Contenedor tonal `secondaryContainer` (`56.dp`) |

---

## 2. Diseño de Interfaz: Bocetos, Wireframes y Mockups

### 2.1 Bocetos (*Sketches*)
En la etapa de ideación de baja fidelidad se definió una estructura vertical de una sola columna centrada, priorizando la jerarquía de lectura: logotipo superior, título de bienvenida, bloque de credenciales, botón de acción principal y acceso biométrico inferior.

> **[INSERTAR AQUÍ LA IMAGEN DEL BOCETO A LÁPIZ GENERADO]**

### 2.2 Esquemas Estructurales (*Wireframes*)
El *wireframe* de fidelidad media establece las proporciones exactas, alineaciones y márgenes en unidades independientes de densidad (`dp`):

* **Márgenes horizontales:** `24dp` en ambos lados para mantener comodidad visual en pantallas compactas.
* **Contenedor de logotipo:** `88dp × 88dp` centrado con separación inferior de `24dp`.
* **Campos de texto (`OutlinedTextField`):** Ancho al $100\%$ (`fillMaxWidth`), altura mínima de `56dp`, radio de esquina de `16dp` y espaciado vertical de `12dp`–`16dp`.
* **Botón principal (`FilledButton`):** Ancho al $100\%$, altura de `52dp` y bordes completamente redondeados (`CircleShape`).

> **[INSERTAR AQUÍ LA IMAGEN DEL WIREFRAME ESTRUCTURAL CON MEDIDAS EN DP]**

### 2.3 Maquetado Visual (*Mockups* — Material 3 y Material You)
El diseño visual en alta fidelidad demuestra la coherencia cromática entre **Modo Claro** y **Modo Oscuro**, respetando los *tokens* de superficie y contraste de Material You:

> **[INSERTAR AQUÍ LA IMAGEN DEL MOCKUP DE ALTA FIDELIDAD MODO CLARO / MODO OSCURO]**

---

## 3. Implementación del Prototipo en Android Studio

El proyecto fue desarrollado en **Android Studio** utilizando **Kotlin** y **Jetpack Compose** (`androidx.compose.material3`).

### 3.1 Configuración de Tema Dinámico (`ui/theme/Theme.kt`)
Se habilita `dynamicLightColorScheme` y `dynamicDarkColorScheme` para dispositivos con Android 12 (`Build.VERSION_CODES.S`) o superior, manteniendo una paleta Material 3 de respaldo para versiones anteriores.

```kotlin
@Composable
fun LoginMaterialYouTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }
        darkTheme -> DarkFallbackColors
        else -> LightFallbackColors
    }

    MaterialTheme(
        colorScheme = colorScheme,
        content = content
    )
}
```

### 3.2 Implementación de la Interfaz (`MainActivity.kt`)
La interfaz integra validación de correo electrónico y contraseña, estado de carga con `CircularProgressIndicator`, alternancia de visibilidad de clave, diálogo de acceso concedido y vistas previas (`@Preview`) para modos claro y oscuro.

*(El código fuente completo se encuentra disponible y versionado en el repositorio público de GitHub indicado en la portada).*

---

## 4. Evidencias de Implementación (Capturas de Pantalla)

A continuación se presentan las capturas obtenidas tras compilar y ejecutar el prototipo en el emulador de Android Studio:

### 4.1 Ejecución en Modo Claro (Material 3)
> **[INSERTAR AQUÍ CAPTURA DEL EMULADOR EN MODO CLARO]**

### 4.2 Adaptación de Color Dinámico (Material You)
> **[INSERTAR AQUÍ CAPTURA DEL EMULADOR TRAS CAMBIAR EL FONDO DE PANTALLA / TEMA DINÁMICO]**

### 4.3 Ejecución en Modo Oscuro (Dark Theme)
> **[INSERTAR AQUÍ CAPTURA DEL EMULADOR EN MODO OSCURO]**

### 4.4 Vista Previa en Android Studio (Design / Split Preview)
> **[INSERTAR AQUÍ CAPTURA DEL EDITOR DE ANDROID STUDIO MOSTRANDO EL CÓDIGO Y LOS PREVIEWS]**










---

## 5. Conclusiones

La implementación de **Material Design 3** y **Material You** mediante **Jetpack Compose** permite construir interfaces móviles altamente adaptables, accesibles y coherentes con el sistema operativo Android moderno. La separación de la arquitectura en capas facilita la validación reactiva de formularios, mientras que el uso de *tokens* dinámicos garantiza una transición fluida entre temas claros, oscuros y paletas personalizadas por el usuario sin duplicar código de diseño.

---

## 6. Referencias

* Google. (2024). *Material Design 3 Guidelines — Color System & Dynamic Color*. Material.io. https://m3.material.io/
* Android Developers. (2024). *Jetpack Compose Material 3 Documentation*. Google Developers. https://developer.android.com/jetpack/compose/designsystems/material3
