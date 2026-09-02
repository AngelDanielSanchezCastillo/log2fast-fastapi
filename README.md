# log2fast-fastapi

🚀 Advanced logging module for FastAPI with file rotation, colored output, and environment-based auto-configuration

> 📖 **Conventions reference**: this package follows the [2fast-handbook](https://github.com/AngelDanielSanchezCastillo/2fast-handbook) for ecosystem conventions (structure, versioning, README, commits, release).

> [!WARNING]
> **Internal Use Notice**
> 
> This package is designed and maintained by the **Solautyc Team** for internal use. While it is publicly available, it may not work as expected in all environments or use cases outside of our specific infrastructure. We do not provide support or guarantees for external usage, and we are not responsible for any issues that may arise from using this package in other contexts.
> 
> Use at your own risk. Contributions and feedback are welcome, but compatibility with external environments is not guaranteed.

## Features

- 🎨 **Multiple Output Formats**: JSON (production), Colored (development), Structured (debugging), Simple (testing)
- 🌍 **Environment-Based Configuration**: Automatic setup for dev, test, prod, and debug
- 📦 **Module-Based Loggers**: Each module gets its own logger instance
- 🔄 **File Rotation**: Automatic log file rotation with configurable size
- 🔄 **Environment-Specific Logging**: Control which logs appear in which environments (prevent sensitive data leaks)
- 🚀 **FastAPI Integration**: Middleware for automatic request/response logging with unique request IDs
- 📊 **Structured Logging**: Add context data to any log message
- 📊 **Context Injection**: Support for request_id, user_id, and custom context data
- 🎯 **Zero Configuration**: Works out of the box with sensible defaults

## 📚 Documentation

- **[Usage Guide](docs/usage.md)** - Comprehensive usage guide with examples
- **[File Management](docs/file_management.md)** - Complete guide on log rotation and storage (English)
- **[Gestión de Archivos](docs/file_management_es.md)** - Guía completa de rotación y almacenamiento (Español)
- **[Logger Best Practices](docs/logger_best_practices.md)** - Best practices for creating and naming loggers


## Installation

### From PyPI (Recommended)

```bash
pip install log2fast-fastapi
```

### From Source

```bash
# Clone the repository
git clone https://github.com/AngelDanielSanchezCastillo/log2fast-fastapi.git
cd log2fast-fastapi

# Install in development mode
pip install -e .

# Or install with dev dependencies
pip install -e ".[dev]"
```


## Quick Start

### Basic Usage

```python
from log2fast_fastapi import get_logger

logger = get_logger(__name__)

logger.info("Application started")
logger.warning("This is a warning")
logger.error("An error occurred")
```

### FastAPI Integration

```python
from fastapi import FastAPI
from log2fast_fastapi import RequestLoggingMiddleware, get_logger

app = FastAPI()
app.add_middleware(RequestLoggingMiddleware)

logger = get_logger(__name__)

@app.get("/")
async def root():
    logger.info("Root endpoint accessed")
    return {"message": "Hello World"}
```

### Logging with Context

```python
from log2fast_fastapi import get_logger

logger = get_logger(__name__)

logger.info(
    "User logged in",
    extra_data={
        "user_id": "12345",
        "ip_address": "192.168.1.1"
    }
)
```

### Environment-Specific Logging (Prevent Sensitive Data Leaks!)

```python
# Logs ONLY in development/debug (NOT in production)
logger.debug(
    "Sensitive debug info",
    extra_data={"password_hash": "...", "token": "..."},
    only_in=["development", "debug"]
)

# Logs ONLY in production
logger.info(
    "Performance metrics",
    extra_data={"response_time": 120},
    only_in=["production"]
)
```

## Configuration

**Simple: Just set the environment in `.env`**

```bash
# That's it! Format and level auto-configure
LOG_ENVIRONMENT=production
```

Auto-configuration by environment:

| Environment | Auto Level | Auto Format |
|------------|-----------|-------------|
| `development` | INFO | colored |
| `production` | WARNING | json |
| `testing` | INFO | simple |
| `debug` | DEBUG | colored |

**Optional: Override defaults**


```bash
# Optional: Override auto-configuration
LOG_LEVEL=DEBUG
LOG_FORMAT=json
LOG_FILE_SETTINGS__ENABLED=true
```

## Environment Presets

### Development
- Format: Colored console output
- Level: INFO
- Perfect for local development

### Production
- Format: JSON (structured)
- Level: WARNING
- Optimized for log aggregation tools

### Testing
- Format: Simple
- Level: INFO
- Minimal output for tests

### Debug
- Format: Colored
- Level: DEBUG
- Maximum verbosity

## Documentation

See [docs/usage.md](docs/usage.md) for complete documentation including:
- Advanced configuration
- Custom formatters
- Best practices
- Integration examples

## Example

Run the example application:

```bash
python log2fast_fastapi/example.py
```

Then visit:
- http://localhost:8000/ - Root endpoint
- http://localhost:8000/users/123 - User endpoint
- http://localhost:8000/docs - API documentation

## Testing

Run the test suite:

```bash
python log2fast_fastapi/tests/test_logging.py
```

## Module Structure

```
log2fast-fastapi/
├── pyproject.toml       # Package configuration
├── MANIFEST.in          # Additional files to include
├── README.md            # This file
├── LICENSE              # License file
├── src/
│   └── log2fast_fastapi/
│       ├── __init__.py          # Main exports
│       ├── __version__.py       # Version information
│       ├── base.py              # Core FastLogger class
│       ├── settings.py          # Configuration with Pydantic
│       ├── formatters.py        # Custom log formatters
│       └── middleware.py        # FastAPI middleware
├── docs/
│   ├── usage.md                 # Complete documentation
│   ├── file_management.md       # File rotation guide (EN)
│   ├── file_management_es.md    # File rotation guide (ES)
│   ├── logger_best_practices.md # Best practices
│   └── publishing.md            # PyPI publishing guide
├── examples/
│   ├── example.py               # Basic example
│   ├── demo_features.py         # Feature demonstrations
│   └── demo_rotation.py         # Rotation examples
└── tests/
    ├── test_logging.py          # Test suite
    └── test_new_features.py     # Feature tests
```

## Acknowledgments

This project uses the following open-source packages:

- [FastAPI](https://github.com/tiangolo/fastapi) - Modern, fast web framework for building APIs (MIT License)
- [Pydantic](https://github.com/pydantic/pydantic) - Data validation using Python type annotations (MIT License)
- [Pydantic Settings](https://github.com/pydantic/pydantic-settings) - Settings management using Pydantic (MIT License)

We are grateful to the maintainers and contributors of these projects for their excellent work.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Copyright (c) 2026 Angel Daniel Sanchez Castillo

**Note**: This package is designed and maintained by the Solautyc Team for internal use. While publicly available under MIT license, use at your own risk.
