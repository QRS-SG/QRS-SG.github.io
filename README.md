# QRS Documentation

This repository contains the documentation for QRS, built with MkDocs and deployed to GitHub Pages.

## Project Structure

```
.
├── docs/                    # Documentation source files
│   ├── index.md            # Home page
│   ├── getting-started/    # Getting started guides
│   ├── user-guide/         # User documentation
│   └── development/        # Development guides
├── .github/workflows/      # GitHub Actions workflows
├── mkdocs.yml             # MkDocs configuration
├── requirements.txt        # Python dependencies
├── build-docs.sh          # Build script
└── README.md              # This file
```

## Local Development

### Prerequisites

- Python 3.8 or higher
- pip

### Setup

1. Clone the repository:
```bash
git clone https://github.com/QRS-SG/QRS-SG.github.io.git
cd QRS-SG.github.io
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Serve documentation locally:
```bash
mkdocs serve
```

The documentation will be available at `http://localhost:8000`.

### Building Documentation

To build the documentation:

```bash
./build-docs.sh
```

This will:
- Build the MkDocs documentation
- Place it in the `site/documentation` directory
- Copy main site files to the root
- Prepare everything for GitHub Pages deployment

## Documentation Structure

### Getting Started
- [Installation](docs/getting-started/installation.md) - Installation instructions
- [Quick Start](docs/getting-started/quick-start.md) - Quick start guide

### User Guide
- [Overview](docs/user-guide/overview.md) - Comprehensive usage guide
- [Configuration](docs/user-guide/configuration.md) - Configuration reference
- [API Reference](docs/user-guide/api-reference.md) - API documentation

### Development
- [Contributing](docs/development/contributing.md) - Contributing guidelines
- [Changelog](docs/development/changelog.md) - Release history

## Deployment

The documentation is automatically deployed to GitHub Pages when changes are pushed to the `main` or `establish-documentation` branches.

### Manual Deployment

1. Build the documentation:
```bash
./build-docs.sh
```

2. Commit and push changes:
```bash
git add site/
git commit -m "Update documentation"
git push origin main
```

## Configuration

The documentation is configured in `mkdocs.yml`. Key settings:

- **Theme**: Material Design theme
- **Site URL**: `https://qrs-sg.github.io/documentation/`
- **Repository**: `https://github.com/QRS-SG/QRS-SG.github.io`

## Contributing

To contribute to the documentation:

1. Make changes to files in the `docs/` directory
2. Test locally with `mkdocs serve`
3. Submit a pull request

For more information, see [Contributing Guidelines](docs/development/contributing.md).

## License

This documentation is licensed under the MIT License.
