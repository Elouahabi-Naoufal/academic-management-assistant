<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

<!-- PROJECT SHIELDS -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT TITLE -->
<br />
<div align="center">
  <a href="https://github.com/Elouahabi-Naoufal/academic-management-assistant">
    <img src="logo.png" alt="Academic Management Assistant icon" width="96" height="96">
  </a>

  <h3 align="center">Academic Management Assistant</h3>

  <p align="center">
    A native Android study companion for classes, modules and teachers — offline, on-device, with a
    live countdown to the next session.
    <br />
    <a href="https://github.com/Elouahabi-Naoufal/academic-management-assistant"><strong>Explore the repository »</strong></a>
    <br />
    <br />
    <a href="https://github.com/Elouahabi-Naoufal/academic-management-assistant/issues">Report Bug</a>
    &middot;
    <a href="https://github.com/Elouahabi-Naoufal/academic-management-assistant/issues">Request Feature</a>
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
is stored in a local SQLite database. The manifest requests no `INTERNET` permission, so the app
cannot talk to a network.

`MainActivity` hosts a five-tab bar — Dashboard, Classes, Modules, Teachers and Settings. The
selected tab is persisted in `SharedPreferences("app_prefs")` and restored on the next launch.

What each screen does:

* **Dashboard** — finds the next class across the week (wrapping to next week once today's sessions
  are over) and counts down in `HH:MM:SS`, ticking once a second. It also shows class/module/teacher
  counts, total weekly hours and a per-day hours breakdown.
* **Classes** — sessions sorted by weekday and start time, each with module, teacher and room. Add
  with the FAB, edit in place, or use selection mode to delete several at once.
* **Modules** — CRUD over the module list, including bulk delete. Editing a module lists the classes
  attached to it.
* **Teachers** — CRUD with an optional photo per teacher: pick from the gallery, crop, replace or
  remove. Editing a teacher lists their classes.
* **Settings** — follow-system or forced light/dark mode, one of several accent colours, and the
  school name plus academic-year labels shown in the top bar.
* **Year management** — create an academic year, archive the current one, and restore previous
  classes (all or a selected subset) into the current year.

Data access is hand-written: `DatabaseHelper` (schema version 12) plus one DAO per table
(`ClassDao`, `ModuleDao`, `TeacherDao`, `YearDao`, `ThemeDao`) issuing raw SQL. Theme and school/year
labels live in a single-row `theme` table so the top bar, tab accents and dashboard gradient read
the same source.

There is **no GPA tracking** in the current source, despite the repository topics mentioning it. The
dashboard tracks sessions and hours only.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

* [![Java][java-shield]][java-url]
* [![Android][android-shield]][android-url] — `compileSdk`/`targetSdk` 36, `minSdk` 30
* [![Gradle][gradle-shield]][gradle-url] — Gradle 8.13 wrapper, Android Gradle Plugin 8.13.1
* [![SQLite][sqlite-shield]][sqlite-url] — `android.database.sqlite` with raw SQL
* [![AndroidX][androidx-shield]][androidx-url] — AppCompat, Material Components, RecyclerView,
  Fragment, ViewPager2

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

### Prerequisites

* Android Studio with SDK 36 installed.
* JDK 11 or newer (the module uses Java 11 source/target compatibility).
* A device or emulator on API 30 (Android 11) or newer.
* No API key, backend or account.

### Installation

1. Clone the repository
   ```sh
   git clone https://github.com/Elouahabi-Naoufal/academic-management-assistant.git
   cd academic-management-assistant
   ```
2. Build and install a debug build
   ```sh
   ./gradlew installDebug
   ```
   Or open the folder in Android Studio and press Run.
3. Other useful targets
   ```sh
   ./gradlew assembleDebug   # APK under app/build/outputs/apk/debug/
   ./gradlew test            # local unit tests
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE -->
## Usage

1. Open **Modules** and add the modules for the semester.
2. Open **Teachers** and add teachers; attach a photo if you want one.
3. Open **Classes** and create sessions by choosing a module, teacher, room, weekday and time range.
4. Return to **Dashboard** to see the next session and the weekly hour totals.
5. Use **Year management** in Settings to archive the current year and restore classes for a new one.

The first launch starts with an empty database. `populate.sql` and `populate_database.py` are sample
data scripts for local experiments; they are not run automatically by the app.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ROADMAP -->
## Roadmap

- [x] Offline SQLite storage with no network permission
- [x] Class, module and teacher CRUD
- [x] Next-class countdown and weekly hour statistics
- [x] Themes, accent colours and school/year labels
- [x] Academic-year archive and restore
- [ ] Add the GPA calculation that the repository topics mention, or remove the topic
- [ ] Add unit tests for DAO queries and the next-class calculation
- [ ] Add a first-run setup flow instead of an empty dashboard
- [ ] Replace the unused Room dependency with either Room or no dependency

See the [open issues](https://github.com/Elouahabi-Naoufal/academic-management-assistant/issues) for a full list of proposed features and known issues.

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

### Top contributors:

<a href="https://github.com/Elouahabi-Naoufal/academic-management-assistant/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Elouahabi-Naoufal/academic-management-assistant" alt="contrib.rocks image" />
</a>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License

No `LICENSE` file is bundled with this repository. This project is not licensed — all rights reserved.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

Naoufal Elouahabi
* LinkedIn: [https://www.linkedin.com/in/elouahabi-naoufal/](https://www.linkedin.com/in/elouahabi-naoufal/)
* Email: [elouahabi-naoufal1@gmail.com](mailto:elouahabi-naoufal1@gmail.com)

Project Link: [https://github.com/Elouahabi-Naoufal/academic-management-assistant](https://github.com/Elouahabi-Naoufal/academic-management-assistant)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Android Developers](https://developer.android.com/)
* [Material Design](https://m3.material.io/)
* [Best-README-Template](https://github.com/othneildrew/Best-README-Template)
* [Img Shields](https://shields.io)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/Elouahabi-Naoufal/academic-management-assistant.svg?style=for-the-badge
[contributors-url]: https://github.com/Elouahabi-Naoufal/academic-management-assistant/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/Elouahabi-Naoufal/academic-management-assistant.svg?style=for-the-badge
[forks-url]: https://github.com/Elouahabi-Naoufal/academic-management-assistant/network/members
[stars-shield]: https://img.shields.io/github/stars/Elouahabi-Naoufal/academic-management-assistant.svg?style=for-the-badge
[stars-url]: https://github.com/Elouahabi-Naoufal/academic-management-assistant/stargazers
[issues-shield]: https://img.shields.io/github/issues/Elouahabi-Naoufal/academic-management-assistant.svg?style=for-the-badge
[issues-url]: https://github.com/Elouahabi-Naoufal/academic-management-assistant/issues
[license-shield]: https://img.shields.io/badge/License-All%20rights%20reserved-red.svg?style=for-the-badge
[license-url]: https://github.com/Elouahabi-Naoufal/academic-management-assistant
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/elouahabi-naoufal/
[java-shield]: https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white
[java-url]: https://openjdk.org/
[android-shield]: https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white
[android-url]: https://developer.android.com/
[gradle-shield]: https://img.shields.io/badge/Gradle-02303A?style=for-the-badge&logo=gradle&logoColor=white
[gradle-url]: https://gradle.org/
[sqlite-shield]: https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white
[sqlite-url]: https://www.sqlite.org/
[androidx-shield]: https://img.shields.io/badge/AndroidX-3DDC84?style=for-the-badge&logo=android&logoColor=white
[androidx-url]: https://developer.android.com/jetpack/androidx
