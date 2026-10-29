# GSoC 2026 – JabRef Native Image Support

| Field                  | Information                                                                                                                                                     |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Student**            | [Wanling Fu](https://github.com/wanling0000)                                                                                                                    |
| **Organization**       | [JabRef e.V.](https://verein.jabref.org/)                                                                                                                       |
| **Primary repository** | [JabRef/jabref](https://github.com/JabRef/jabref)                                                                                                               |
| **Project name**       | JabRef Native Image Support                                                                                                                                     |
| **Project mentors**    | [@koppor](https://github.com/koppor/), [@calixtus](https://github.com/calixtus), [@Maran23](https://github.com/Maran23), [@Stewori](https://github.com/Stewori) |
| **Project page**       | [Google Summer of Code 2026 Project Page](https://summerofcode.withgoogle.com/programs/2026/projects/WpfCv7KA)                                                  |
| **Status**             | Completed                                                                                                                                                       |

---

## Project summary

JabRef is comprehensive literature management software. Besides the desktop application (JabGui), it offers a command-line tool (JabKit), an HTTP server (JabSrv) and a language server for text editors (JabLS). All four share the core library JabLib. The main motivation was JabKit. Before this project, JabKit was available as a `jpackage` build and through JBang. Both run it on a JVM, and their startup time makes short command-line operations slow. [GraalVM Native Image](https://www.graalvm.org/latest/reference-manual/native-image/) compiles a Java application ahead of time into a standalone executable that starts faster.

This project added native image builds for all four applications. Native Image includes only code that it can find at build time by following method calls. It can miss code invoked in other ways, such as classes that frameworks load by name or Java methods called from native AWT and JavaFX code. These classes also differ per operating system, so a binary that works on one system can fail on another. Some of these gaps cause silent failures. As a result, most problems appeared only after a successful build, when the binary ran. The project filled them with metadata, generated from the code where possible. Smoke tests run the real binaries in CI to detect new gaps.

In a [benchmark](https://github.com/wanling0000/jabkit-startup-benchmark) on a Linux VM, the native `jabkit --help` took 2.1 seconds, and the JVM distributions took 4.0 to 5.3 seconds. The native download is 51 MiB, and the `jpackage` portable download is 194 MiB.

### Deliverable status

| Application | Native build | Smoke-test status                                            | Packaging     |
| :---------- | :----------- | :----------------------------------------------------------- | :------------ |
| JabKit      | Merged       | Merged, Linux and macOS                                      | Available     |
| JabLS       | Merged       | Merged, LSP session                                          | Not available |
| JabSrv      | Merged       | Merged, HTTP API                                             | Not available |
| JabGui      | Merged       | Open ([#17545](https://github.com/JabRef/jabref/pull/17545)) | Not available |

## Contributions to JabRef

### GSoC project scope

#### JabKit

##### [#15899](https://github.com/JabRef/jabref/pull/15899) - Add smoke-test CI workflow for JabKit native image

Highlights:

- Added a shared Gradle plugin for native image builds.
- Added the bootstrap metadata. With it, the native JabKit starts and shows `--help`.
- Added a [clitest](https://github.com/aureliojargas/clitest) smoke test for verification, and a CI workflow that runs it against the JabKit binary built with GraalVM.

##### [#15959](https://github.com/JabRef/jabref/pull/15959) - Add native image support for `check consistency` and `check integrity`

##### [#15971](https://github.com/JabRef/jabref/pull/15971) - Add native image support for `convert` and `citationkeys generate`

Highlights:

- The smoke tests now use `--porcelain`. Thus, they compare only the data output.

##### [#15973](https://github.com/JabRef/jabref/pull/15973) - Add native image support for `preferences` and `pseudonymize`

##### [#15985](https://github.com/JabRef/jabref/pull/15985) - Add native image support for `doi-to-bibtex`, `fetch`, `get-cited-works`, `get-citing-works`, `generate-bib-from-aux`

Highlights:

- Added HTTPS support to the native binary.
- Divided the smoke tests into offline and online tests.

##### [#15996](https://github.com/JabRef/jabref/pull/15996) - Add Linux to the JabKit native smoke test CI matrix

Highlights:

- The native smoke test now runs on Linux and macOS.

##### [#15997](https://github.com/JabRef/jabref/pull/15997) - Use picocli-codegen to generate reflection metadata

Highlights:

- picocli-codegen writes the reflection metadata for all commands at compile time.
- Removed 499 lines of manual metadata. When a command changes, a manual update is not necessary.

##### [#16060](https://github.com/JabRef/jabref/pull/16060) - Native image support for `search` and `pdf update`

Highlights:

- `search`: Added the missing Caffeine metadata. The GraalVM metadata repository does not have these entries.
- `pdf update`: PDFBox loads the AWT native library. On Linux, this library calls back into Java through JNI, and GraalVM CE has no metadata for these calls. Changed the toolchain to [Liberica NIK Full](https://bell-sw.com/liberica-native-image-kit/). It supplies AWT support, so the command works on Linux.

##### [#16144](https://github.com/JabRef/jabref/pull/16144) - Register Caffeine `weakKeys` cache classes for native image

Highlights:

- A new Caffeine cache in `AuthorList` needed other generated classes. Without them, all commands that read author names failed in the native image.

##### [#16135](https://github.com/JabRef/jabref/pull/16135) - Add JabKit native image how-to

Highlights:

- Added a developer how-to. It tells how to build and test the native image, and where the metadata is.

##### [#16327](https://github.com/JabRef/jabref/pull/16327) - Document non-AWT JabKit native usage

Highlights:

- Documented that only the JabKit commands that use AWT need the bundled `.so` files.

#### JabLS

##### [#16267](https://github.com/JabRef/jabref/pull/16267) - Enable GraalVM Native Image builds for JabLS

Highlights:

- The metadata for each dependency is in the module that owns the dependency (`jabls` or `jabls-cli`).

##### [#16462](https://github.com/JabRef/jabref/pull/16462) - Add native-image smoke test for JabLS

Highlights:

- A Python script starts the native binary and sends a full LSP session over TCP. It examines each response.

#### JabSrv

##### [#16565](https://github.com/JabRef/jabref/pull/16565) - Add GraalVM Native Image support for JabSrv

Highlights:

- In standalone mode (without the JabRef GUI), the native binary works like the JVM version.
- Jersey and HK2 find classes by name at runtime. The PR description puts the metadata into groups and gives the reason for each group.

##### [#16634](https://github.com/JabRef/jabref/pull/16634) - Run the CAYW JavaFX picker in the native JabSrv image

Highlights:

- Added the JavaFX metadata and resources for the CAYW window.
- Tested on macOS, and on a Linux VM without a GPU.

##### [#16705](https://github.com/JabRef/jabref/pull/16705) - Use JavaFX clipboard for CAYW so it works in native image

Highlights:

- The AWT clipboard does not work in the native image. CAYW now uses the JavaFX clipboard.

##### [#16822](https://github.com/JabRef/jabref/pull/16822) - Add native smoke test for JabSrv

Highlights:

- The existing Jersey tests now also run against the native binary. Only the tests that need the GUI or test mocks are excluded.
- The smoke test found a bug in the CAYW endpoint. This PR fixes it.

#### JabGui

##### [#17407](https://github.com/JabRef/jabref/pull/17407) - Add GraalVM native image support for JabGui

Highlights:

- Used [StaticFX](https://github.com/HebiRobotics/jfx-static-feature). It links JavaFX into the binary.
- Added `JabRefViewsFeature`. It registers all JabRef views, also the classes that only FXML and CSS files refer to.
- Replaced the 16,000-line tracing agent output with approximately 270 lines of hand-written metadata.
- All native images now include the 26 UI languages of JabRef.

##### [#17545](https://github.com/JabRef/jabref/pull/17545) - Add native smoke test and automated screenshots for JabGui (open)

Highlights:

- CI starts the native JabGui under Xvfb and makes sure that it runs without errors.
- [OculiX](https://github.com/oculix-org/Oculix) takes screenshots of five views.

### Outside core scope

- [#16422](https://github.com/JabRef/jabref/pull/16422) - Fix build after SvgNode 2.0 upgrade.
- [#16024](https://github.com/JabRef/jabref/pull/16024) - Fix `pdf` subcommand hint: remove non-existent `write-xmp` subcommand.
- Issue [#16087](https://github.com/JabRef/jabref/issues/16087) - `jabkit pdf update --format=xmp` reports success but does not modify existing PDFs (found while testing native `pdf update`).

### Upstream contributions

- PDFBox: `PDDocument` static AWT warm-up fails hard when AWT native libs are unavailable (GraalVM native image). Reported on the [PDFBox users mailing list](https://lists.apache.org/thread/dkvct72z2ltjy1cm73z7x23jyfjrnkxj) with a suggested fix; the maintainer opened the issues and applied the suggestions:
  - [PDFBOX-6214](https://issues.apache.org/jira/browse/PDFBOX-6214) - catch `UnsatisfiedLinkError` (fixed in 2.0.37, 3.0.8).
  - [PDFBOX-6219](https://issues.apache.org/jira/browse/PDFBOX-6219) - follow-up for Linux, catch `NoClassDefFoundError` (fixed in 2.0.38, 3.0.9, 4.0.0).
- Comment on [bell-sw/LibericaNIK#38](https://github.com/bell-sw/LibericaNIK/issues/38) - `libawt_lwawt.dylib` cannot be loaded on macOS. I linked the root cause to [oracle/graal#13272](https://github.com/oracle/graal/issues/13272), confirmed with a GraalVM CE nightly build that the fix works for PDFBox on macOS, and asked when Liberica NIK will include it.

### Tools and repositories developed during GSoC

#### [jabkit-startup-benchmark](https://github.com/wanling0000/jabkit-startup-benchmark)

Measures the startup time and size of each JabKit distribution, and of 29 JDK 25 distributions. Command: `jabkit --help`, measured with [hyperfine](https://github.com/sharkdp/hyperfine) on an Azure Linux VM (2026-10-12).

| Variant                 |    Mean startup | Download | Unpacked / installed |
| :---------------------- | --------------: | -------: | -------------------: |
| Native image            | 2.139 ± 0.207 s |   51 MiB |              136 MiB |
| `java -jar` (JDK 25)    | 4.017 ± 0.321 s |  113 MiB |      113 MiB + a JDK |
| `jpackage` portable     | 4.298 ± 0.424 s |  194 MiB |              250 MiB |
| JBang (`jabkit@jabref`) | 5.280 ± 0.426 s |      N/A |              437 MiB |

#### [jabref-native-env](https://github.com/wanling0000/jabref-native-env)

This repository provides one-shot provisioning of Linux VMs for building and testing selected JabRef native image modules. It installs only the required toolchain and system libraries and includes cloud-init user data for fresh VMs.

#### [jabref-native-lab](https://github.com/wanling0000/jabref-native-lab)

Experiments from before GSoC (March 2026). Each branch tests if one JabLib dependency group (for example, Lucene, PDFBox or JGit) compiles and runs as a native image. These results were the basis of the proposal.

## Future work

### Toolchain and platforms

- Move JabKit and JabSrv from Liberica NIK to GraalVM CE, as in JabGui. JabKit needs trimmed AWT JNI metadata for `pdf update` on Linux. JabSrv can use StaticFX for the CAYW picker.
- Add Windows to the native image smoke tests in the upstream CI.
- Package native binaries of JabLS, JabSrv and JabGui. At the moment, only JabKit is packaged.
- Remove AWT from JabGui. AWT needs JNI metadata for each operating system (see [`pdf update`](#16060---native-image-support-for-search-and-pdf-update)). JabGui still uses AWT for the clipboard and `java.awt.Desktop`: [JabRef/jabref-koppor#764](https://github.com/JabRef/jabref-koppor/issues/764), draft [JabRef/jabref-koppor#786](https://github.com/JabRef/jabref-koppor/pull/786).

### Features that do not work in the native JabGui yet

- Keyring on Linux (probably missing dbus-java metadata).
- AI features (DJL cannot load the PyTorch native library).
- LibreOffice integration.

### Tests

- Finish the JabGui smoke test ([#17545](https://github.com/JabRef/jabref/pull/17545)).
- Add a component test that opens all 95 FXML views in the native image. This makes sure that no view misses metadata.
- Differential GUI tests with the JVM version as the [test oracle](https://en.wikipedia.org/wiki/Test_oracle): do the same steps on the JVM version and on the native binary, and compare the screenshots. This makes sure that both behave the same.

### Metadata, startup time and size

- Remove hand-written entries that the GraalVM metadata repository now supplies, and give metadata back to the libraries (for example, Caffeine and lsp4j). The [JabKit how-to](https://devdocs.jabref.org/code-howtos/jabkit-native-image.html#the-big-picture) explains where the metadata comes from.
- Make the JabKit initialization lazy. Most of the 2.1 seconds are used to create all web fetchers and formats at each start.
- Remove unused code from JabSrv (Jackson 2.x, `java.desktop`, PostgreSQL driver).

### Documentation

- Write a how-to and a decision record (MADR) for the JabGui native image.

## Challenges and important things I learned during the project

### Open source contribution

This year, I also reviewed pull requests from other contributors. I learned how to help move a contribution from its initial proposal through testing and revision to a maintainable result, and how to take responsibility for that result.

### Technical challenges

To make JabRef work with GraalVM Native Image, I had to understand what happens below the application-code level. I strengthened my knowledge of Java runtime behavior and native interoperability. This knowledge includes reflection, resource loading, JNI, native libraries, threads and environment-dependent behavior.

- **How to explore a solution.** There was no ready-made solution for JabRef. I first studied prior art and built the smallest complete path. I then changed one factor at a time and compared the native result with the JVM result. I kept each confirmed result as metadata, a test or documentation.
- **Silent failures.** Missing Native Image metadata does not always cause an error. A feature can instead show no content or do nothing. These failures are difficult to detect.
- **Scope of metadata.** Metadata can register code and resources that Native Image cannot find. It cannot solve problems with native library loading, threads or the execution environment. These problems require different solutions.
- **Tracing agent coverage.** The tracing agent records only the code paths that run during tracing. I generated metadata from the source code where possible. This method does not depend on one tracing run.
- **Metadata ownership.** Each metadata entry must have a clear purpose. It must be in the module that owns the relevant dependency.
- **Interface-specific tests.** Each application needs a smoke test that uses its real interface: CLI, LSP, HTTP or GUI. I used the JVM version as the reference result.
- **Upstream release delays.** A fix in a library or in GraalVM reaches JabRef only after the upstream project releases it. Different release lines can receive the fix at different times.

### Working with AI tools

I used AI tools to read large logs and source code, and to draft changes. I verified each result myself. AI tools can produce drafts faster than a person can review and validate them. How contributors and maintainers should handle this is an open question for me.

## Thanks

I sincerely thank my GSoC mentors [@koppor](https://github.com/koppor/), [@calixtus](https://github.com/calixtus), [@Maran23](https://github.com/Maran23) and [@Stewori](https://github.com/Stewori) for their patience, support and guidance throughout the project. This project had much trial and error, and their reviews, ideas and encouragement in each meeting helped me through it.

I also thank:

- [@Siedlerchr](https://github.com/Siedlerchr) for the support with Azure and with testing.
- [@InAnYan](https://github.com/InAnYan) for testing and feedback. [Your comment](https://github.com/JabRef/jabref/pull/17407#issuecomment-6277664144) on the JabGui pull request was the happiest moment of this project for me.
- [@ThiloteE](https://github.com/ThiloteE) for testing on a computer from 2014. With the native image, JabRef now opens on it in approximately 9 seconds. Before, it sometimes took 20 to 40 seconds.
- [@subhramit](https://github.com/subhramit) for the help with my CV.

When I had problems with my Payoneer payments, you all suggested solutions and gave me moral support. Thank you!

This project also depends on many open-source projects. Special thanks to [Apache PDFBox](https://pdfbox.apache.org/) for the fast fixes, and to [StaticFX](https://github.com/HebiRobotics/jfx-static-feature), which made the native JabGui possible. Thanks also to [GraalVM](https://github.com/oracle/graal), [Native Build Tools](https://github.com/graalvm/native-build-tools), the [GraalVM reachability metadata repository](https://github.com/oracle/graalvm-reachability-metadata), [Liberica NIK](https://bell-sw.com/liberica-native-image-kit/), [picocli](https://github.com/remkop/picocli), [clitest](https://github.com/aureliojargas/clitest), [OculiX](https://github.com/oculix-org/Oculix) and [hyperfine](https://github.com/sharkdp/hyperfine).
