# Requirements Document

## Introduction

This feature delivers the Android application MiCuadragesimaPrimeraApp exactly as specified in Module 15 (`modulo_15.md`). The application demonstrates IoT communication using the MQTT publisher/subscriber pattern: two Android devices run the same app, one acting as PUBLICADOR (publishing accelerometer and lux-meter readings as JSON) and the other as SUSCRIPTOR (receiving those readings and rendering them on GaugeSimple tachometers).

The work is an assembly-and-debug task, not a design task. All source code already exists verbatim inside `modulo_15.md`. The Agent transcribes each class into the correct package in the correct order, copies the required drawable images, updates the Android manifest, and verifies compilation after each step. The Agent operates autonomously within the single editable working directory and reports blocking errors to the user with proposed solutions.

A partial build from a previous session already exists and MUST be preserved. The remaining work is: create ClientePubSubMQTT, the four secondary activities, the root activity, copy the required drawable images, and update the AndroidManifest.xml.

## Glossary

- **Agent**: The autonomous coding assistant that transcribes the code, copies resources, updates configuration, and verifies the build.
- **Module_Markdown**: The source document `c:\Users\PRODESK\OneDrive\Documentos\UNAL\SIMULACIONES\md\modulo_15.md` containing the complete source code of every class.
- **Working_Directory**: The single editable project directory `c:\Users\PRODESK\OneDrive\Documentos\UNAL\SIMULACIONES\MiCuadragesimaPrimeraApp`.
- **Images_Directory**: The read-only source folder `c:\Users\PRODESK\OneDrive\Documentos\UNAL\SIMULACIONES\Images` containing candidate drawable images.
- **Drawable_Directory**: The Android resource folder `app/src/main/res/drawable` within the Working_Directory.
- **Namespace**: The application package identifier `com.curso_simulaciones.micuadragesimaprimeraapp`.
- **Java_Source_Root**: The directory `app/src/main/java/com/curso_simulaciones/micuadragesimaprimeraapp` within the Working_Directory.
- **Manifest**: The file `app/src/main/AndroidManifest.xml` within the Working_Directory.
- **Existing_Classes**: The five already-present and verified classes: datos/AlmacenDatosRAM, vista/Boton, vista/GaugeSimple, vista/Acelerometro, vista/Luxometro.
- **Pending_Classes**: The classes not yet created: comunicaciones/ClientePubSubMQTT, the four secondary activities (ActividadComunicacion, ActividadConfiguracion, ActividadComoClientePubMQTT, ActividadComoClienteSubMQTT), and the root activity ActividadPrincipalMiCuadragesimaPrimeraApp.
- **Secondary_Activities**: The four activities that reside in the package `actividades_secundarias` per the Manifest.
- **Required_Drawables**: The image files the code references: wifi_subida.png, wifi_bajada.png, modelo.png, ax.png, ay.png, az.png, a.png, entrar.png, salir.png, configuracion.png, comunicacion_iot.png.
- **MQTT_Client_Class**: The class ClientePubSubMQTT that wraps `MqttAsyncClient` from `org.eclipse.paho.client.mqttv3`.
- **Broker_Defaults**: BROKER `tcp://45.56.74.248:1883`, USERNAME `fisica`, PASSWORD `iotfisica`, and a user-definable TOPIC.
- **Build_Verification**: Running the project's Gradle build (or compile step) to confirm the code compiles without errors.

## Requirements

### Requirement 1: Preserve and continue from existing partial work

**User Story:** As the user, I want the Agent to build on the verified partial work, so that completed configuration and classes are not lost or duplicated.

#### Acceptance Criteria

