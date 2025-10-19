# Gensyn BlockAssist Installation Guide for Windows 11 WSL

A comprehensive guide for setting up Gensyn BlockAssist on Windows 11 using Windows Subsystem for Linux (WSL2).

## 📋 System Requirements

Before starting, ensure your system meets these requirements:

- **Operating System:** Windows 11 with WSL2 enabled
- **Linux Distribution:** Ubuntu 22.04 LTS (recommended)
- **Memory:** Minimum 12 GB RAM (32 GB recommended for optimal performance)
- **Storage:** At least 5 GB free disk space
- **Accounts Required:**
  - Hugging Face account with Write-access API token
  - Gensyn Testnet account (created during setup if needed)

## 🚀 Complete Installation Guide

### Phase 1: WSL Environment Setup

#### 1.1 Enable WSL2 and Install Ubuntu

**Install WSL2:**
```powershell
# Run in PowerShell as Administrator
wsl --install
```

**Specify Ubuntu 22.04 if needed:**
```powershell
wsl --install -d Ubuntu-22.04
```

**Initial Ubuntu Setup:**
- Launch Ubuntu from Start menu
- Create your UNIX username and password
- Restart computer if prompted

#### 1.2 System Dependencies Installation

**Update package lists and install essential tools:**
```bash
sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev \
libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

**Install Java (required for Minecraft):**
```bash
sudo apt install -y openjdk-8-jdk
java -version  # Verify installation
```

### Phase 2: Python Environment Configuration

#### 2.1 Install Python Version Manager

**Install pyenv:**
```bash
curl -fsSL https://pyenv.run | bash
```

**Configure pyenv environment:**
```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc
```

**Install Python 3.10:**
```bash
pyenv install 3.10.0
pyenv global 3.10.0
```

#### 2.2 Create Virtual Environment

**Set up isolated Python environment:**
```bash
python3 -m venv blockassist-venv
source blockassist-venv/bin/activate
```

**Upgrade pip and install core packages:**
```bash
pip install --upgrade pip setuptools wheel
pip install psutil readchar rich
```

### Phase 3: BlockAssist Installation

#### 3.1 Clone Repository

**Get the latest BlockAssist code:**
```bash
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist
```

**Make setup script executable and run:**
```bash
chmod +x setup.sh
./setup.sh
```

#### 3.2 Install BlockAssist Dependencies

**Install in development mode:**
```bash
pip install -e .
```

**Install additional utilities:**
```bash
sudo apt install -y zip unzip
```

### Phase 4: Node.js and Frontend Setup

#### 4.1 Install Node.js via NVM

**Install Node Version Manager:**
```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

**Configure and install Node.js:**
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
nvm install --lts
nvm alias default 'lts/*'
nvm use default
```

#### 4.2 Install Yarn Package Manager

**Enable Corepack and install Yarn:**
```bash
corepack enable
corepack prepare yarn@stable --activate
```

**Verify installations:**
```bash
node --version
npm --version
yarn --version
```

### Phase 5: GUI Configuration (Windows 11)

Windows 11 WSL2 provides native GUI support, but you can verify functionality:

**Test GUI support:**
```bash
sudo apt install -y x11-apps
xeyes  # Should open a small window with animated eyes
```

**If GUI doesn't work, set display variable:**
```bash
export DISPLAY=:0
echo 'export DISPLAY=:0' >> ~/.bashrc
source ~/.bashrc
```

### Phase 6: Authentication Setup

#### 6.1 Gensyn Modal Login

**Navigate to login directory:**
```bash
cd ~/blockassist/modal-login
```

**Install dependencies and start server:**
```bash
yarn install
yarn dev
```

**Complete authentication:**
- Open browser to `http://localhost:3000`
- Log in to your Gensyn Testnet account
- Press `Ctrl+C` to stop the server after login

### Phase 7: Launch BlockAssist

#### 7.1 Final Setup

**Return to main directory:**
```bash
cd ~/blockassist
```

**Activate virtual environment:**
```bash
source blockassist-venv/bin/activate
```

#### 7.2 Start the Application

**Launch BlockAssist:**
```bash
python3 run.py
```

**Complete the setup:**
- Enter your Hugging Face API token when prompted
- Two Minecraft windows will open automatically
- Follow on-screen instructions for interaction

## 🔧 Troubleshooting Guide

### Common Issues and Solutions

#### GUI Applications Not Displaying
```bash
# Test X11 forwarding
xeyes

# If needed, set display variable
export DISPLAY=:0
```

#### OpenGL Rendering Issues
```bash
# Enable software rendering
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```

#### Python Environment Issues
```bash
# Reinitialize pyenv
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc
```

#### WSL Startup Problems
```bash
# Check WSL status
wsl --status

# Update WSL if needed
wsl --update

# Restart WSL
wsl --shutdown
# Then restart Ubuntu
```

## 🚀 Daily Usage

### Starting BlockAssist After Shutdown

**Quick start process:**
```bash
cd ~/blockassist
source blockassist-venv/bin/activate
python3 run.py
```

### Updating BlockAssist

**For version updates:**
```bash
cd ~/blockassist
git pull origin main
pip install -e .
```

## 📚 Additional Resources

- [Official Gensyn Documentation](https://docs.gensyn.ai/)
- [WSL Documentation](https://docs.microsoft.com/en-us/windows/wsl/)
- [BlockAssist GitHub Repository](https://github.com/gensyn-ai/blockassist)

## 🤝 Support

If you encounter issues:

1. Check the [official Gensyn documentation](https://docs.gensyn.ai/)
2. Join the [Gensyn Discord community](https://discord.gg/gensyn)
3. Report issues on the [BlockAssist GitHub repository](https://github.com/gensyn-ai/blockassist)

## 📝 Important Notes

- **Version Compatibility:** BlockAssist v0.1.0 requires fresh installation
- **System Resources:** Close unnecessary applications for optimal performance
- **Storage Requirements:** Ensure adequate disk space for Minecraft and dependencies
- **GUI Support:** Windows 11 WSL2 provides native GUI support without additional X servers

---

**Ready to build with Gensyn BlockAssist! 🎮✨**
