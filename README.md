# C++ VSCode CMake Template

Plantilla base limpia para proyectos de C++ usando VSCode, CMake y MinGW/MSYS2 en Windows.

## 1. Requisitos

Instala y verifica:

- Visual Studio Code
- Extensión **C/C++** de Microsoft
- Extensión **CMake Tools** de Microsoft
- CMake
- MSYS2 / MinGW con `g++`, `gdb` y `mingw32-make`

En PowerShell:

```powershell
g++ --version
cmake --version
where g++
where gdb
where mingw32-make
```

Si `where g++` no muestra una ruta, el compilador no está correctamente agregado al `PATH`.

## 2. Abrir la plantilla

Abre VSCode y selecciona:

```text
File > Open Folder
```

Selecciona la carpeta raíz del proyecto, donde está el archivo `CMakeLists.txt`.

## 3. Configurar IntelliSense

La plantilla incluye dos configuraciones en:

```text
.vscode/c_cpp_properties.json
```

Por defecto se recomienda MSYS2 UCRT64:

```json
"compilerPath": "C:/msys64/ucrt64/bin/g++.exe"
```

Si tu compilador está en otra ruta, cámbiala según el resultado de:

```powershell
where g++
```

## 4. Configurar CMake

Desde PowerShell, en la raíz del proyecto:

```powershell
cmake -S . -B build -G "MinGW Makefiles" -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
```

## 5. Compilar

```powershell
cmake --build build
```

## 6. Ejecutar

```powershell
.\build\cpp_vscode_cmake_template.exe
```

Salida esperada:

```text
Hola, Mundo Pro++!
```

## 7. Ejecutar pruebas

```powershell
ctest --test-dir build --output-on-failure
```

Salida esperada:

```text
Test passed!
100% tests passed
```

## 8. Usar desde VSCode

Puedes usar las tareas incluidas:

- `Ctrl + Shift + B`: compilar.
- `Terminal > Run Task > Run: app`: compilar y ejecutar.
- `Terminal > Run Task > Test: ctest`: compilar y probar.
- `F5`: depurar, si `gdb.exe` está correctamente configurado en `.vscode/launch.json`.

## 9. Estructura del proyecto

```text
.
├── include/                  # Archivos .hpp
│   └── template.hpp
├── src/                      # Archivos .cpp
│   ├── main.cpp
│   └── saludo.cpp
├── test/                     # Pruebas básicas
│   ├── CMakeLists.txt
│   └── sample_test.cpp
├── docs/                     # Documentación
│   └── architecture.md
├── .vscode/                  # Configuración sugerida de VSCode
├── .github/workflows/        # Integración continua
├── CMakeLists.txt
└── README.md
```

## 10. Agregar una nueva clase

Ejemplo: agregar una clase `Alumno`.

1. Crear el encabezado:

```text
include/Alumno.hpp
```

2. Crear la implementación:

```text
src/Alumno.cpp
```

3. Agregar el `.cpp` al target `template_core` en `CMakeLists.txt`:

```cmake
add_library(template_core STATIC
    src/saludo.cpp
    src/Alumno.cpp
)
```

4. Compilar de nuevo:

```powershell
cmake --build build
```

## 11. Reutilizar como plantilla para otro proyecto

1. Copia la carpeta completa.
2. Cambia el nombre de la carpeta.
3. Borra la carpeta `build/`, si existe.
4. Cambia el nombre del proyecto en `CMakeLists.txt`:

```cmake
project(nombre_de_tu_proyecto LANGUAGES CXX)
```

5. Cambia el nombre del ejecutable en:

```text
.vscode/tasks.json
.vscode/launch.json
```

6. Compila desde cero.

## 12. Diagnóstico rápido

Si algo falla, limpia y recompila:

```powershell
Remove-Item -Recurse -Force build
cmake -S . -B build -G "MinGW Makefiles" -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
cmake --build build
.\build\cpp_vscode_cmake_template.exe
ctest --test-dir build --output-on-failure
```
