# PR summary (GSoC 2026)

Pulled with `gh` on 2026-10-22. Lines = +additions / −deletions.

## JabRef/jabref, merged

| PR | Merged | +/− | Component | Title |
| -- | ------ | --- | --------- | ----- |
| [#15899](https://github.com/JabRef/jabref/pull/15899) | 2026-06-08 | +894 / −0 | JabKit | Add smoke-test CI workflow for JabKit native image |
| [#15959](https://github.com/JabRef/jabref/pull/15959) | 2026-06-11 | +102 / −2 | JabKit | Native image support for check-consistency and check-integrity |
| [#15971](https://github.com/JabRef/jabref/pull/15971) | 2026-06-13 | +57 / −3 | JabKit | Native image support for convert and citationkeys generate |
| [#15973](https://github.com/JabRef/jabref/pull/15973) | 2026-06-15 | +37 / −0 | JabKit | Native image support for `preferences` and `pseudonymize` |
| [#15985](https://github.com/JabRef/jabref/pull/15985) | 2026-06-19 | +71 / −3 | JabKit | Native image support for doi-to-bibtex, fetch, get-cited-works, get-citing-works, generate-bib-from-aux |
| [#15996](https://github.com/JabRef/jabref/pull/15996) | 2026-06-18 | +12 / −28 | JabKit CI | Add Linux to JabKit native smoke test CI matrix |
| [#15997](https://github.com/JabRef/jabref/pull/15997) | 2026-06-18 | +4 / −499 | JabKit | Use picocli-codegen to auto-generate reflection metadata |
| [#16024](https://github.com/JabRef/jabref/pull/16024) | 2026-06-21 | +5 / −5 | JabKit (side fix) | Fix pdf subcommand hint: remove non-existent write-xmp |
| [#16060](https://github.com/JabRef/jabref/pull/16060) | 2026-07-02 | +139 / −3 | JabKit | Native image support for search and pdf update (switch to Liberica NIK) |
| [#16144](https://github.com/JabRef/jabref/pull/16144) | 2026-07-03 | +57 / −0 | JabLib | Register Caffeine weakKeys cache classes for native image |
| [#16135](https://github.com/JabRef/jabref/pull/16135) | 2026-07-30 | +123 / −0 | Docs | Add JabKit native-image how-to |
| [#16327](https://github.com/JabRef/jabref/pull/16327) | 2026-07-26 | +2 / −0 | Docs | Document non-AWT JabKit native usage |
| [#16267](https://github.com/JabRef/jabref/pull/16267) | 2026-07-30 | +1572 / −1 | JabLS | Enable GraalVM Native Image builds for JabLS |
| [#16422](https://github.com/JabRef/jabref/pull/16422) | 2026-07-29 | +3 / −3 | Build (side fix) | Fix build after SvgNode 2.0 upgrade |
| [#16462](https://github.com/JabRef/jabref/pull/16462) | 2026-08-03 | +678 / −0 | JabLS | Add native-image smoke test for JabLS |
| [#16565](https://github.com/JabRef/jabref/pull/16565) | 2026-08-14 | +1466 / −1 | JabSrv | Add GraalVM Native Image support for JabSrv |
| [#16634](https://github.com/JabRef/jabref/pull/16634) | 2026-08-27 | +281 / −3 | JabSrv / CAYW | Run the CAYW JavaFX picker in the native jabsrv image |
| [#16705](https://github.com/JabRef/jabref/pull/16705) | 2026-08-28 | +13 / −8 | CAYW | Use JavaFX clipboard for CAYW so it works in native image |
| [#16822](https://github.com/JabRef/jabref/pull/16822) | 2026-09-03 | +218 / −19 | JabSrv | Add native smoke test for JabSrv |
| [#17407](https://github.com/JabRef/jabref/pull/17407) | 2026-10-24 | +1942 / −33 | JabGui | Add GraalVM native image support for JabGui |

**Total merged: 20 PRs, +7676 / −611.**

## JabRef/jabref, open

| PR | Opened | +/− | Component | Title |
| -- | ------ | --- | --------- | ----- |
| [#17545](https://github.com/JabRef/jabref/pull/17545) | 2026-10-22 | +1838 / −18 | JabGui | Add native smoke test and automated screenshots for jabgui |

## JabRef/jabref, closed without merge (superseded)

Usually not listed in the wiki. Kept here for reference.

| PR | Title | Superseded by |
| -- | ----- | ------------- |
| [#15695](https://github.com/JabRef/jabref/pull/15695) | Bootstrap JabKit GraalVM native-image support | TODO (#15899 + per-command PRs?) |
| [#16265](https://github.com/JabRef/jabref/pull/16265) | Run JabKit PDF native smoke tests with GraalVM on macOS | TODO |
| [#16436](https://github.com/JabRef/jabref/pull/16436) | Initiate jabls smoke test | #16462 |
| [#16566](https://github.com/JabRef/jabref/pull/16566) | Add native smoke test for JabSrv | #16822 |

## Issues

- [JabRef/jabref#16087](https://github.com/JabRef/jabref/issues/16087) - `jabkit pdf update --format=xmp` reports success but does not modify existing PDFs (closed)

## Outside JabRef/jabref

| Where | What |
| ----- | ---- |
| [PDFBox users mailing list](https://lists.apache.org/thread/dkvct72z2ltjy1cm73z7x23jyfjrnkxj) | My report + suggested fix (2026-07); maintainer Tilman Hausherr opened the two issues below |
| [PDFBOX-6214](https://issues.apache.org/jira/browse/PDFBOX-6214) | Fixed in 2.0.37 / 3.0.8; commits: "catch UnsatisfiedLinkError as suggested by Wanling Fu" |
| [PDFBOX-6219](https://issues.apache.org/jira/browse/PDFBOX-6219) | Linux follow-up, fixed in 2.0.38 / 3.0.9 / 4.0.0; commits: "catch NoClassDefFoundError as suggested by Wanling Fu" |
| [bell-sw/LibericaNIK#38](https://github.com/bell-sw/LibericaNIK/issues/38) | Comment on macOS `libawt_lwawt.dylib` loading issue |
| [wanling0000/jabkit-startup-benchmark](https://github.com/wanling0000/jabkit-startup-benchmark) | Startup time + download size benchmark (created 2026-07-06) |
| [wanling0000/jabref-native-env](https://github.com/wanling0000/jabref-native-env) | Native build env provisioning; PR #2 cloud-init (merged) |
| [wanling0000/jabref-native-lab](https://github.com/wanling0000/jabref-native-lab) | Pre-GSoC dependency experiments (created 2026-03-25) |
| [JabRef/blog.jabref.org#126](https://github.com/JabRef/blog.jabref.org/pull/126) | 2025 blog post (Git integration). For reference: the 2026 blog post would go in the same repo |
| wanling0000/jabref#14, #15 (fork) | CI experiments: jabgui native smoke test, Windows debug (not for the report) |
