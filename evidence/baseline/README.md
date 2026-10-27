# Baseline image evidence (Stage 01, step 2)

|                         | web                                                                                                 | votes                                                                                                 |
| ----------------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Image                   | `ghcr.io/dilnat/lab404-web@sha256:ded2af7e994dcaad35e4db88ae8443628a9cb36316200e893800ed0105cc5038` | `ghcr.io/dilnat/lab404-votes@sha256:e725fa32f7d09df8994e58903d52308a3808047d4d222d31c554595037618b3d` |
| Base image              | `python:3.14` (Debian 13, full)                                                                     | `golang:1.26` (Debian 13, full)                                                                       |
| Size (uncompressed)     | 1.15 GB                                                                                             | 1.29 GB                                                                                               |
| Configured user         | none (root)                                                                                         | none (root)                                                                                           |
| Build                   | single stage                                                                                        | single stage, toolchain shipped                                                                       |
| Shell / tools present   | `sh`, `bash`, `apt`, `gcc`, `pip`                                                                   | `sh`, `bash`, `apt`, `gcc`, `go`                                                                      |
| SBOM components         | 519                                                                                                 | 261                                                                                                   |
| Vulnerabilities (total) | 3834                                                                                                | 1990                                                                                                  |
| CRITICAL / HIGH         | 17 / 454                                                                                            | 3 / 153                                                                                               |
| MEDIUM / LOW / UNKNOWN  | 1380 / 1833 / 150                                                                                   | 782 / 1001 / 51                                                                                       |
| From OS packages        | 3824                                                                                                | 1974                                                                                                  |
| From application deps   | 10 (Python)                                                                                         | 16 (Go binary)                                                                                        |

- Both images run as root and ship a shell, a package manager, and a compiler.
- Almost all findings come from the Debian base layers, not from the
  application. A minimal runtime image removes most of them.
- The largest layers are build tooling: 657 MB of `-dev` packages in `web`,
  287 MB of Go build cache plus 233 MB of Go toolchain in `votes`.
- Application dependencies with fixes available: `urllib3`, `msgpack`,
  `setuptools`, `requests`, `Flask`, and `opentelemetry-api` in `web`;
  `golang.org/x/net`, `golang.org/x/text`, `google.golang.org/grpc`, and
  OpenTelemetry modules in `votes`.
- No base image is pinned by digest and `requirements.txt` has no hashes.