1. THE Agent SHALL retain the existing `settings.gradle.kts` jitpack.io maven repository entry byte-for-byte identical, adding, removing, or reordering zero characters in that entry.
2. THE Agent SHALL retain the existing `app/build.gradle.kts` dependency declaration `com.github.hannesa2:paho.mqtt.android:3.3.5` byte-for-byte identical, preserving the exact group, artifact, and version `3.3.5` without modification.
3. THE Agent SHALL retain each of the Existing_Classes (datos/AlmacenDatosRAM, vista/Boton, vista/GaugeSimple, vista/Acelerometro, vista/Luxometro) with content byte-for-byte identical, unless a compilation error directly references that class, in which case THE Agent SHALL apply only the minimal change required to resolve that specific compilation error.
4. WHEN the Agent begins the assembly task, THE Agent SHALL verify the presence of all five Existing_Classes (datos/AlmacenDatosRAM, vista/Boton, vista/GaugeSimple, vista/Acelerometro, vista/Luxometro) and SHALL NOT create any Pending_Class until all five are confirmed present.
5. IF any Existing_Class is missing from the MiCuadragesimaPrimeraApp working folder, THEN THE Agent SHALL halt assembly and report to the user a discrepancy message identifying each missing class by its path, without creating any Pending_Class.
6. IF any Existing_Class content is inconsistent with the Module_Markdown (module 15) definition for that class, THEN THE Agent SHALL halt assembly and report to the user a discrepancy message identifying the affected class and the nature of the inconsistency, preserving the existing class content until the user responds.

### Requirement 2: Verify source material and target structure before assembly

**User Story:** As the user, I want the Agent to confirm all source code and resources are available first, so that assembly does not start on an incomplete basis.

#### Acceptance Criteria

1. WHEN the Agent begins processing a Pending_Class, THE Agent SHALL locate the source code of that Pending_Class within the Module_Markdown before transcribing that class.
2. IF the source code of a Pending_Class cannot be located within the Module_Markdown, THEN THE Agent SHALL report the missing class name to the user, SHALL NOT transcribe that class, and SHALL NOT begin assembly.
3. THE Agent SHALL verify that the Java_Source_Root contains the package directories `datos`, `comunicaciones`, `vista`, and `actividades_secundarias`.
4. IF a required package directory is absent from the Java_Source_Root, THEN THE Agent SHALL create that package directory under the Java_Source_Root before transcribing any class into it.
5. WHEN the Agent prepares to copy Required_Drawables, THE Agent SHALL verify that the Drawable_Directory exists.
6. IF the Drawable_Directory is absent, THEN THE Agent SHALL create the Drawable_Directory before copying any Required_Drawable.
7. IF a Required_Drawable source file is absent from the Images_Directory, THEN THE Agent SHALL report the missing file name to the user, SHALL NOT proceed with copying that Required_Drawable, and SHALL present the user with at least one resolution option before continuing.

### Requirement 3: Copy required drawable images

**User Story:** As the user, I want the required images placed in the resource folder, so that the app can load every drawable the code references.

#### Acceptance Criteria

1. THE Agent SHALL copy exactly the eleven Required_Drawables (wifi_subida.png, wifi_bajada.png, modelo.png, ax.png, ay.png, az.png, a.png, entrar.png, salir.png, configuracion.png, comunicacion_iot.png) from the Images_Directory into the Drawable_Directory using the exact file name referenced by the code.
2. IF a Required_Drawable file name violates the Android drawable resource naming constraint ([a-z0-9_] lowercase), THEN THE Agent SHALL report the offending name to the user and request a decision before copying, rather than silently renaming.
3. THE Agent SHALL leave the Images_Directory unmodified during the copy operation, preserving its file count, file names, and file contents identical before and after.
4. WHEN all Required_Drawables are copied, THE Agent SHALL confirm that each of the eleven referenced drawable resource names resolves to exactly one file in the Drawable_Directory.
5. IF any Required_Drawable fails to resolve to a file in the Drawable_Directory, THEN THE Agent SHALL report each unresolved drawable name to the user and SHALL NOT report the copy step as successful.
6. WHERE a custom launcher icon is requested using the source `lanzador_15.png`, THE Agent SHALL perform the launcher-icon task only after core assembly succeeds, and a failure of the launcher-icon task SHALL NOT cause the core assembly result to be reported as failed.

### Requirement 4: Create the MQTT client class

**User Story:** As a developer, I want the ClientePubSubMQTT class created from the module code, so that the app can connect, publish, and subscribe over MQTT.

