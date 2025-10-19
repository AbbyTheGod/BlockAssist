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
sudo apt update && sudo apt upgrade -y
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev \
libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev zip unzip openjdk-8-jdk
```

Verify Java installation:
```bash
java -version
```

---

### Part 2: Python Environment Setup

#### Install pyenv for Python Management

```bash
curl -fsSL https://pyenv.run | bash

# Add pyenv to your shell
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"

# Apply changes
source ~/.bashrc
```

#### Install Python 3.10

```bash
pyenv install 3.10.0
pyenv global 3.10.0
python --version  # Should show Python 3.10.0
```

---

### Part 3: Node.js and JavaScript Tools

#### Install Node Version Manager (nvm)

```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Load nvm
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```

#### Install Node.js and Yarn

```bash
nvm install --lts
nvm use default

# Install Yarn via Corepack
corepack enable
corepack prepare yarn@stable --activate

# Verify
node -v && npm -v && yarn -v
```

---

### Part 4: BlockAssist Installation

#### Get BlockAssist Source Code

```bash
# Clean install (remove old version if exists)
rm -rf ~/blockassist

# Clone repository
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist
```

#### Run Initial Setup

```bash
chmod +x setup.sh
./setup.sh
```

#### Set Up Python Virtual Environment

```bash
# Create isolated environment
python3 -m venv blockassist-venv

# Activate environment
source blockassist-venv/bin/activate

# Upgrade pip and tools
python -m pip install --upgrade pip setuptools wheel

# Install BlockAssist
pip install -e .

# Install additional dependencies
pip install psutil readchar rich
```

#### Verify Installation

```bash
# Check zip utilities
which zip && zip -v

# Verify Malmo package
python - <<'PY'
import pkgutil
print('✓ Malmo installed' if 'malmo' in [m.name for m in pkgutil.iter_modules()] else '✗ Malmo missing')
PY
```

---

### Part 5: Authentication and Login

#### Set Up Gensyn Authentication

```bash
cd ~/blockassist/modal-login
yarn install
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
# Make sure you're in blockassist directory
cd ~/blockassist

# Activate virtual environment
source blockassist-venv/bin/activate

# Run BlockAssist
python3 run.py
```

#### What Happens Next

1. You'll be prompted for your **Hugging Face API token** - paste it and press Enter
2. Two Minecraft windows will open (managed automatically by BlockAssist)
3. Press Enter in the terminal when prompted
4. Click inside the Minecraft window and press Enter again
5. Start building! 🎮

> **Note:** Windows 11 WSL2 includes native GUI support, so Minecraft windows open automatically without additional configuration!

---

## 🔄 Restarting BlockAssist

When you come back to use BlockAssist again:

```bash
cd blockassist

# Reinitialize pyenv (if in a new terminal)
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"

# Activate virtual environment
source blockassist-venv/bin/activate

# Run
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

---

## 🛠️ Troubleshooting Common Issues

### Issue: GUI/Minecraft Window Not Opening

**Solution 1:** Test GUI support
```bash
sudo apt install -y x11-apps
xeyes
```

**Solution 2:** Set DISPLAY variable
```bash
export DISPLAY=:0
echo 'export DISPLAY=:0' >> ~/.bashrc
```

### Issue: Black Screen or Graphics Glitches

Enable software rendering:
```bash
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```

### Issue: "pyenv: command not found"

Reinitialize pyenv:
```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc
```

### Issue: WSL Not Starting

```bash
# Update WSL
wsl --update

# Restart WSL
wsl --shutdown
# Open Ubuntu again from Start menu
```

### Issue: Permission Denied

```bash
# Check permissions
ls -la

# Make scripts executable
chmod +x setup.sh

# Use sudo for system operations
sudo apt install package-name
```

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

# Restart WSL
wsl --shutdown

# Test GUI
xeyes
```

---

**Ready to start building with AI! 🤖✨**

Made with ❤️ for the Gensyn community
