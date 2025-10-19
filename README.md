# Gensyn BlockAssist - Windows 11 WSL2 Installation Guide

A comprehensive guide to get Gensyn BlockAssist running on Windows 11 using Windows Subsystem for Linux.

---

## 📋 What You'll Need

Before starting the installation:

| Requirement | Details |
|------------|---------|
| **OS** | Windows 11 with WSL2 enabled |
| **Linux Distro** | Ubuntu 22.04 LTS (recommended) |
| **RAM** | 12GB minimum, 32GB recommended |
| **Storage** | At least 5GB free space |
| **Accounts** | Hugging Face ([sign up](https://huggingface.co/join)) with API token ([get token](https://huggingface.co/settings/tokens)) |
| | Gensyn Testnet account (created during setup) |

---

## ✅ Pre-Installation Checklist

Before starting, verify you have:

- [ ] Windows 11 installed and updated
- [ ] Administrator access to your computer
- [ ] Stable internet connection
- [ ] At least 10GB free disk space confirmed
- [ ] Hugging Face account created ([sign up here](https://huggingface.co/join))
- [ ] Hugging Face API token generated and saved ([get token](https://huggingface.co/settings/tokens))
- [ ] 30-45 minutes for complete installation

---

## 🚀 Quick Start Installation

### Part 1: WSL Environment Setup

#### Enable WSL2 on Windows 11

Open PowerShell as Administrator:

```powershell
wsl --install -d Ubuntu-22.04
```

Restart your computer when prompted. Open Ubuntu from the Start menu and set up your username/password.

#### Update System Packages

In Ubuntu terminal:

```bash
# Update package lists
sudo apt update
```

```bash
# Install build tools, libraries, and Java
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev \
libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev zip unzip openjdk-8-jdk
```

Verify Java installation:

```bash
java -version
```

You should see output showing `openjdk version "1.8.x"`.

---

### Part 2: Python Environment Setup

#### Install pyenv for Python Management

```bash
# Download and install pyenv
curl -fsSL https://pyenv.run | bash
```

```bash
# Configure pyenv in your shell
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

```bash
# Reload shell configuration
source ~/.bashrc
```

#### Install Python 3.10

```bash
# Install Python 3.10.0 via pyenv
pyenv install 3.10.0
```

```bash
# Set Python 3.10.0 as default version
pyenv global 3.10.0
```

Verify Python installation:

```bash
python --version
```

Should show: `Python 3.10.0`

---

### Part 3: Node.js and JavaScript Tools

#### Install Node Version Manager (nvm)

```bash
# Install Node Version Manager
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

```bash
# Load nvm into current session
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```

#### Install Node.js and Yarn

```bash
# Install latest LTS version of Node.js
nvm install --lts
```

```bash
# Set LTS as default
nvm alias default 'lts/*'
```

```bash
# Use default Node version
nvm use default
```

```bash
# Enable Corepack for Yarn
corepack enable
```

```bash
# Install Yarn package manager
corepack prepare yarn@stable --activate
```

Verify installations:

```bash
node -v
```

```bash
npm -v
```

```bash
yarn -v
```

---

### Part 4: BlockAssist Installation

#### Get BlockAssist Source Code

```bash
# Remove old installation if it exists
rm -rf ~/blockassist
```

```bash
# Clone BlockAssist repository
git clone https://github.com/gensyn-ai/blockassist.git
```

```bash
# Navigate to blockassist directory
cd blockassist
```

#### Run Initial Setup

```bash
# Make setup script executable
chmod +x setup.sh
```

```bash
# Run setup script (installs Java dependencies)
./setup.sh
```

#### Set Up Python Virtual Environment

```bash
# Create isolated Python environment
python3 -m venv blockassist-venv
```

```bash
# Activate virtual environment
source blockassist-venv/bin/activate
```

```bash
# Upgrade pip and build tools
python -m pip install --upgrade pip setuptools wheel
```

```bash
# Install BlockAssist in editable mode (allows importing malmo and other packages)
pip install -e .
```

```bash
# Install additional required Python packages
pip install psutil readchar rich
```

#### Verify Installation

```bash
# Verify zip utilities are installed
which zip && zip -v
```

```bash
# Verify Malmo package is available (should output: True)
python - <<'PY'
import pkgutil
print('malmo' in [m.name for m in pkgutil.iter_modules()])
PY
```

---

### Part 5: Authentication and Login

#### Set Up Gensyn Authentication

```bash
cd ~/blockassist/modal-login
```

```bash
yarn install
```

```bash
yarn dev
```

**In your browser:**
1. Navigate to `http://localhost:3000`
2. Log in with your Gensyn Testnet credentials
3. Return to terminal and press `Ctrl+C` after successful login

```bash
cd ~/blockassist
```

---

### Part 6: Launch BlockAssist

#### Start the Application

```bash
cd ~/blockassist
```

```bash
source blockassist-venv/bin/activate
```

```bash
python3 run.py
```

#### What Happens Next

1. You'll be prompted for your **Hugging Face API token** - paste it and press Enter
2. **First run takes 5-10 minutes** - BlockAssist downloads Minecraft and AI models
3. Two Minecraft windows will open (managed automatically by BlockAssist)
4. **DO NOT close the Minecraft windows manually** - BlockAssist manages them
5. Press Enter in the terminal when prompted
6. Click inside the Minecraft window and press Enter again
7. Start building! 🎮

> **Note:** Windows 11 WSL2 includes native GUI support, so Minecraft windows open automatically without additional configuration!

---

## 🔄 Restarting BlockAssist

When you come back to use BlockAssist again, copy and run this entire block:

```bash
cd blockassist

# Reinitialize pyenv (needed for new terminal sessions)
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"

# Activate virtual environment
source blockassist-venv/bin/activate

# Run BlockAssist
python3 run.py
```

---

## ⚡ Optional: GPU Acceleration

For users with NVIDIA GPUs who want better performance:

### Install CUDA/cuDNN Support

```bash
# Download cuDNN
wget https://developer.download.nvidia.com/compute/cudnn/9.11.0/local_installers/cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb

# Install package
sudo dpkg -i cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb

# Add GPG key
sudo cp /var/cudnn-local-repo-ubuntu2204-9.11.0/cudnn-local-4EC753EA-keyring.gpg /usr/share/keyrings/

# Configure repository
echo "deb [signed-by=/usr/share/keyrings/cudnn-local-4EC753EA-keyring.gpg] file:///var/cudnn-local-repo-ubuntu2204-9.11.0 /" | sudo tee /etc/apt/sources.list.d/cudnn-local.list

# Install cuDNN
sudo apt update
sudo apt install -y libcudnn9 libcudnn9-dev

# Set library path
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

### Install Poetry (Advanced Dependency Management)

If you prefer Poetry for Python package management:

```bash
curl -sSL https://install.python-poetry.org | python3 -
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
poetry --version
```

### Configure WSL Memory Limits (Advanced)

For users who want to optimize WSL performance:

1. Create `.wslconfig` file in your Windows user directory (`C:\Users\YourUsername\.wslconfig`)
2. Add the following configuration:

```ini
[wsl2]
memory=16GB
processors=8
swap=8GB
```

3. Restart WSL: `wsl --shutdown` (in Windows PowerShell)

Adjust values based on your system resources.

---

## 🛠️ Troubleshooting Common Issues

### Display Issues

#### GUI/Minecraft Window Not Opening

**Try this first:** Test if WSLg is working
```bash
sudo apt install -y x11-apps && xeyes
```

**If that doesn't work:** Set DISPLAY variable
```bash
export DISPLAY=:0
echo 'export DISPLAY=:0' >> ~/.bashrc
source ~/.bashrc
```

#### Black Screen or Graphics Glitches

**Try this first:** Enable software rendering
```bash
export LIBGL_ALWAYS_SOFTWARE=1
```

**Full solution:**
```bash
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```

---

### Environment Issues

#### "pyenv: command not found"

**Try this first:** Reload shell
```bash
source ~/.bashrc
```

**Full solution:**
```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc
```

#### WSL Not Starting

**Try this first:**
```bash
wsl --shutdown
```
Then restart Ubuntu from Start menu

**Full solution:**
```bash
wsl --update
wsl --set-default-version 2
wsl --shutdown
```

---

### Installation Issues

#### Permission Denied

**Try this first:**
```bash
chmod +x setup.sh
```

**Check permissions:**
```bash
ls -la
```

**For system packages:**
```bash
sudo apt install package-name
```

---

## ⚠️ Common Error Messages and Solutions

| Error Message | Cause | Solution |
|--------------|-------|----------|
| `ModuleNotFoundError: No module named 'malmo'` | BlockAssist not installed in editable mode | Run `pip install -e .` in blockassist directory with venv activated |
| `DISPLAY environment variable not set` | WSLg not configured properly | Run `export DISPLAY=:0` and add to ~/.bashrc |
| `yarn: command not found` | Yarn not installed or not in PATH | Run `corepack enable && corepack prepare yarn@stable --activate` |
| `pyenv: command not found` | pyenv not in PATH | Reload shell with `source ~/.bashrc` or reinitialize pyenv |
| `Permission denied: ./setup.sh` | Script not executable | Run `chmod +x setup.sh` |
| `Cannot connect to X server` | GUI support not working | Test with `xeyes`, set `DISPLAY=:0` if needed |
| `python: command not found` | Python not set as global | Run `pyenv global 3.10.0` |
| `Port 3000 already in use` | Previous login server still running | Kill process: `pkill -f "yarn dev"` |

---

## 🔄 Clean Reinstall

If you need to completely remove BlockAssist and start fresh:

```bash
# Stop any running processes
pkill -f run.py
pkill -f "yarn dev"

# Remove BlockAssist and virtual environment
rm -rf ~/blockassist
rm -rf ~/blockassist-venv

# Optional: Remove pyenv (only if you want to reinstall Python too)
rm -rf ~/.pyenv

# Restart from Part 4 of the installation guide
```

After clean reinstall, return to **Part 4: BlockAssist Installation** above.

---

## 📚 Helpful Resources

### Documentation
- [Official Gensyn Documentation](https://docs.gensyn.ai/) - Complete platform docs
- [BlockAssist GitHub Repository](https://github.com/gensyn-ai/blockassist) - Source code and issues
- [Microsoft WSL Documentation](https://docs.microsoft.com/en-us/windows/wsl/) - WSL setup and troubleshooting

### Tools & Platforms
- [Hugging Face](https://huggingface.co/) - AI model platform
- [Create Hugging Face Token](https://huggingface.co/settings/tokens) - Generate API token
- [pyenv Documentation](https://github.com/pyenv/pyenv) - Python version management
- [Node.js Official Site](https://nodejs.org/) - JavaScript runtime
- [Poetry Documentation](https://python-poetry.org/docs/) - Python dependency manager

### GPU & Performance
- [NVIDIA CUDA Downloads](https://developer.nvidia.com/cuda-toolkit) - GPU computing platform
- [cuDNN Library](https://developer.nvidia.com/cudnn) - Deep learning acceleration
- [WSL GPU Support](https://docs.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute) - GPU passthrough guide

---

## 💬 Getting Support

If you encounter issues:

1. **Check Documentation** - Review the [official Gensyn docs](https://docs.gensyn.ai/)
2. **Search Issues** - Look through [GitHub Issues](https://github.com/gensyn-ai/blockassist/issues)
3. **Join Discord** - Get help from the community at [Gensyn Discord](https://discord.gg/gensyn)
4. **Report Bugs** - Create a new issue on [GitHub](https://github.com/gensyn-ai/blockassist/issues/new)

---

## 💡 Tips & Best Practices

- **Fresh Install**: BlockAssist v0.1.0 works best with a clean installation
- **Disk Space**: Keep at least 5GB free for Minecraft and dependencies
- **Performance**: Close unnecessary apps while running BlockAssist
- **Updates**: Use `git pull` in the blockassist directory to get latest updates
- **Backups**: Keep your Hugging Face token secure and backed up

---

## ✨ Quick Command Reference

```bash
# Start BlockAssist
cd ~/blockassist && source blockassist-venv/bin/activate && python3 run.py

# Update BlockAssist
cd ~/blockassist && git pull && pip install -e .

# Check Python version
python --version

# Check Node version
node --version

# Check WSL version
wsl -l -v

# Check disk space
df -h

# View running processes
ps aux | grep python

# Kill stuck BlockAssist process
pkill -f run.py

# Kill stuck login server
pkill -f "yarn dev"

# Restart WSL (run in Windows PowerShell)
wsl --shutdown

# Test GUI support
xeyes

# Check Malmo installation
python -c "import pkgutil; print('malmo' in [m.name for m in pkgutil.iter_modules()])"
```

---

**Ready to start building with AI! 🤖✨**

Made with ❤️ for the Gensyn community
