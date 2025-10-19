# 🎮 Gensyn BlockAssist v0.1.0 on Windows 11 WSL

**BlockAssist v0.1.0 has been released with important fixes, including a HuggingFace update that all users need to apply.**

⚡️ **Action Required:** Please wipe & reclone BlockAssist, then follow the official guide below.

---

## ✅ Prerequisites

* 🖥️ **Windows 11** with WSL 2 installed
* 🐧 **Ubuntu 22.04 LTS** (or your preferred Linux distro) installed in WSL
* 🐍 **Python 3.10+** installed
* 🔧 **Git** installed
* 🧠 **Hugging Face account** with Write-access API token
* 📦 **Poetry** for Python dependency management
* 🟢 **Node.js and Yarn** for the login interface

---

## 🛠️ Installation Steps

### 🧱 Step 1: Clone and Setup BlockAssist in WSL

Open your WSL terminal and run these commands **one by one**:

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

### ⚡ Step 2: Install Node.js and Yarn

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

### 🧠 Step 3: (Optional) Install and Configure cuDNN for Nvidia GPU Support

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

### 📦 Step 4: Install Poetry (Python Dependency Manager)

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

### 🧰 Step 5: Install psutil, readchar, and rich

```bash
pip install psutil readchar rich
```

---

### 🔐 Step 6: Login to Gensyn Modal

**1)** First, go to the modal-login directory and start the login server:

```bash
cd modal-login
```

```bash
yarn install
```

```bash
yarn dev
```

**2)** Open your browser at `http://localhost:3000` and log in to Gensyn.

**After login enter `Ctrl+C` on your terminal and run:**

```bash
cd
```

```bash
cd blockassist
```

---

### 🧩 Step 7: Activate Environment & Install BlockAssist Locally (Malmo Fix) 🛠️

**1)** Activate the same venv

```bash
python3 -m venv blockassist-venv
```

```bash
source blockassist-venv/bin/activate
```

**2)** Make sure build tools are fine

```bash
python -m pip install --upgrade pip setuptools wheel
```

**3)** Install the repo in editable mode so Python can import its packages (including `malmo`)

```bash
pip install -e .
```

**Check if malmo is available**

```bash
python - <<'PY'
import pkgutil
print('malmo' in [m.name for m in pkgutil.iter_modules()])
PY
```

**4)** Install Zip Utilities

```bash
sudo apt update
```

```bash
sudo apt install -y zip unzip
```

**5)** Now run BlockAssist:

```bash
python3 run.py
```

---

## Now it will ask for Hugging Face token: `Paste your token`

## And soon your Minecraft game will open

> **Note:** Windows 11 WSL2 has native GUI support! Minecraft windows open automatically without needing VcXsrv or any X server.

`1. Press Enter in the terminal again.`

`2. Select your game screen, then press Enter again inside the screen.`

---

## 🚀 How To Start BlockAssist the Next Day

Follow these steps whenever you want to restart BlockAssist after shutdown:

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
```

```bash
python3 run.py
```

---

## 🔧 Troubleshooting

### GUI Not Working?

Windows 11 WSL2 handles GUI automatically. If Minecraft doesn't open:

```bash
# Test GUI support
sudo apt install -y x11-apps
```

```bash
xeyes
```

If that doesn't work:

```bash
export DISPLAY=:0
```

### Graphics Issues?

Enable software rendering:

```bash
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```

---

## 📚 Resources

- [Official Gensyn Documentation](https://docs.gensyn.ai/)
- [BlockAssist GitHub](https://github.com/gensyn-ai/blockassist)
- [Hugging Face](https://huggingface.co/)
- [Get HuggingFace Token](https://huggingface.co/settings/tokens)

---

**Happy building with Gensyn BlockAssist! 🎮✨**
