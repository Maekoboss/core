
# Home Assistant Core Development Setup Guide

This guide explains how to set up and maintain a development environment for Home Assistant Core, including forking, syncing, and managing dependencies.

## Git Setup and Fork Management

### Initial Setup
Add upstream repository:
```
# Add the original Home Assistant repository as upstream
git remote add upstream https://github.com/home-assistant/core.git
git remote -v  # Verify remotes
```

### Keeping Fork Updated
1. Fetch upstream changes:
```
# Get the latest changes from original repository
git fetch upstream
```

2. Update your local main branch:
```
# Switch to main branch and update it
git checkout dev
git merge upstream/main   # Or use: git rebase upstream/main
```

3. Push to your fork:
```
# Update your fork on GitHub
git push origin dev
```

## Virtual Environment Setup

### Creating New Virtual Environment
```
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate
```

### Installing Dependencies
```
# Update pip to latest version
pip install --upgrade pip

# Install all required packages
pip install -r requirements_all.txt
pip install -r requirements_test_all.txt

# Install in development mode
pip install -e .
```

## Running Home Assistant

### Command Line
```
# Start Home Assistant with config directory
python3 -m homeassistant -c config
```

### PyCharm Setup and Run Configuration

1. **Open Project**:
   - Open PyCharm
   - Select "Open" and navigate to your cloned `core` directory
   - Wait for PyCharm to index the project

2. **Configure Python Interpreter**:
   - Go to PyCharm → Preferences (or Settings on Windows/Linux)
   - Navigate to Project: core → Python Interpreter
   - Click the gear icon → Add
   - Choose "Virtual Environment" → "Existing"
   - Select the `venv/bin/python` from your project directory
   - Click "OK" to save

3. **Create Run Configuration**:
   - Go to "Run → Edit Configurations..."
   - Click the "+" button and select "Python"
   - Configure as follows:
     * Name: `Home Assistant Dev`
     * Module name: `homeassistant`
     * Parameters: `-c config`
     * Working directory: `$ProjectFileDir$`
     * Python interpreter: Select your virtual environment
   - Click "OK" to save

4. **Running Home Assistant**:
   - Click the green "Run" button in the toolbar, or
   - Use the keyboard shortcut (⌃R on macOS, Shift+F10 on Windows/Linux)
   - Home Assistant will start and the console will show the startup logs

5. **Debugging**:
   - Use the "Debug" button (bug icon) instead of "Run" to start with debugger
   - Set breakpoints by clicking in the left margin of the code
   - Use "Debug" tool window to inspect variables and step through code

## Python Version Update Required

If you encounter the error "Package 'homeassistant' requires a different Python version":

### Using Homebrew (macOS)
```
# Update Homebrew package manager
brew update

# Install/Upgrade Python (adjust version as needed)
brew upgrade python@3.13
```

### Complete Rebuild After Python Update
1. Deactivate and remove old environment:
```
# Clean up existing environment
deactivate
rm -rf venv/
```

2. Create new environment with updated Python:
```
# Create fresh virtual environment
python3 -m venv venv
source venv/bin/activate
```

3. Reinstall all dependencies:
```
# Reinstall all packages
pip install --upgrade pip
pip install -r requirements_all.txt
pip install -r requirements_test_all.txt
pip install -e .
```

## Troubleshooting

### Git Issues
- If you have uncommitted changes:
```
# Save changes temporarily
git stash

# Restore saved changes
git stash pop
```

- If you have merge conflicts:
```
# Cancel merge operation
git merge --abort

# Or if using rebase
git rebase --abort
```

### Dependencies Issues
- Clean build artifacts:
```
# Remove build directories
rm -rf build/
rm -rf homeassistant.egg-info/
```

- Recreate virtual environment:
```
# Full environment reset
deactivate
rm -rf venv/
python3 -m venv venv
source venv/bin/activate
```

### Verification Steps
- Run tests:
```
# Execute test suite
pytest tests/
```

- Check Python version:
```
# Verify Python version
python3 -V
```

## Best Practices
1. Always backup local changes before syncing with upstream
2. Keep your fork updated regularly
3. Create feature branches for changes
4. Run tests after major updates
5. Use virtual environment for isolation
6. Keep Python and dependencies up to date

## Important Notes
- The config directory is in the `config/` folder of your project
- Always activate virtual environment before running commands
- Check requirements.txt for minimum Python version
- Keep git remotes properly configured
- Regularly clean and rebuild when encountering issues