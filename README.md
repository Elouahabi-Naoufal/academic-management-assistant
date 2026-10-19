<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>




<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]




<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant">
    <img src="app/src/main/res/drawable/ic_launcher.png" alt="Academic Management Assistant icon" width="80" height="80">
  </a>

  <h3 align="center">Academic Management Assistant</h3>

  <p align="center">
    A native Android app for keeping your weekly class timetable, modules and teachers — offline,
    on-device, with a live countdown to the next session.
    <br />
    <a href="https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant"><strong>Read the source »</strong></a>
    <br />
    <br />
    <a href="https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/issues">Report Bug</a>
    &middot;
    <a href="https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/issues">Request Feature</a>
  </p>
</div>




<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>




<!-- ABOUT THE PROJECT -->
## About The Project

Academic Management Assistant is a single-module Java Android app for the data a student actually
has to keep track of: which module, which teacher, which room, which weekday, what time. Everything
lives in a local SQLite database. The manifest requests no `INTERNET` permission, so the app cannot talk
to a network even by accident.

`MainActivity` hosts a five-tab bar — Dashboard, Classes, Modules, Teachers, Settings — built from plain
`android.app.Fragment`s swapped into a container view. The selected tab is written to
`SharedPreferences("app_prefs")` and restored on the next launch.

What each screen does:

