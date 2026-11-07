# Implementation Plan: MiCuadragesimaPrimeraApp (Módulo 15 — IoT MQTT Pub/Sub)

## Overview

Este plan ensambla y depura la app Android Java del Módulo 15 transcribiendo código **verbatim** desde `modulo_15.md` hacia el Working_Directory `MiCuadragesimaPrimeraApp`. No es diseño de lógica nueva: se transcribe cada clase, se reconcilian únicamente los encabezados `package`/`import`, se copian los 11 drawables requeridos desde `Images` (solo lectura) y se actualiza el manifiesto. Tras cada clase se corre Build_Verification (`gradlew`) antes de continuar.

Reglas transversales que aplican a todas las tareas de creación de clases:

- Transcribir el bloque de código correspondiente del markdown **verbatim**, sin agregar, quitar ni alterar líneas de lógica (R9.3).
- La única modificación permitida es insertar/ajustar el `package` correcto y los `import` necesarios para que cada símbolo resuelva (R5.3, R5.4, R6.1).
- Escribir todos los archivos con las **herramientas de edición de archivos**, nunca con comandos de terminal largos (problema de PSReadLine en Windows). El terminal se reserva para `gradlew` y comprobaciones de prerequisitos.
- Correr Build_Verification con `gradlew` tras cada clase; si falla, reportar clase + error del compilador y proponer una corrección, conservando la fuente (R4.6, R5.7, R6.5, R7.7).
- Restringir toda escritura al Working_Directory; tratar `Images` como solo lectura (R9.1, R9.2).

PBT no aplica a esta feature (transcripción verbatim de UI/IO sin propiedades universales), por lo que no hay sub-tareas de property tests; la verificación de código es la compilación incremental (ver design.md — Testing Strategy).

## Tasks

- [x] 1. Prechequeo: clases existentes, configuración Gradle y estructura
  - Verificar presencia e integridad byte-a-byte de las 5 Existing_Classes: `datos/AlmacenDatosRAM.java`, `vista/Boton.java`, `vista/GaugeSimple.java`, `vista/Acelerometro.java`, `vista/Luxometro.java`. Si alguna falta o difiere del markdown, HALT y reportar sin crear pendientes.
  - Confirmar que `settings.gradle.kts` conserva el repositorio maven `jitpack.io` y que `app/build.gradle.kts` conserva la dependencia `com.github.hannesa2:paho.mqtt.android:3.3.5`, ambos byte-a-byte.
  - Confirmar que existen los paquetes `datos`, `comunicaciones`, `vista`, `actividades_secundarias` bajo el Java_Source_Root y que existe `app/src/main/res/drawable`; crear cualquiera que falte antes de transcribir en él.
  - Localizar en `modulo_15.md` el bloque de código de cada Pending_Class (ClientePubSubMQTT, las 4 secundarias, la raíz); si alguno no se localiza o está vacío, reportar y no iniciar ensamblado.
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 1.6, 2.1, 2.2, 2.3, 2.4, 2.5, 2.6_

- [x] 2. Copiar los 11 drawables requeridos a res/drawable
  - Copiar con operaciones de archivo desde `Images` a `app/src/main/res/drawable`, usando el nombre exacto referenciado por el código: `wifi_subida.png`, `wifi_bajada.png`, `modelo.png`, `ax.png`, `ay.png`, `az.png`, `a.png`, `entrar.png`, `salir.png`, `configuracion.png`, `comunicacion_iot.png`.
  - Si un archivo fuente falta en `Images`, reportar el nombre y ofrecer al menos una opción de resolución antes de continuar; no renombrar silenciosamente nombres inválidos ([a-z0-9_]), reportar y pedir decisión.
  - Dejar `Images` intacta (solo lectura); confirmar que cada uno de los 11 nombres resuelve a exactamente un archivo en `res/drawable`. Si alguno no resuelve, reportar y no marcar el paso como exitoso.
  - _Requirements: 2.5, 2.6, 2.7, 3.1, 3.2, 3.3, 3.4, 3.5, 9.2_

- [x] 3. Crear el cliente MQTT comunicaciones/ClientePubSubMQTT.java
  - [x] 3.1 Transcribir ClientePubSubMQTT.java verbatim desde el markdown
    - Crear `comunicaciones/ClientePubSubMQTT.java` reproduciendo el bloque del markdown línea a línea (esta clase ya trae `package` e `import` completos en la fuente).
    - Debe usar `MqttAsyncClient` de `org.eclipse.paho.client.mqttv3` y NO referenciar `MqttAndroidClient`; leer MQTTHOST/USERNAME/PASSWORD/topicStr de `AlmacenDatosRAM` en el constructor.
    - Si la fuente no se localiza o está vacía, HALT, conservar archivos y reportar la fuente faltante.
    - _Requirements: 4.1, 4.2, 4.3, 4.5, 9.3_
  - [x] 3.2 Build_Verification tras ClientePubSubMQTT
    - Ejecutar `gradlew` para compilar; si hay error en la clase, reportar clase + error del compilador y proponer corrección (máx. 3 intentos automáticos; tras ello detener y reportar).
    - _Requirements: 4.4, 4.6, 4.7_

