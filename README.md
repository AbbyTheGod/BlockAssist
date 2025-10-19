# Gensyn BlockAssist Installation Guide for Windows 11 WSL

Complete installation guide for running Gensyn BlockAssist v0.1.0 on Windows 11 using WSL2.

**Important:** If you're upgrading from a previous version, clean install is recommended. Remove old files and follow this guide from the beginning.

---

## 💻 System Requirements

### Minimum Requirements:
- **OS:** Windows 11 (64-bit)
- **RAM:** 12 GB minimum
- **Storage:** 10 GB free disk space
- **Processor:** 4 cores minimum
- **Internet:** Stable broadband connection

### Recommended:
- **RAM:** 32 GB or more
- **Storage:** 20 GB+ free disk space
- **Processor:** 8+ cores
- **GPU:** NVIDIA GPU with CUDA support (optional, for better performance)

---

## 🔧 Step 0: Install WSL2 on Windows 11

If you haven't installed WSL yet, follow these steps:

### Enable WSL

Open **PowerShell as Administrator** and run:

```powershell
wsl --install
```

This will install WSL2 with Ubuntu by default.

**If you want Ubuntu 22.04 specifically:**

```powershell
wsl --install -d Ubuntu-22.04
```

### Complete Setup

1. **Restart your computer** when prompted
2. **Open Ubuntu** from the Start menu
3. **Create a username and password** when asked (remember these!)
4. **Update Ubuntu:**

```bash
sudo apt update && sudo apt upgrade -y
```

Your WSL2 environment is now ready!

---

## 📋 What You'll Need

Before starting installation, make sure you have:

* ✅ **Windows 11** with WSL 2 installed (see above)
* ✅ **Ubuntu 22.04 LTS** running in WSL
* ✅ **Administrator access** to your Windows PC
* ✅ **Hugging Face account** with API token ([Create account](https://huggingface.co/join) | [Get token](https://huggingface.co/settings/tokens))
* ✅ **30-45 minutes** for complete installation

**Note:** Python, Git, Poetry, Node.js, and Yarn will be installed during the setup process below.

---

## 🚀 Installation Steps

### Part 1: Repository Setup and System Configuration

Launch your WSL terminal and execute these commands in sequence:

```bash
# Remove old installation (if exists)
rm -rf ~/blockassist
```

```bash
# Clone the repository and enter its folder
git clone https://github.com/gensyn-ai/blockassist.git
```

```bash
cd blockassist
```

```bash
# Install Core Dependencies
./setup.sh
```

```bash
# Install and configure pyenv
curl -fsSL https://pyenv.run | bash
```

```bash
# Configure pyenv environment variables
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

```bash
# Reload your shell environment
source ~/.bashrc
```

```bash
# Update system and install dependencies (enter 'y' if prompted)
sudo apt update
```

```bash
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

```bash
# Install Python 3.10 using pyenv and set it globally
pyenv install 3.10.0
```

```bash
pyenv global 3.10.0
```

```bash
# Install required Python packages
pip install psutil readchar
```

---

### Part 2: JavaScript Runtime Installation

Configure Node.js and Yarn for the web-based login interface:

```bash
# Install Node.js via nvm (Node Version Manager)
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

```bash
# Install latest LTS Node.js and set as default
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```

```bash
nvm install --lts
```

```bash
nvm alias default 'lts/*'
```

```bash
nvm use default
```

```bash
# Enable Corepack and install Yarn
corepack enable
```

```bash
corepack prepare yarn@stable --activate
```

Confirm successful installation:

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

### Part 3: GPU Acceleration (Optional - NVIDIA Users Only)

Skip this section if you don't have an NVIDIA graphics card. For GPU acceleration:

```bash
wget https://developer.download.nvidia.com/compute/cudnn/9.11.0/local_installers/cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
```

```bash
sudo dpkg -i cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
```

```bash
sudo cp /var/cudnn-local-repo-ubuntu2204-9.11.0/cudnn-local-4EC753EA-keyring.gpg /usr/share/keyrings/
```

```bash
echo "deb [signed-by=/usr/share/keyrings/cudnn-local-4EC753EA-keyring.gpg] file:///var/cudnn-local-repo-ubuntu2204-9.11.0 /" | sudo tee /etc/apt/sources.list.d/cudnn-local.list
```

```bash
sudo apt update
```

```bash
sudo apt install -y libcudnn9 libcudnn9-dev
```

```bash
# Add CUDA libraries to your path and reload
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
```

```bash
source ~/.bashrc
```

---

### Part 4: Poetry Setup (Dependency Manager)

Install Poetry for advanced Python package management:

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

```bash
source ~/.bashrc
```

```bash
poetry --version
```

---

### Part 5: Additional Python Libraries

Install remaining Python dependencies:

```bash
pip install psutil readchar rich
```

---

### Part 6: Authentication Setup

Configure your Gensyn account access:

**Step 1:** Navigate to login directory and install dependencies

```bash
cd modal-login
```

```bash
yarn install
```

```bash
yarn dev
```

**Step 2:** Access `http://localhost:3000` in your web browser and complete the Gensyn login process.

**Step 3:** After successful authentication, return to terminal and press `Ctrl+C`, then execute:

```bash
cd
```

```bash
cd blockassist
```

---

### Part 7: Final Configuration and Malmo Package Setup

Complete the installation by setting up the Python environment:

**Step 1:** Create and activate isolated environment

```bash
python3 -m venv blockassist-venv
```

```bash
source blockassist-venv/bin/activate
```

**Step 2:** Update core Python tools

```bash
python -m pip install --upgrade pip setuptools wheel
```

**Step 3:** Install BlockAssist in development mode (enables Malmo package import)

```bash
pip install -e .
```

**Step 4:** Verify Malmo availability

```bash
python - <<'PY'
import pkgutil
print('malmo' in [m.name for m in pkgutil.iter_modules()])
PY
```

**Step 5:** Install compression utilities

```bash
sudo apt update
```

```bash
sudo apt install -y zip unzip
```

**Step 6:** Launch the application

```bash
python3 run.py
```

---

## When prompted, provide your Hugging Face API token

## Minecraft will launch automatically

> **Windows 11 Advantage:** Native GUI support means Minecraft opens automatically without additional X server configuration.

**Usage Instructions:**
1. Enter your token when prompted
2. Wait for Minecraft windows to appear (managed automatically)
3. Press Enter in the terminal
4. Click the game window and press Enter again

---

## 🔄 Daily Restart Procedure

Use this streamlined process for subsequent sessions:

```bash
cd blockassist

# Configure pyenv environment variables
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

Start BlockAssist:

```bash
source blockassist-venv/bin/activate
```

```bash
python3 run.py
```

---

## 🔧 Common Issues and Solutions

### Display Problems

Windows 11 includes native WSL GUI support. If display issues occur:

```bash
# Verify GUI functionality
sudo apt install -y x11-apps
```

```bash
xeyes
```

If issues persist:

```bash
export DISPLAY=:0
```

### Rendering Issues

For graphics glitches, enable software rendering:

```bash
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```

---

## 📚 Helpful Links

- [Gensyn Official Documentation](https://docs.gensyn.ai/)
- [BlockAssist Source Repository](https://github.com/gensyn-ai/blockassist)
- [Hugging Face Platform](https://huggingface.co/)
- [Generate API Token](https://huggingface.co/settings/tokens)

---

**Ready to build with AI-powered Minecraft! 🎮✨**