* **Dashboard** — finds the next class across the whole week (including wrap-around to next week once
  today's sessions are over) and counts down to it in `HH:MM:SS`, ticking once a second. Below that it
  shows counts of classes, modules and teachers, the total weekly hours, and a per-day hours breakdown.
* **Classes** — every session sorted by weekday then start time, each showing its module, teacher and
  room. Add via the FAB, edit in place, or enter selection mode and delete several at once.
* **Modules** — CRUD over the module list, including a bulk-delete selection mode. Opening a module for
  editing lists the classes attached to it.
* **Teachers** — CRUD with an optional photo per teacher: pick from the gallery, crop, replace or remove.
  Editing a teacher lists their classes, which is the quickest way to spot a clash.
* **Settings** — follow-system or forced light/dark mode, one of five accent colours, and the school name
  and academic-year labels shown in the gradient top bar.
* **Year management** — create an academic year, archive the current one, and restore a previous year's
  classes — all of them or a hand-picked subset — into the current year.

Data access is hand-written: a `SQLiteOpenHelper` (`DatabaseHelper`, schema version 12) plus one DAO class
per table (`ClassDao`, `ModuleDao`, `TeacherDao`, `YearDao`, `ThemeDao`) issuing raw SQL. Theme and
school/year labels live in a single-row `theme` table rather than in `SharedPreferences`, so the top bar,
the tab accents and the dashboard gradient all read the same source.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

* [![Java][Java.com]][Java-url]
* [![Android][Android.com]][Android-url]
* [![Gradle][Gradle.com]][Gradle-url] — Gradle 8.13 wrapper, Android Gradle Plugin 8.13.1, Kotlin DSL build scripts
* [![AndroidX][AndroidX.com]][AndroidX-url] — AppCompat, Material, Activity, ConstraintLayout, RecyclerView, Fragment, ViewPager2
* [![SQLite][SQLite.com]][SQLite-url] — via `android.database.sqlite`, with raw SQL

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

### Prerequisites

* Android Studio with an SDK matching `compileSdk = 36`.
* JDK 11 or newer — the module compiles with `sourceCompatibility`/`targetCompatibility` set to
  `VERSION_11`.
* A device or emulator running **API 30 (Android 11) or newer**, which is the project's `minSdk`.
* No API key, backend or account. Nothing to sign up for.

### Installation

1. Clone the repository

   ```sh
   git clone https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant.git
   cd Academic_Management_Assistant
   ```

2. Build and install a debug build

   ```sh
   ./gradlew installDebug
   ```

   Or open the folder in Android Studio and press Run. The launcher activity is `MainActivity`.

3. Other Gradle targets you may want

   ```sh
   ./gradlew assembleDebug        # APK at app/build/outputs/apk/debug/
   ./gradlew test                 # unit tests
   ./gradlew connectedAndroidTest # instrumented tests, needs a running device
   ```

4. Change the git remote to your fork before committing

   ```sh
   git remote set-url origin https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant.git
   git remote -v # confirm the changes
   ```

<details>
<summary>Loading the sample timetable</summary>

`populate.sql` and `populate_database.py` create `academic_management.db` in the repository root — a
workstation copy of a 13-module, 14-session timetable — so the schema and the queries can be inspected
without a device:

```sh
python3 populate_database.py       # writes academic_management.db in the project root
```

Both fixtures target an older revision of the schema (they use `archive_session_id` on `class`, and have
no `academic_year` or `theme` table). They are development aids, not an import format for the app: the
real database lives in the app's private storage and is created by `DatabaseHelper`.

</details>

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

### Adding a class

Open **Classes** and tap the FAB. The form takes a title, a module and teacher (both populated from your
existing lists via spinners), a room, a weekday and a start/end time. Weekday values follow
`Calendar.DAY_OF_WEEK`, i.e. `1` = Sunday through `7` = Saturday, which is what `populate.sql` uses too.

### Editing and bulk actions

Every list has a **Select** toggle that switches into multi-select mode, with *Select all* and *Delete
selected* alongside it. Tapping an item outside selection mode opens the corresponding edit activity.
Modules and teachers both have a separate Add/Edit/Delete activity rather than an inline dialog.

### Working across academic years

Year management is reached from the app's year controls. Creating a new year marks the previous one as
non-current and syncs the name into the `theme` table so the top bar updates. Archiving flags the current
year's classes as archived instead of deleting them. **Restore** opens a picker listing another year's
classes with per-row checkboxes, *Restore all*, and *Restore selected*, copying rows into the current year
as fresh, unarchived sessions.

### Theming

**Settings** exposes a *follow system theme* switch and a manual dark-mode switch, plus five accent
colours — `#6200EE` purple (default), `#2196F3` blue, `#10B981` green, `#F44336` red, `#FF9800` orange.
The accent drives the tab pill, the top-bar gradient and the dashboard countdown card. There are
light and dark resource sets under `res/values/` and `res/values-night/`.

### Schema

```sql
CREATE TABLE module   (id INTEGER PRIMARY KEY AUTOINCREMENT, name VARCHAR(255) NOT NULL);
CREATE TABLE teacher  (id INTEGER PRIMARY KEY AUTOINCREMENT,
                       full_name VARCHAR(255) NOT NULL,
                       image_path VARCHAR(255));
CREATE TABLE class    (id INTEGER PRIMARY KEY AUTOINCREMENT,
                       title VARCHAR(255) NOT NULL,
                       module_id INTEGER NOT NULL REFERENCES module(id),
                       teacher_id INTEGER NOT NULL REFERENCES teacher(id),
                       location VARCHAR(255),
                       weekday INTEGER NOT NULL,
                       start_time TEXT NOT NULL,
                       end_time TEXT NOT NULL);
CREATE TABLE theme    (id INTEGER PRIMARY KEY AUTOINCREMENT,
                       is_dark INTEGER DEFAULT 0,
                       accent_color VARCHAR(7) DEFAULT '#6200EE',
                       use_system_theme INTEGER DEFAULT 0,
                       school_name VARCHAR(255),
                       academic_year VARCHAR(50),
                       current_academic_year VARCHAR(50));
```

`DatabaseHelper` also carries an `onUpgrade` path that rebuilds `class` through a temporary backup table
for pre-v11 databases and backfills the v12 `theme` columns.

### Project layout

```
app/src/main/java/academic/management/assistant/
├── MainActivity.java              # tab bar, fragment host, top bar, accent theming
├── DashboardFragment.java         # next-class countdown and weekly statistics
├── ClassesFragment.java           # session list, selection mode
├── ModulesFragment.java           # module list, selection mode
├── TeachersFragment.java          # teacher list with photos, selection mode
├── SettingsFragment.java          # theme, accent colour, school name, academic year
├── Add|Edit{Class,Module,Teacher}Activity.java
├── YearManagementActivity.java    # create / archive academic years
├── YearDetailsActivity.java       # per-year class listing
├── RestoreActivity.java           # restore all or selected classes from a past year
├── adapter/                       # RecyclerView adapters for each list
├── database/                      # DatabaseHelper + Class/Module/Teacher/Year/Theme DAOs
└── model/                         # AcademicYear, ClassItem, Module, Teacher
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ROADMAP -->
## Roadmap

Implemented in the current tree:

- [x] Five-tab navigation with the last tab restored on relaunch
- [x] Dashboard with next-class detection and a per-second `HH:MM:SS` countdown
- [x] Weekly statistics: class/module/teacher counts, total hours, per-day hours
- [x] Full CRUD for classes, modules and teachers, plus multi-select delete on all three lists
- [x] Teacher photos with gallery pick, crop, replace and remove
- [x] Per-module and per-teacher class listings inside the edit screens
- [x] Light / dark / follow-system theming with five accent colours, applied at runtime
- [x] School name and academic-year labels in the gradient top bar
- [x] Academic year creation, archiving and selective restore
- [x] `onUpgrade` migrations up to schema version 12

Gaps visible in the code:

- [ ] `YearDao` queries an `academic_year` table and the `class.year_id` / `class.is_archived` columns,
      but `DatabaseHelper.onCreate` creates neither, and `onUpgrade` only handles pre-v11 and v12. A fresh
      install therefore cannot use year management until the missing DDL is added.
- [ ] `room-runtime` / `room-compiler` and `gson` are declared in `app/build.gradle.kts` but nothing
      imports them; the data layer is plain `SQLiteDatabase` and JSON is unused.
- [ ] The manifest declares `POST_NOTIFICATIONS` and `READ_EXTERNAL_STORAGE`, but no notification or
      storage API is called. `READ_EXTERNAL_STORAGE` is also a no-op on API 33+ under scoped storage.
- [ ] `compileSdk`/`targetSdk` are 36 while the declared AndroidX versions are pinned lower; the
      dependencies still resolve, but the pins should be revisited.
- [ ] `MainActivity` uses `android.app.Fragment` rather than AndroidX `Fragment`, and applies night mode
      both locally and via `setDefaultNightMode`, which is more than one mechanism for the same job.
- [ ] Only the stock `ExampleUnitTest` / `ExampleInstrumentedTest` exist; there is no coverage of the DAOs
      or the next-class calculation.
- [ ] No release signing configuration and `isMinifyEnabled = false`, so `assembleRelease` produces an
      unsigned, unshrunk build.

See the [open issues](https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

The schema is versioned by hand: if you change `DatabaseHelper`, bump `DATABASE_VERSION` and add the
corresponding branch to `onUpgrade`, otherwise existing installs will not pick the change up.

### Top contributors:

<a href="https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Elouahabi-Naoufal/Academic_Management_Assistant" alt="contrib.rocks image" />
</a>

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

No license file is currently bundled in this repository, so the terms are still to be settled. Open an
issue if you need a clear answer before reusing the code.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Naoufal Elouahabi - [LinkedIn](https://www.linkedin.com/in/elouahabi-naoufal/) - elouahabi-naoufal1@gmail.com

Project Link: [https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant](https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Android Jetpack / AndroidX](https://developer.android.com/jetpack)
* [Material Design Components for Android](https://m2.material.io/)
* [Gradle](https://gradle.org/)
* [Img Shields](https://shields.io)
* [Choose an Open Source License](https://choosealicense.com)

<p align="right">(<a href="#readme-top">back to top</a>)</p>




<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/Elouahabi-Naoufal/Academic_Management_Assistant.svg?style=for-the-badge
[contributors-url]: https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/Elouahabi-Naoufal/Academic_Management_Assistant.svg?style=for-the-badge
[forks-url]: https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/network/members
[stars-shield]: https://img.shields.io/github/stars/Elouahabi-Naoufal/Academic_Management_Assistant.svg?style=for-the-badge
[stars-url]: https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/stargazers
[issues-shield]: https://img.shields.io/github/issues/Elouahabi-Naoufal/Academic_Management_Assistant.svg?style=for-the-badge
[issues-url]: https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant/issues
[license-shield]: https://img.shields.io/badge/license-see%20repository-informational?style=for-the-badge
[license-url]: https://github.com/Elouahabi-Naoufal/Academic_Management_Assistant
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/elouahabi-naoufal/
[Java.com]: https://img.shields.io/badge/java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white
[Java-url]: https://openjdk.org/
[Android.com]: https://img.shields.io/badge/android-3DDC84?style=for-the-badge&logo=android&logoColor=white
[Android-url]: https://developer.android.com/
[Gradle.com]: https://img.shields.io/badge/Gradle-02303A?style=for-the-badge&logo=gradle&logoColor=white
[Gradle-url]: https://gradle.org/
[AndroidX.com]: https://img.shields.io/badge/AndroidX-3DDC84?style=for-the-badge
[AndroidX-url]: https://developer.android.com/jetpack/androidx-overview
[SQLite.com]: https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white
[SQLite-url]: https://www.sqlite.org/