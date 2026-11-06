# Python pip Cheat Sheet

`pip` installs and manages Python packages, commonly from PyPI.

## Common commands
```bash
python -m pip --version
python -m pip install requests
python -m pip install requests==2.32.0
python -m pip install --upgrade requests
python -m pip uninstall requests
python -m pip list
python -m pip show requests
python -m pip check
```

## Requirements files
```bash
python -m pip freeze > requirements.txt
python -m pip install -r requirements.txt
```

## Best practices
- Prefer `python -m pip` so pip matches the Python interpreter.
- Install project dependencies inside an activated virtual environment.
- Pin versions when reproducible installs matter.
- Avoid installing project packages globally.

## Typical setup
```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```
