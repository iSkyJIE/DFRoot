# DFRoot Build & Release

DFRoot keeps its release build environment in the repository so future releases can be reproduced without reconstructing the CI setup.

## Pinned build environment

- JDK: 17
- Android compileSdk / targetSdk: 36
- Android Build Tools: 35.0.0
- Android NDK: 27.0.12077973
- CMake: 3.22.1
- Gradle: 8.9
- Android Gradle Plugin: 8.7.3
- ABI: arm64-v8a

The Gradle wrapper, Android module configuration, and GitHub Actions workflow are the source of truth.

## Version format

Release versions use:

`v1.0.YYMMDD`

Examples:

- 2026-10-01 -> `v1.0.261001`
- 2026-10-02 -> `v1.0.261002`

The Android `versionName` is the tag without the leading `v`, and `versionCode` is the six-digit `YYMMDD` value.

The current default source version is `1.0.261001`. Tag builds override the version automatically from the tag.

## Local build

```sh
./build.sh
```

The release APK is written to:

`dirtyfrag.apk`

To reproduce a tagged version locally:

```sh
./gradlew :app:assembleRelease -PDFROOT_VERSION_NAME=1.0.261001 -PDFROOT_VERSION_CODE=261001
```

## Release

Push a tag in the required format, for example:

```sh
git tag v1.0.261002
git push origin v1.0.261002
```

GitHub Actions will validate the date tag, build the APK with the pinned environment, create the GitHub Release if it does not already exist, and upload the APK automatically.
