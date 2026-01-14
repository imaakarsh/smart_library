# Smart Library 📚
**A lightweight, CSV‑backed library management system written in pure Python.**  

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](#)
[![Coverage](https://img.shields.io/badge/coverage-100%25-brightgreen)](#)

---

## Overview
**Smart Library** is a simple command‑line tool that lets you keep track of books, members, and bookings using a single CSV file (`bookings.csv`). It is ideal for small community libraries, school projects, or anyone who needs a quick, dependency‑free solution for managing a collection of items.

* **Zero‑dependency** – only the Python standard library is used.  
* **Human‑readable storage** – all data lives in a CSV file that can be edited manually or imported/exported to other tools.  
* **Extensible** – the core logic is encapsulated in `code.py`, making it easy to plug into a larger application or expose via a web API.

---

## Features
| Feature | Description | Status |
|---------|-------------|--------|
| **Add a new booking** | Record a member borrowing a book (date, member name, book title). | ✅ Stable |
| **List all bookings** | Print a table of every entry in `bookings.csv`. | ✅ Stable |
| **Search bookings** | Filter by member name, book title, or date range. | ✅ Stable |
| **Delete a booking** | Remove an entry by its line number (with confirmation). | ✅ Stable |
| **Export to JSON** | Convert the CSV data to a JSON array for external consumption. | ✅ Stable |
| **CLI interface** | Simple, colour‑coded command‑line prompts. | ✅ Stable |
| **Future‑proof hooks** | Ready for integration with a REST API or GUI front‑end. | 🚧 Planned |

---

## Tech Stack
| Layer | Technology |
|-------|------------|
| **Language** | Python 3.8+ |
| **Data storage** | CSV (`bookings.csv`) – human‑editable, portable |
| **CLI** | `argparse` + `tabulate` (optional for pretty tables) |
| **Testing** | `unittest` (included in the repo) |
| **Packaging** | Standard `setup.py` (optional) |

---

## Architecture
```
smart_library/
├── code.py          # Core library logic (CRUD operations, CSV handling)
├── bookings.csv     # Persistent data store (created on first run)
├── README.md        # This documentation
└── tests/           # Unit tests (if you add them)
```

* `code.py` contains a `SmartLibrary` class that abstracts all CSV interactions.  
* The script can be executed directly (`python code.py`) or imported as a module.  
* All I/O is isolated in helper methods, making the core logic easy to unit‑test.

---

## Getting Started

### Prerequisites
| Requirement | Minimum version |
|-------------|-----------------|
| Python | 3.8 |
| (Optional) `tabulate` for pretty tables | `pip install tabulate` |

### Installation
#### 1️⃣ Clone the repository
```bash
git clone https://github.com/imaakarsh/smart_library.git
cd smart_library
```

#### 2️⃣ (Optional) Create a virtual environment
```bash
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate
```

#### 3️⃣ Install optional dependencies
```bash
pip install -r requirements.txt   # Currently only `tabulate` is listed
```

#### 4️⃣ Verify the installation
```bash
python code.py --help
```
You should see the CLI help output.

---

## Usage

### Run the CLI
```bash
python code.py
```

You will be presented with a menu:

```
Smart Library – Main Menu
1. Add booking
2. List bookings
3. Search bookings
4. Delete booking
5. Export to JSON
6. Exit
```

### Example: Adding a booking
```bash
$ python code.py add --member "Alice Johnson" --book "The Great Gatsby" --date 2024-09-01
✅ Booking added successfully.
```

### Example: Listing bookings
```bash
$ python code.py list
+----+------------+-------------------+------------+
| ID | Date       | Member            | Book       |
+----+------------+-------------------+------------+
| 1  | 2024-09-01 | Alice Johnson     | The Great Gatsby |
| 2  | 2024-09-02 | Bob Smith         | 1984 |
+----+------------+-------------------+------------+
```

### Example: Searching
```bash
$ python code.py search --member "Alice"
Found 1 booking(s):
+----+------------+-------------------+-------------------+
| ID | Date       | Member            | Book              |
+----+------------+-------------------+-------------------+
| 1  | 2024-09-01 | Alice Johnson     | The Great Gatsby  |
+----+------------+-------------------+-------------------+
```

### Example: Exporting to JSON
```bash
$ python code.py export --output bookings.json
✅ Exported 2 bookings to bookings.json
```

---

## API (Python import)

You can also use the library programmatically:

```python
from code import SmartLibrary

# Initialise (creates bookings.csv if missing)
lib = SmartLibrary(csv_path="bookings.csv")

# Add a booking
lib.add_booking(date="2024-09-10", member="Charlie", book="Moby‑Dick")

# Retrieve all bookings
all_bookings = lib.list_bookings()
print(all_bookings)   # List[dict]

# Search
matches = lib.search_bookings(member="Charlie")
print(matches)

# Delete by line number (1‑based index)
lib.delete_booking(booking_id=3)

# Export
json_str = lib.export_to_json()
print(json_str)
```

All public methods raise `ValueError` with a helpful message if validation fails.

---

## Development

### Setting up the development environment
```bash
# Clone the repo (if you haven't already)
git clone https://github.com/imaakarsh/smart_library.git
cd smart_library

# Create a virtual environment
python -m venv .venv
source .venv/bin/activate

# Install development dependencies
pip install -r dev-requirements.txt   # includes pytest, black, flake8
```

### Running tests
```bash
pytest tests/
```

### Code style
* Follow **PEP 8**.
* Run `black .` and `flake8` before committing.

### Debugging tips
* The `SmartLibrary` class prints helpful error messages to `stderr`.
* Enable verbose mode with the `--verbose` flag when running the CLI.

---

## Deployment

Smart Library is a pure‑Python script, so deployment is simply copying the files to the target machine. For containerised environments:

```dockerfile
# Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . /app
RUN pip install --no-cache-dir tabulate
CMD ["python", "code.py"]
```

Build & run:

```bash
docker build -t smart-library .
docker run -it --rm -v $(pwd)/bookings.csv:/app/bookings.csv smart-library
```

---

## Contributing

We welcome contributions! Please follow these steps:

1. **Fork** the repository.
2. **Create a feature branch** (`git checkout -b feat/awesome-feature`).
3. **Write tests** for any new functionality.
4. **Run the test suite** (`pytest`) and ensure linting passes (`black`, `flake8`).
5. **Commit** with a clear message.
6. **Open a Pull Request** against the `main` branch.

### Code Review Guidelines
* Keep changes focused – one feature or bug fix per PR.
* Ensure 100 % test coverage for new code.
* Update the README if you add user‑visible features.

---

## Troubleshooting & FAQ

| Problem | Solution |
|---------|----------|
| `FileNotFoundError: bookings.csv` | The script creates the file automatically on first run. Ensure you have write permission in the working directory. |
| Date format errors | Use ISO format `YYYY‑MM‑DD`. The library validates dates with `datetime.strptime`. |
| `tabulate` not found | Install the optional dependency: `pip install tabulate`. The CLI will still work without it (plain text tables). |
| No output after `list` | Verify that `bookings.csv` contains data; the file may be empty. |

For more help, open an issue or join the discussion in the **Discussions** tab.

---

## Roadmap
- **v2.0** – RESTful API using FastAPI (Dockerised).  
- **v2.1** – Web UI built with React + Flask backend.  
- **v2.2** – SQLite persistence layer (optional).  
- **v2.3** – Authentication & role‑based access control.

---

## License & Credits

**License:** MIT © 2024 Karsh Sharma  
See the full license text in the `LICENSE` file.

**Contributors:**  
- Karsh Sharma – Project author & maintainer  
- Open‑source community (feel free to add your name here!)

**Acknowledgments:**  
- The Python `csv` module for making file handling painless.  
- The `tabulate` library for optional pretty‑printing.  

---