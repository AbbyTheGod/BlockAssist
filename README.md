# Gensyn BlockAssist - Windows 11 Installation Guide

Get Gensyn BlockAssist running on Windows 11 in under 30 minutes.

---

## ⚡ Before You Start

- [ ] Windows 11 installed
- [ ] At least 10GB free space
- [ ] Create a [Hugging Face account](https://huggingface.co/join)
- [ ] Get your [Hugging Face API token](https://huggingface.co/settings/tokens)

---

## 🚀 Installation (Copy & Run Each Command)

### Step 1: Install WSL

Open **PowerShell as Administrator**:

```powershell
wsl --install -d Ubuntu-22.04
```

Restart your computer. Open Ubuntu from Start menu, create username and password.

---

### Step 2: Install System Packages

```bash
sudo apt update
```

```bash
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev \
libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev zip unzip openjdk-8-jdk
```

---

### Step 3: Install pyenv

```bash
curl -fsSL https://pyenv.run | bash
```

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

```bash
source ~/.bashrc
```

---

### Step 4: Install Python 3.10

```bash
pyenv install 3.10.0
```

```bash
pyenv global 3.10.0
```

---

### Step 5: Install Node.js

```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

```bash
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

---

### Step 6: Install Yarn

```bash
corepack enable
```

```bash
corepack prepare yarn@stable --activate
```

---

### Step 7: Get BlockAssist

```bash
git clone https://github.com/gensyn-ai/blockassist.git
```

```bash
cd blockassist
```

```bash
chmod +x setup.sh
```

```bash
./setup.sh
```

---

### Step 8: Set Up Python Environment

```bash
python3 -m venv blockassist-venv
```

```bash
source blockassist-venv/bin/activate
```

```bash
pip install --upgrade pip setuptools wheel
```

```bash
pip install -e .
```

```bash
pip install psutil readchar rich
```

---

### Step 9: Login to Gensyn

```bash
cd modal-login
```

```bash
yarn install
```

```bash
yarn dev
```

Open browser to `http://localhost:3000` and login.

After login, press `Ctrl+C` in terminal, then:

```bash
cd ~/blockassist
```

---

### Step 10: Run BlockAssist

```bash
source blockassist-venv/bin/activate
```

```bash
python3 run.py
```

When prompted, paste your **Hugging Face API token**.

First run takes 5-10 minutes. Two Minecraft windows will open - don't close them!

---

## 🔄 Running Again Later

Copy and run this entire block:

```bash
cd blockassist
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
source blockassist-venv/bin/activate
python3 run.py
```

---

## 🔧 Quick Fixes

**Minecraft won't open?**
```bash
export DISPLAY=:0
```

**"pyenv: command not found"?**
```bash
source ~/.bashrc
```

**"Module 'malmo' not found"?**
```bash
cd ~/blockassist
source blockassist-venv/bin/activate
pip install -e .
```

**Need to restart fresh?**
```bash
rm -rf ~/blockassist
# Then start again from Step 7
```

---

## 📚 Need Help?

- [Official Docs](https://docs.gensyn.ai/)
- [Discord Community](https://discord.gg/gensyn)
- [GitHub Issues](https://github.com/gensyn-ai/blockassist/issues)

---

**That's it! Start building with AI! 🎮✨**