#### Acceptance Criteria

1. THE Agent SHALL create `comunicaciones/ClientePubSubMQTT.java` containing the source code transcribed from the Module_Markdown with every line of the source block reproduced in order and no lines added, removed, or altered.
2. THE MQTT_Client_Class SHALL use `MqttAsyncClient` from `org.eclipse.paho.client.mqttv3` and SHALL NOT reference `MqttAndroidClient`.
3. WHEN the MQTT_Client_Class is constructed, THE MQTT_Client_Class SHALL read the Broker_Defaults values (MQTTHOST, USERNAME, PASSWORD, topicStr) from AlmacenDatosRAM.
4. WHEN transcription of the MQTT_Client_Class completes, THE Agent SHALL run Build_Verification (Gradle build/compilation of the project) before creating the next class.
5. IF the Module_Markdown source for the MQTT_Client_Class cannot be located or is empty, THEN THE Agent SHALL halt creation of the class, retain any existing files unchanged, and report to the user an error indicating the missing source.
6. IF Build_Verification reports an error in the MQTT_Client_Class, THEN THE Agent SHALL report the class name and the specific compiler error to the user and provide a proposed code correction for that error.
7. WHILE Build_Verification continues to report an error in the MQTT_Client_Class after 3 correction attempts, THE Agent SHALL stop further automatic correction attempts and report the unresolved error to the user.

### Requirement 5: Create the secondary activities

**User Story:** As a developer, I want the four secondary activities created from the module code, so that the app provides configuration, role selection, publisher, and subscriber screens.

#### Acceptance Criteria

1. THE Agent SHALL create exactly four Secondary_Activities named ActividadComunicacion, ActividadConfiguracion, ActividadComoClientePubMQTT, and ActividadComoClienteSubMQTT, with source code transcribed character-for-character identical to the corresponding code blocks in the Module_Markdown, excluding only the package declaration and import statements which may be reconciled per criterion 4.
2. THE Agent SHALL place each Secondary_Activity in the package `actividades_secundarias` such that the package path matches the package declared for that class in the Manifest.
3. WHEN the Module_Markdown source for a Secondary_Activity omits a package declaration, THE Agent SHALL insert the package declaration for `actividades_secundarias` as the first statement of the class file.
4. WHEN the package declaration in a transcribed class differs from the `actividades_secundarias` target, THE Agent SHALL set the package declaration to the target and update all import statements so that every referenced symbol resolves, and THE Agent SHALL confirm resolution through Build_Verification.
5. THE Agent SHALL transcribe the Secondary_Activities in the top-to-bottom order in which their code blocks appear in the Module_Markdown.
6. WHEN transcription of a Secondary_Activity completes, THE Agent SHALL perform Build_Verification via the Gradle build before creating the next Secondary_Activity.
7. IF Build_Verification reports one or more errors in a Secondary_Activity, THEN THE Agent SHALL report to the user the class name and the error output from the Gradle build, propose a specific correction, and retain the transcribed source without discarding it, before proceeding.

### Requirement 6: Create the root activity

**User Story:** As a developer, I want the main launcher activity created from the module code, so that the app presents the entry screen and routes to the other screens.

#### Acceptance Criteria

1. THE Agent SHALL create `ActividadPrincipalMiCuadragesimaPrimeraApp.java` in the Namespace root package (com.curso_simulaciones.micuadragesimaprimeraapp) with source code character-for-character identical to the Module_Markdown block, preserving all whitespace, comments, and ordering, excluding only package and import statements.
2. THE root activity SHALL import the Secondary_Activities ActividadComunicacion and ActividadConfiguracion from the `actividades_secundarias` subpackage so that the routing calls resolve.
3. THE root activity SHALL resolve resolution-dependent sizing values and persist MQTT preferences through SharedPreferences as defined in the Module_Markdown, adding, removing, or renaming zero preference keys relative to the source.
4. WHEN transcription of the root activity completes, THE Agent SHALL perform Build_Verification by executing the Gradle build.
5. IF Build_Verification reports an error in ActividadPrincipalMiCuadragesimaPrimeraApp, THEN THE Agent SHALL report the class name, the location, and the description of each error to the user, propose a correction, and leave the source file unchanged until the user responds.

