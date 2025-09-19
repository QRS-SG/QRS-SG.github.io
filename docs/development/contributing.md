# Contributing to QRS

Thank you for your interest in contributing to QRS! This guide will help you get started with contributing to the project.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Contributing Guidelines](#contributing-guidelines)
- [Code Style](#code-style)
- [Testing](#testing)
- [Documentation](#documentation)
- [Submitting Changes](#submitting-changes)

## Getting Started

### Prerequisites

Before contributing, ensure you have:

- Python 3.8 or higher
- Git
- A GitHub account
- Basic knowledge of Python development

### Fork and Clone

1. Fork the repository on GitHub
2. Clone your fork locally:

```bash
git clone https://github.com/YOUR_USERNAME/QRS-SG.github.io.git
cd QRS-SG.github.io
```

3. Add the upstream repository:

```bash
git remote add upstream https://github.com/QRS-SG/QRS-SG.github.io.git
```

## Development Setup

### 1. Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
pip install -r requirements-dev.txt  # Development dependencies
```

### 3. Install QRS in Development Mode

```bash
pip install -e .
```

### 4. Verify Installation

```bash
qrs --version
```

## Contributing Guidelines

### Types of Contributions

We welcome various types of contributions:

- **Bug Fixes**: Fix existing issues
- **New Features**: Add new functionality
- **Documentation**: Improve or add documentation
- **Tests**: Add or improve test coverage
- **Performance**: Optimize existing code
- **Refactoring**: Improve code structure

### Before You Start

1. Check existing issues and pull requests
2. Discuss major changes in an issue first
3. Ensure your changes align with project goals
4. Follow the coding standards

## Code Style

### Python Code Style

We follow PEP 8 with some modifications:

- Line length: 100 characters
- Use type hints where appropriate
- Follow Google docstring style
- Use meaningful variable and function names

### Code Formatting

We use `black` for code formatting:

```bash
black qrs/
```

### Linting

We use `flake8` for linting:

```bash
flake8 qrs/
```

### Import Organization

Use `isort` for import organization:

```bash
isort qrs/
```

## Testing

### Running Tests

Run all tests:

```bash
pytest
```

Run specific test files:

```bash
pytest tests/test_processors.py
```

Run with coverage:

```bash
pytest --cov=qrs --cov-report=html
```

### Writing Tests

- Write tests for new functionality
- Aim for high test coverage
- Use descriptive test names
- Test both success and failure cases

### Test Structure

```python
import pytest
from qrs.processors import DataProcessor

class TestDataProcessor:
    def test_basic_processing(self):
        processor = DataProcessor({"type": "filter"})
        data = [{"status": "active"}, {"status": "inactive"}]
        result = processor.process(data)
        assert len(result) == 1
        assert result[0]["status"] == "active"
    
    def test_invalid_config(self):
        with pytest.raises(ValidationError):
            DataProcessor({"invalid": "config"})
```

## Documentation

### Code Documentation

- Use Google-style docstrings
- Document all public functions and classes
- Include type hints
- Provide usage examples

### Example Docstring

```python
def process_data(data: List[Dict], config: Dict) -> List[Dict]:
    """Process data according to configuration.
    
    Args:
        data: List of dictionaries to process
        config: Processing configuration
        
    Returns:
        List of processed dictionaries
        
    Raises:
        ValidationError: If configuration is invalid
        ProcessingError: If data processing fails
        
    Example:
        >>> data = [{"status": "active"}]
        >>> config = {"type": "filter"}
        >>> result = process_data(data, config)
        >>> print(result)
        [{"status": "active"}]
    """
```

### Documentation Updates

- Update relevant documentation for new features
- Fix documentation errors
- Add examples and tutorials
- Keep API documentation current

## Submitting Changes

### 1. Create a Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-number
```

### 2. Make Changes

- Write your code following the guidelines
- Add tests for new functionality
- Update documentation as needed
- Ensure all tests pass

### 3. Commit Changes

Use descriptive commit messages:

```bash
git add .
git commit -m "Add new data processor for CSV files

- Implement CSVProcessor class
- Add support for custom delimiters
- Include comprehensive tests
- Update documentation"
```

### 4. Push Changes

```bash
git push origin feature/your-feature-name
```

### 5. Create Pull Request

1. Go to your fork on GitHub
2. Click "New Pull Request"
3. Fill out the pull request template
4. Request review from maintainers

## Pull Request Guidelines

### Pull Request Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Refactoring

## Testing
- [ ] Tests pass locally
- [ ] New tests added for new functionality
- [ ] All existing tests still pass

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No breaking changes (or documented)
```

### Review Process

1. Automated checks must pass
2. Code review by maintainers
3. Address feedback and suggestions
4. Maintainers will merge when ready

## Issue Guidelines

### Reporting Bugs

When reporting bugs, include:

- Clear description of the issue
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, Python version, etc.)
- Relevant logs or error messages

### Feature Requests

For feature requests, include:

- Clear description of the feature
- Use case and motivation
- Proposed implementation (if applicable)
- Any alternatives considered

## Development Workflow

### Daily Workflow

1. Sync with upstream:
```bash
git fetch upstream
git checkout main
git merge upstream/main
```

2. Create feature branch:
```bash
git checkout -b feature/new-feature
```

3. Make changes and test:
```bash
# Make changes
pytest
black qrs/
flake8 qrs/
```

4. Commit and push:
```bash
git add .
git commit -m "Descriptive message"
git push origin feature/new-feature
```

### Release Process

1. Update version numbers
2. Update changelog
3. Create release branch
4. Tag release
5. Deploy to PyPI

## Community Guidelines

### Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and grow
- Follow the golden rule

### Getting Help

- Check existing issues and discussions
- Ask questions in GitHub Discussions
- Join our community chat
- Contact maintainers directly

## Recognition

Contributors will be recognized in:

- CONTRIBUTORS.md file
- Release notes
- Project documentation
- Community acknowledgments

Thank you for contributing to QRS! Your efforts help make this project better for everyone.