- [x] 4. Crear las 4 actividades secundarias (orden del markdown) con build incremental
  - [x] 4.1 Transcribir actividades_secundarias/ActividadComunicacion.java
    - Transcribir verbatim; insertar `package ...actividades_secundarias` como primera sentencia y los `import` necesarios (`vista.Boton`, `R.drawable.*`, las actividades Pub/Sub del mismo paquete). Luego ejecutar `gradlew`; reportar errores y proponer corrección conservando la fuente.
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 9.3_
  - [x] 4.2 Transcribir actividades_secundarias/ActividadConfiguracion.java
    - Transcribir verbatim; ajustar `package`/`import` (`datos.AlmacenDatosRAM`, widgets Android, `SharedPreferences`). Persistir con claves exactas `broker`, `usuario`, `pasword`, `topico` sin agregar/quitar/renombrar. Ejecutar `gradlew`; reportar errores y proponer corrección.
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.6, 5.7, 9.3_
  - [x] 4.3 Transcribir actividades_secundarias/ActividadComoClientePubMQTT.java
    - Transcribir verbatim; ajustar `package`/`import` (`vista.Boton`, `vista.Acelerometro`, `vista.Luxometro`, `comunicaciones.ClientePubSubMQTT`, `datos.AlmacenDatosRAM`, `org.json.*`, `R`). Ejecutar `gradlew`; reportar errores y proponer corrección.
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.6, 5.7, 9.3_
  - [x] 4.4 Transcribir actividades_secundarias/ActividadComoClienteSubMQTT.java
    - Transcribir verbatim; ajustar `package`/`import` (`vista.Acelerometro`, `vista.Luxometro`, `comunicaciones.ClientePubSubMQTT`, `datos.AlmacenDatosRAM`, `org.json.*`, `android.os.Handler`). Ejecutar `gradlew`; reportar errores y proponer corrección.
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.6, 5.7, 9.3_

- [x] 5. Checkpoint - Verificar build tras las actividades secundarias
  - Ensure all tests pass, ask the user if questions arise.

- [x] 6. Crear la actividad raíz y actualizar el manifiesto
  - [x] 6.1 Transcribir ActividadPrincipalMiCuadragesimaPrimeraApp.java
    - Crear en el paquete raíz `com.curso_simulaciones.micuadragesimaprimeraapp` transcribiendo verbatim (preservar whitespace, comentarios y orden); ajustar solo `package`/`import`, importando `actividades_secundarias.ActividadComunicacion` y `ActividadConfiguracion` para que el ruteo resuelva.
    - Resolver sizing por resolución y persistir preferencias MQTT vía `SharedPreferences("MisPreferencias")` sin agregar/quitar/renombrar claves. Ejecutar `gradlew`; si hay error reportar clase/ubicación/descripción, proponer corrección y dejar el archivo sin cambios hasta respuesta del usuario.
    - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5, 9.3_
  - [x] 6.2 Actualizar AndroidManifest.xml
    - Declarar `ActividadPrincipalMiCuadragesimaPrimeraApp` como launcher con `android:exported="true"` y un único intent-filter MAIN/LAUNCHER; declarar las 4 actividades bajo `.actividades_secundarias`; tema `NoTitleBar.Fullscreen` y orientación `portrait`; exactamente 4 permisos (WAKE_LOCK, INTERNET, ACCESS_NETWORK_STATE, HIGH_SAMPLING_RATE_SENSORS) sin duplicados; omitir el atributo `package` del root (el namespace vive en Gradle). Ejecutar `gradlew`; reportar errores de manifiesto y proponer corrección conservando el manifiesto.
    - _Requirements: 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7_

- [x] 7. Build_Verification final del proyecto completo
  - Ejecutar el wrapper `gradlew` para compilar todo el proyecto. Si falta JDK o Android SDK, HALT y reportar cuál falta y que el estado del build no pudo determinarse.
  - Si el build termina con error (exit distinto de 0), reportar cada clase/recurso que falla con su error específico y proponer correcciones, preservando los archivos. Si termina con exit 0 sin errores, reportar el build exitoso. Eliminar cualquier temporal creado durante la verificación.
  - _Requirements: 8.1, 8.2, 8.3, 8.4, 8.5, 9.6_

- [ ]* 8. Tarea opcional: icono lanzador personalizado desde lanzador_15.png
  - Generar el icono lanzador a partir de `Images/lanzador_15.png` solo después de que el ensamblado core tenga éxito. Un fallo de esta tarea NO debe hacer que el resultado del ensamblado core se reporte como fallido.
  - _Requirements: 3.6_

## Notes

- Las sub-tareas marcadas con `*` son opcionales y pueden omitirse (aquí, solo el icono lanzador).
- No se añaden sub-tareas de property tests: PBT no aplica (transcripción verbatim de UI/IO; ver design.md — Testing Strategy). La verificación de código es la compilación incremental.
- Toda escritura de archivos se hace con herramientas de edición (no terminal) por el problema de PSReadLine; el terminal solo corre `gradlew` y chequeos de prerequisitos.
- Cada tarea referencia cláusulas de requisitos granulares para trazabilidad.
- Los checkpoints aseguran validación incremental del build antes de avanzar.

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1", "2"] },
    { "id": 1, "tasks": ["3.1"] },
    { "id": 2, "tasks": ["3.2"] },
    { "id": 3, "tasks": ["4.1", "4.2", "4.3", "4.4"] },
    { "id": 4, "tasks": ["6.1"] },
    { "id": 5, "tasks": ["6.2"] },
    { "id": 6, "tasks": ["7"] },
    { "id": 7, "tasks": ["8"] }
  ]
}
```
