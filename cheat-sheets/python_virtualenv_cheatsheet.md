# Python Virtual Environments Cheat Sheet

A virtual environment isolates a project's Python packages from other projects and the system Python.

## Create and activate
```bash
python3 -m venv .venv
source .venv/bin/activate       # Linux/macOS
# Windows PowerShell:
# py -m venv .venv
# .venv\Scripts\Activate.ps1
```

## Useful commands
```bash
python --version
python -m pip --version
deactivate
```

## Remove
Deactivate, then delete the `.venv` directory. Re-create it when needed.
```bash
deactivate
rm -rf .venv
```

## Best practices
- Use one environment per project; `.venv` is a common name.
- Add `.venv/` to `.gitignore`.
- Activate it before installing packages.
- Save dependencies in `requirements.txt`.

## Example workflow
```bash
cd my-project
python3 -m venv .venv
source .venv/bin/activate
python -m pip install requests
python -m pip freeze > requirements.txt
```
