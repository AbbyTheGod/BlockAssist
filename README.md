# 🎮 Gensyn BlockAssist Installation Guide for Windows 11 WSL

Complete step-by-step guide to install and run Gensyn BlockAssist v0.1.0 on Windows 11 using WSL2.

---

## ✅ Prerequisites

Before you begin, make sure you have:

* 🖥️ **Windows 11** with WSL 2 installed
* 🐧 **Ubuntu 22.04 LTS** (or your preferred Linux distro) in WSL
* 🐍 **Python 3.10+** 
* 🔧 **Git** installed
* 🧠 **Hugging Face account** with Write-access API token ([Get token here](https://huggingface.co/settings/tokens))
* 🟢 **Node.js and Yarn** for the login interface
* 💾 At least **12 GB RAM** (32 GB recommended)

---

## 🛠️ Installation Guide

### Step 1: Enable WSL2 and Install Ubuntu

Open **PowerShell** as Administrator and run:

```powershell
wsl --install
```

If Ubuntu 22.04 isn't the default, specify it:

```powershell
wsl --install -d Ubuntu-22.04
```

Restart your computer if prompted, then launch Ubuntu from the Start menu and create your username and password.

---

### Step 2: Clone BlockAssist Repository

Open your WSL terminal and run these commands **one by one**:

```bash
# Remove old installation if it exists
rm -rf ~/blockassist

# Clone the repository
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist
```

---

### Step 3: Install System Dependencies

```bash
# Update system packages
sudo apt update

# Install build tools and libraries
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev \
libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

---

### Step 4: Install Java

Java is required to run Minecraft:

```bash
sudo apt install -y openjdk-8-jdk
java -version
```

You should see output showing `openjdk version "1.8.x"`.

---

### Step 5: Install and Configure pyenv

```bash
# Install pyenv
curl -fsSL https://pyenv.run | bash

# Configure pyenv environment
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"

# Reload shell
source ~/.bashrc
```

---

### Step 6: Install Python 3.10

```bash
# Install Python 3.10 via pyenv
pyenv install 3.10.0
pyenv global 3.10.0

# Verify installation
python --version
```

---

### Step 7: Run Setup Script

```bash
chmod +x setup.sh
./setup.sh
```

---

### Step 8: Install Node.js and Yarn

```bash
# Install Node.js via nvm
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Load nvm and install Node.js LTS
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
nvm install --lts
nvm alias default 'lts/*'
nvm use default

# Enable Corepack and install Yarn
corepack enable
corepack prepare yarn@stable --activate

# Verify installations
node -v
npm -v
yarn -v
```

---

### Step 9: Install Poetry (Optional)

Poetry is a Python dependency manager. It's optional but recommended:

```bash
curl -sSL https://install.python-poetry.org | python3 -
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
poetry --version
```

---

### Step 10: Install GPU Support (Optional)

**Only for NVIDIA GPU users who want hardware acceleration:**

```bash
# Download and install cuDNN
wget https://developer.download.nvidia.com/compute/cudnn/9.11.0/local_installers/cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2204-9.11.0/cudnn-local-4EC753EA-keyring.gpg /usr/share/keyrings/

# Add repository
echo "deb [signed-by=/usr/share/keyrings/cudnn-local-4EC753EA-keyring.gpg] file:///var/cudnn-local-repo-ubuntu2204-9.11.0 /" | sudo tee /etc/apt/sources.list.d/cudnn-local.list

# Install cuDNN packages
sudo apt update
sudo apt install -y libcudnn9 libcudnn9-dev

# Configure CUDA libraries
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

---

### Step 11: Login to Gensyn

First, authenticate with your Gensyn Testnet account:

```bash
cd modal-login
yarn install
yarn dev
```

Open your browser to `http://localhost:3000` and log in.

After successful login, press `Ctrl+C` in the terminal to stop the server.

```bash
cd ~/blockassist
```

---

### Step 12: Create Virtual Environment and Install Dependencies

```bash
# Create and activate virtual environment
python3 -m venv blockassist-venv
source blockassist-venv/bin/activate

# Upgrade pip and install dependencies
python -m pip install --upgrade pip setuptools wheel

# Install BlockAssist in editable mode
pip install -e .

# Install additional required packages
pip install psutil readchar rich
```

---

### Step 13: Install Zip Utilities

```bash
sudo apt update
sudo apt install -y zip unzip

# Verify installation
which zip && zip -v
```

---

### Step 14: Verify Malmo Installation

Check if the Malmo package is properly installed:

```bash
python - <<'PY'
import pkgutil
print('malmo' in [m.name for m in pkgutil.iter_modules()])
PY
```

This should output `True`.

---

### Step 15: Run BlockAssist

```bash
python3 run.py
```

When prompted:
1. Enter your **Hugging Face API token**
2. Two Minecraft windows will open automatically
3. Press **Enter** in the terminal
4. Click on the game window, then press **Enter** again

**Note:** Windows 11 WSL2 has native GUI support, so Minecraft windows will open automatically without any additional setup!

---

## 🚀 How To Start BlockAssist the Next Day

Follow these steps whenever you restart BlockAssist:

```bash
cd blockassist

# Configure pyenv environment variables
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

Now run BlockAssist:

```bash
source blockassist-venv/bin/activate
python3 run.py
```

---

## 🔧 Troubleshooting

### Python/pyenv Issues

If you get `pyenv: command not found`:

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc
```

### GUI Not Working

Windows 11 WSL2 has native GUI support. If Minecraft doesn't open:

```bash
# Test GUI support
sudo apt install -y x11-apps
xeyes
```

If `xeyes` doesn't work, set the DISPLAY variable:

```bash
export DISPLAY=:0
echo 'export DISPLAY=:0' >> ~/.bashrc
source ~/.bashrc
```

### Black Screen or Graphics Issues

Enable software rendering:

```bash
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```

### WSL Issues

```bash
# Update WSL
wsl --update

# Restart WSL
wsl --shutdown
# Then restart Ubuntu
```

### Permission Errors

```bash
# Check file permissions
ls -la

# Fix permissions if needed
chmod +x setup.sh
```

---

## 📚 Resources

### Official Links
- [Gensyn Official Website](https://www.gensyn.ai/)
- [Gensyn Documentation](https://docs.gensyn.ai/)
- [BlockAssist GitHub](https://github.com/gensyn-ai/blockassist)

### Tools & Accounts
- [Hugging Face](https://huggingface.co/)
- [Get Hugging Face API Token](https://huggingface.co/settings/tokens)
- [WSL Documentation](https://docs.microsoft.com/en-us/windows/wsl/)

### Development Tools
- [Python Poetry](https://python-poetry.org/docs/)
- [Node.js](https://nodejs.org/)
- [pyenv](https://github.com/pyenv/pyenv)

### GPU Support
- [NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit)
- [cuDNN Downloads](https://developer.nvidia.com/cudnn)
- [WSL GPU Guide](https://docs.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute)

---

## 🤝 Community & Support

Need help? Here's where to go:

1. 📖 [Official Gensyn Documentation](https://docs.gensyn.ai/)
2. 💬 [Gensyn Discord Community](https://discord.gg/gensyn)
3. 🐛 [Report Issues on GitHub](https://github.com/gensyn-ai/blockassist/issues)

---

## 📝 Important Notes

- ✅ Windows 11 WSL2 provides **native GUI support** - no X server needed!
- ⏱️ First run may take longer as it downloads Minecraft and dependencies
- 💾 Ensure you have at least **5GB free disk space**
- 🔄 BlockAssist v0.1.0 requires a **fresh installation** - wipe and reclone if upgrading
- 🚀 For best performance, close unnecessary applications while running

---

**Enjoy building with Gensyn BlockAssist! 🎮✨**
