# aiwaf-rust

`aiwaf-rust` is a Rust extension module for Python that provides fast request-header and behavior heuristics for WAF-style detection.

- PyPI package: `aiwaf-rust`
- Python import: `aiwaf_rust`
- Built with: `PyO3` + `maturin`

## Features

- Header validation with configurable required headers and scoring
- Request feature extraction for downstream detection logic
- Recent behavior analysis for suspicious scanning patterns
- Cross-platform wheel publishing (Linux, macOS, Windows)

## Installation

### From PyPI

```bash
pip install aiwaf-rust
```

### Local development install

```bash
pip install maturin
maturin develop
```

## Python API

```python
import aiwaf_rust

# 1) Basic header validation
reason = aiwaf_rust.validate_headers({
    "HTTP_USER_AGENT": "Mozilla/5.0",
    "HTTP_ACCEPT": "text/html"
})

# 2) Configurable validation
reason = aiwaf_rust.validate_headers_with_config(
    {
        "HTTP_USER_AGENT": "Mozilla/5.0",
        "HTTP_ACCEPT": "text/html"
    },
    ["HTTP_USER_AGENT", "HTTP_ACCEPT"],
    3,
)

# 3) Feature extraction
features = aiwaf_rust.extract_features(
    {
        "ip": "1.2.3.4",
        "path_lower": "/wp-admin",
        "path_len": 9,
        "timestamp": 1700000000.0,
        "response_time": 0.03,
        "status_idx": 3,
        "kw_check": True,
        "total_404": 5,
    },
    []
)

# 4) Behavior analysis
analysis = aiwaf_rust.analyze_recent_behavior([], "1.2.3.4")
```

## Build and Test

```bash
# Run Rust tests
cargo test

# Build Python wheel locally
maturin build --release --out dist

# Build source distribution
maturin sdist --out dist
```

## Publishing

Publishing is handled by GitHub Actions using trusted publishing.

Workflow: `.github/workflows/publish.yml`

Trigger conditions:
- `release.published`
- `workflow_dispatch`

The workflow builds:
- Wheels for Python `3.8` to `3.12` on `ubuntu-latest`, `macos-latest`, `windows-latest`
- One source distribution (`sdist`)

Then it publishes to PyPI using:
- `pypa/gh-action-pypi-publish@release/v1`
- OIDC (`id-token: write`)

## Compatibility Policy

`aiwaf-rust` and `aiwaf` should be versioned together when possible.

Recommended policy:
- Match `major.minor` between `aiwaf` and `aiwaf-rust`
- Use patch versions independently for bug fixes
- Document compatibility here when versions diverge

| aiwaf | aiwaf-rust |
| --- | --- |
| 0.1.x | 0.1.x |

## Development Notes

- Module name in Rust and Python is `aiwaf_rust`
- `pyproject.toml` is the source of Python package metadata
- `Cargo.toml` is the source of Rust crate metadata

## License

MIT. See `LICENSE`.
