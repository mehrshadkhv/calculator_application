[English](README.md) | [فارسی](README.fa.md)

# Flutter Calculator

A small calculator built with Flutter: a dark keypad, a separate expression and result display, and local expression evaluation. The app keeps its UI and calculation logic in two Dart files, without a backend or a state-management package.

![Flutter](https://img.shields.io/badge/Flutter-SDK-02569B?logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-%5E3.12.0-0175C2?logo=dart&logoColor=white)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Screenshots

| Keypad | Addition | Multiplication | `%` operator |
| :---: | :---: | :---: | :---: |
| <img src="assets/screenshots/calculator.png" alt="Calculator keypad with an empty display" width="200"/> | <img src="assets/screenshots/calculator(1).png" alt="45 plus 45 with a result of 90.0" width="200"/> | <img src="assets/screenshots/calculator(2).png" alt="76 times 15 with a result of 1140.0" width="200"/> | <img src="assets/screenshots/calculator(3).png" alt="The expression 10%100 with a result of 10.0" width="200"/> |

These images are kept in `assets/screenshots/`. To update the gallery, replace the images or edit the paths above. They are documentation assets, so they do not need to be registered in `pubspec.yaml`.

## Features

- Addition, subtraction, multiplication, and division, with expression parsing handled by `math_expressions`.
- Numeric input with decimal points and a `00` key.
- Separate displays for the current expression and the last evaluated result.
- `ac` clears both the expression and result; `ce` removes the last input character.
- `=` evaluates the expression without clearing it, so it can still be edited afterward.
- A fixed dark color palette, green operator keys, and a five-row keypad built with Material widgets.
- Calculations run locally. No account, API, or internet connection is needed to use the calculator.

### Current behavior and limitations

The `%` key passes the symbol directly to the expression parser; the app does not implement a separate percentage conversion. Invalid or incomplete expressions are not caught or shown as friendly error messages. Editing an expression leaves the previous result visible until `=` is pressed again or `ac` clears it. There is no calculation history, saved state, theme switcher, or scientific keypad.

## Tech stack and architecture

The package is named `calculator_application`, with version `1.0.0+1`. Its Dart SDK constraint is `^3.12.0` (`>=3.12.0 <4.0.0`). Use a Flutter SDK that includes a compatible Dart version; the existing local package configuration was generated with Flutter `3.44.0` and Dart `3.12.0`.

### Structure

```text
calculator_application/
├── lib/
│   ├── main.dart          # App entry point, calculator UI, input and evaluation
│   └── constants.dart     # Shared text and background colors
├── assets/
│   └── screenshots/       # Four images used in this README
├── android/               # Android host and build configuration
├── web/                   # Browser host and app manifest
├── windows/               # Windows runner and build configuration
├── analysis_options.yaml  # flutter_lints configuration
├── pubspec.yaml           # Package metadata and dependency constraints
├── pubspec.lock           # Resolved dependency versions
└── LICENSE                # MIT license
```

This is a small, single-screen app, not a layered Clean Architecture or MVVM project. `Application` is a `StatefulWidget`; `_ApplicationState` owns the `inputUser` and `result` strings and updates the UI through `setState`. Its `getRow` helper builds each row of buttons. Pressing `=` parses the input with `GrammarParser` and evaluates it with `RealEvaluator` and a `ContextModel`.

There are no separate data, domain, or repository layers. The app does not use BLoC, Provider, Riverpod, networking, or local-storage packages. Colors are defined in `constants.dart` and shared by the screen.

### Packages

Versions below are the constraints declared in `pubspec.yaml`, not necessarily the exact versions resolved in `pubspec.lock`.

- **`flutter` (SDK):** Material widgets, layout, and the built-in `StatefulWidget` / `setState` APIs.
- **`math_expressions: ^3.2.0`:** Parses and evaluates the input expression.
- **`hexcolor: ^3.1.3`:** Converts the hexadecimal color values in `constants.dart` into Flutter colors.
- **`cupertino_icons: ^1.0.8`:** Declared as a dependency, but not used by the current calculator screen.
- **`flutter_lints: ^6.0.0` (development):** Recommended Dart and Flutter lint rules, enabled in `analysis_options.yaml`.
- **`flutter_test` (SDK, development):** Flutter's testing tools. No test files are currently included in the repository.

## Getting started

### 1. Prepare your environment

Install Flutter with Dart `3.12.x` or another version allowed by `^3.12.0`, and add Flutter to your `PATH`. Check the toolchain before running the app:

```bash
flutter --version
flutter doctor
```

Platform host folders are currently included for **Android, web, and Windows**. Use an Android emulator or connected device for Android, Chrome for web, or Windows with Visual Studio's **Desktop development with C++** workload for a Windows build. Android builds also need the Android SDK and a JDK compatible with the project's Gradle configuration; the app targets Java/Kotlin JVM 17. iOS, macOS, and Linux host folders are not included in this checkout.

### 2. Clone the repository

```bash
git clone https://github.com/mehrshadkhv/calculator_application.git
cd calculator_application
```

If you already have a local copy, open a terminal in its root directory instead.

### 3. Install dependencies

```bash
flutter pub get
```

No code-generation step is required. The project does not declare `build_runner`, Freezed, or other code-generation dependencies.

### 4. Run the app

Start an emulator or connect a device, then run:

```bash
flutter devices
flutter run
```

To choose one of the included desktop or browser targets explicitly, use the relevant command:

```bash
# Web (requires Chrome)
flutter run -d chrome

# Windows (run on Windows with the C++ toolchain installed)
flutter run -d windows
```

### Development checks

Run static analysis from the project root:

```bash
flutter analyze
```

`flutter_test` is available for adding tests, but there is no `test/` directory yet. Once tests have been added, run them with `flutter test`.

## License

Licensed under the [MIT License](LICENSE). Copyright © 2026 Mehrshad Khodavirdizadeh.
