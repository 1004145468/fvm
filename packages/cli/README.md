<img src="https://github.com/leoafarias/fvm/blob/master/docs/fvm-logo.png?raw=true" alt="drawing" width="200"/>

![GitHub stars](https://img.shields.io/github/stars/leoafarias/fvm?style=social)
[![Pub Version](https://img.shields.io/pub/v/fvm?label=version&style=flat-square)](https://pub.dev/packages/fvm/changelog)
[![Likes](https://img.shields.io/badge/dynamic/json?color=blue&label=likes&query=likes&url=http://www.pubscore.gq/likes?package=fvm&style=flat-square&cacheSeconds=90000)](https://pub.dev/packages/fvm)
[![Health](https://img.shields.io/badge/dynamic/json?color=blue&label=health&query=pub_points&url=http://www.pubscore.gq/pub-points?package=fvm&style=flat-square&cacheSeconds=90000)](https://pub.dev/packages/fvm/score) ![Coverage](https://raw.githubusercontent.com/leoafarias/fvm/master/coverage_badge.svg?sanitize=true) [![Github All Contributors](https://img.shields.io/github/all-contributors/leoafarias/fvm?style=flat-square)](https://github.com/leoafarias/fvm/graphs/contributors) [![MIT Licence](https://img.shields.io/github/license/leoafarias/fvm?style=flat-square&longCache=true)](https://opensource.org/licenses/mit-license.php) [![Awesome Flutter](https://img.shields.io/badge/awesome-flutter-purple?longCache=true&style=flat-square)](https://github.com/Solido/awesome-flutter)

Flutter Version Management: A simple cli to manage Flutter SDK versions.

FVM helps with the need for a consistent app builds by allowing to reference Flutter SDK version used on a per-project basis. It also allows you to have multiple Flutter versions installed to quickly validate and test upcoming Flutter releases with your apps, without waiting for Flutter installation every time.

**Features:**

- Configure and use Flutter SDK version per project
- Ability to install and cache multiple Flutter SDK Versions
- Fast switch between Flutter channels & versions
- Dynamic SDK paths for IDE debugging support.
- Version FVM config with a project for consistency across teams and CI environments.
- Set global Flutter version across projects

## Version Management

This tool allows you to manage multiple channels and releases, and caches these versions locally, so you don't have to wait for a full setup every time you want to switch versions.

Also, it allows you to grab versions by a specific release, i.e. `v1.2.0` or `1.17.0-dev.3.1`. In case you have projects in different Flutter SDK versions and do not want to upgrade.

## Usage

1. [Install Dart](https://www.dartlang.org/install).
2. Activate Fvm:

```bash
> pub global activate fvm
```

[Read dart.dev docs for more info](https://dart.dev/tools/pub/cmd/pub-global#running-a-script) on how to run global dart scripts.

### Usage on Windows

- On Windows make sure you are running in developer mode [Instructions Here](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development) or as an administrator
- Make sure `pub global activate fvm` is running using the installed `dart-sdk` pub version. Not the one that comes with Flutter. I am currently looking in ways to make this better.

And then, for information on each command:

```bash
> fvm help
```

### Install a SDK Version

FVM gives you the ability to install many Flutter **releases** or **channels**.

- `version` - use `stable` to install the Stable channel and `v1.8.0` or `1.17.0-dev.3.1` to install the release.
- `--skip-setup` - will skip Flutter setup after install

```bash
> fvm install <version>
```

#### Project Config SDK Version

If you configured your project to use a specific version, run `install` without any arguments will install and link the proper version.

```bash
> fvm install
```

Check out `use` command to see how to configure a version per project.

### Use a SDK Version

You can use different Flutter SDK versions per project. To do that you have to go into the root of the project and:

```bash
> fvm use <version>
```

**Important:** Please consider adding `flutter_sdk` to your .gitignore to avoid versioning your Fvm Flutter SDK symlink.

**Set Global Version**

If you want to use a specific version by default in your machine, you can specify the flag `--global` to the `use` command. A symbolic link to the Flutter version will be created in the `fvm` home folder, which you could then add to your PATH environment variable as follows: `FVM_HOME/default/bin`. Use `fvm use --help`, this will give you the exact path you need to configure.

** Do not activate fvm using `flutter pub global activate`** if you plan on using the --global flag. Only activate fvm using `pub global activate fvm`

```bash
> fvm use <version> --global
```

**Force Flag**

Fvm only allows to call the use command on Flutter projects. However if you want to call the `use` command on a non-flutter directory use the `--force` flag.

If you are starting a new project and plan on using `fvm flutter create` you wil have to use the `--force` flag

```bash
> fvm use <version> --force
```

### Remove a SDK Version

Using the remove command will uninstall the SDK version locally, this will impact any projects that depend on that version of the SDK.

```bash
> fvm remove <version>
```

### Upgrade the current SDK Version

To upgrade currently used Flutter SDK version (e.g. `stable`) you should call the Flutter SDK command as you would normally do in case of typical Flutter installation. See more in the section [Running Flutter SDK commands](#running-flutter-sdk-commands).

```bash
> fvm flutter upgrade
```

### List Installed Versions

List all the versions that are installed on your machine. This command will also output where FVM stores the SDK versions.

```bash
> fvm list
```

### List Flutter Releases

Displays all Flutter releases, including the current version for `dev`, `beta` and `stable` channels.

```bash
> fvm releases
```

## Running Flutter SDK commands

There are couple of ways you can interact with the Flutter SDK setup in your project. You can run all the Flutter commands through the fvm _proxy commands_.

### Proxy Commands

Flutter command within `fvm` proxies all calls to the CLI just changing the SDK to be the local one.

For instance, to run the `flutter run` with a given Flutter SDK version just call the following. FVM will recursively try for a version in a parent directory.

```bash
> fvm flutter run
```

This syntax works also for commands with parameters. The following command will call `flutter build` for a selected flavor and target.

```bash
> fvm flutter build aab --release --flavor prod -t lib/main_prod.dart
```

In other words, calling a `fvm flutter xxx` command is equivalent to `flutter xxx` if `fvm` is available in the directory tree.

### Call Local SDK Directly

You can also call the local SDK directly bypassing the _proxy commands_. FVM creates a symbolic link within your project called **fvm** which links to the installed version of the SDK.

```bash
> .fvm/flutter/bin run
```

The above example is equivalent to `flutter run` command using the local project SDK.

### Change FVM Cache Directory

You are able to configure the **fvm** cache directory by setting `FVM_HOME` environment variable. If nothing is set the default **fvm** path will be used. You are also able to change the directory by setting the `--cache-path` on the config. See below

### FVM Config

There are some configurations which you are able to set on FVM. **All settings set on CLI are compatible with the App(GUI)**.

#### List config

```bash
> fvm config
```

#### Set cache path

Location where Flutter SDK versions will be stored. If nothing is set, default will be used.

```bash
> fvm config --cache-path <CACHE_PATH>
```

### Flutter Fork & Git Cache

You are able to use your own Flutter fork or cache the Flutter git locally for faster cloning, by setting the `FVM_GIT_CACHE` environment variable.

## Configure Your IDE

In some situations you might have to restart your IDE and the Flutter debugger to make sure it uses the new version.

### VSCode

Add the following to your `settings.json`. This will list list all Flutter SDKs installed when using VSCode when using `Flutter: Change SDK`.

Use `fvm list` to show you the path to the versions.

#### List all versions installed by FVM

You can see all the versions installed by FVM in VS Code by just providing path to `versions` directory:

```json
{
  "dart.flutterSdkPaths": ["/Users/usr/fvm/versions"]
}
```

Alternatively, you can specify only selected versions. The following snippet will cause VS Code to show only `stable` and `dev` versions of Flutter.

```json
{
  "dart.flutterSdkPaths": [
    "/Users/usr/fvm/versions/stable",
    "/Users/usr/fvm/versions/dev"
  ]
}
```

To change current Flutter version open a project and select `Flutter: Change SDK` in the command palette. You should see all the versions as depicted in the following screenshot.

![VS Code version selector screenshot](./docs/vs_code_versions.png?raw=true "VS Code version selector screenshot")

#### You can also add the version symlink for dynamic switch

```json
{
  "dart.flutterSdkPaths": [".fvm/flutter_sdk"]
}
```

### Android Studio

Copy the **_absolute_** path of fvm symbolic link in your root project directory. Example: `/absolute/path-to-your-project/.fvm/flutter_sdk`

In the Android Studio menu open `Languages & Frameworks -> Flutter` or search for Flutter and change Flutter SDK path. Apply the changes. You now can Run and Debug with the selected versions of Flutter.
Restart Android Studio to see the new settings applied.

[Add your IDE instructions here](https://github.com/leoafarias/fvm/issues)

## Working with this repo

### Tests

```bash
pub run test
```

### Publishing package

Before pushing package to pub.dev. Run command to create version constant.

```bash
pub run build_runner build
```

### Update test coverage

To update test coverage run the following command.

```bash
pub run test_coverage
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