### Requirement 7: Update the Android manifest

**User Story:** As a developer, I want the manifest updated per the module, so that activities, permissions, and the launcher entry point are declared correctly.

#### Acceptance Criteria

1. THE Agent SHALL update the Manifest to declare ActividadPrincipalMiCuadragesimaPrimeraApp as the launcher activity with `android:exported="true"` and exactly one intent-filter containing action MAIN and category LAUNCHER.
2. THE Agent SHALL declare each Secondary_Activity defined in the Module_Markdown in the Manifest under the package path `.actividades_secundarias`.
3. THE Agent SHALL declare each activity with the theme NoTitleBar.Fullscreen and screen orientation portrait as specified in the Module_Markdown.
4. THE Agent SHALL declare exactly the four permissions WAKE_LOCK, INTERNET, ACCESS_NETWORK_STATE, and HIGH_SAMPLING_RATE_SENSORS as single entries with no duplicates.
5. WHERE the project defines the application package through the `namespace` in `app/build.gradle.kts`, THE Agent SHALL omit the `package` attribute from the Manifest root element.
6. WHEN the Manifest update completes, THE Agent SHALL perform Build_Verification via the Gradle build.
7. IF Build_Verification reports a Manifest-related error, THEN THE Agent SHALL report the specific error to the user, propose a correction, and retain the updated Manifest without reverting it until the user responds.

### Requirement 8: Final build verification

**User Story:** As the user, I want a final compile check after all pieces are in place, so that I know the project is in a buildable state.

#### Acceptance Criteria

1. WHEN all Pending_Classes, Required_Drawables, and the Manifest update are complete, THE Agent SHALL perform a final Build_Verification by executing the Gradle wrapper (gradlew) build command for the entire project.
2. IF the required JDK or Android SDK is not configured or cannot be located in the environment when Build_Verification is initiated, THEN THE Agent SHALL halt Build_Verification, report to the user which prerequisite (JDK or Android SDK) is missing, and report that the build state could not be determined.
3. IF the final Build_Verification completes with a non-zero exit status indicating build errors, THEN THE Agent SHALL report to the user each failing class or resource, the specific error associated with each, and propose corrections, while preserving all existing project files unchanged.
4. WHEN the final Build_Verification completes with a zero exit status and no reported errors, THE Agent SHALL report the successful build state to the user.
5. WHEN Build_Verification terminates, whether successful or failed, THE Agent SHALL remove any temporary files the Agent created during Build_Verification within the project workspace.

### Requirement 9: Scope and editing constraints

**User Story:** As the user, I want the Agent to stay within the defined scope, so that no unintended files, dependencies, or features are introduced.

#### Acceptance Criteria

1. THE Agent SHALL restrict all file create, edit, and write operations to within the Working_Directory (MiCuadragesimaPrimeraApp), unless the user explicitly authorizes a specific path outside it.
2. THE Agent SHALL treat the Images_Directory (the Images folder, located outside the Working_Directory) as read-only, permitting read operations such as copying image files into the Working_Directory while prohibiting any create, edit, or write operation within the Images_Directory.
3. WHEN the Agent transcribes class source code, THE Agent SHALL reproduce the corresponding code from the Module_Markdown verbatim, with zero added, removed, or altered lines of code relative to the source.
4. THE Agent SHALL add only the dependencies and resources explicitly named in the Module_Markdown, and SHALL add no dependency or resource that is not explicitly named in the Module_Markdown.
5. IF completing the task appears to require a dependency or resource not explicitly named in the Module_Markdown, THEN THE Agent SHALL halt that addition, report the specific dependency or resource name and the reason it is needed to the user, and request an explicit decision before adding it.
6. WHEN the Agent encounters an error it cannot resolve within the defined scope, THE Agent SHALL report to the user the specific class name and error description, and SHALL present at least one candidate solution, while leaving existing files unchanged until the user selects a resolution.
