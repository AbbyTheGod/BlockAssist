# Gensyn BlockAssist Installation Guide for Windows 11 WSL

This guide will walk you through installing and setting up Gensyn BlockAssist on Windows 11 using the Windows Subsystem for Linux (WSL).

## 📋 Prerequisites

Before you begin, ensure you have:

- **Windows 11** with WSL 2 enabled
- **Ubuntu 22.04 LTS** installed within WSL
- At least **12 GB RAM** (32 GB recommended)
- **Git** installed on both Windows and WSL
- A **Hugging Face** account with a Write-access API token ([Create account](https://huggingface.co/join) | [Get API token](https://huggingface.co/settings/tokens))
- A **Gensyn Testnet** account (you'll be prompted to create one during setup if you don't have it)

## 🚀 Installation Steps

### Step 1: Enable WSL and Install Ubuntu 22.04 LTS

1. **Enable WSL:**
   - Open **PowerShell** as Administrator and run:
     ```powershell
     wsl --install
     ```
   - If Ubuntu 22.04 LTS isn't the default, specify it:
     ```powershell
     wsl --install -d Ubuntu-22.04
     ```
   - Restart your computer if prompted

2. **Set Up Ubuntu:**
   - Launch the **Ubuntu** app from the Start menu
   - Create a new UNIX username and password when prompted

### Step 2: Enable WSL GUI Support (Windows 11)

Windows 11 with WSL2 has built-in GUI support, so no additional X server is needed. The GUI applications will run natively through WSL.

### Step 3: Set Up BlockAssist in WSL

1. **Open Ubuntu Terminal:**
   - Launch the **Ubuntu** app

2. **Update and Install Dependencies:**
   ```bash
   sudo apt update
   sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev \
   libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
   libffi-dev liblzma-dev
   ```

3. **Clone the BlockAssist Repository:**
   ```bash
   git clone https://github.com/gensyn-ai/blockassist.git
   cd blockassist
   ```

4. **Install Java (Required for Minecraft):**
   ```bash
   sudo apt install -y openjdk-8-jdk
   java -version
   ```
   > **Note:** You should see "openjdk version 1.8.x" confirming Java 8 is installed correctly

5. **Install and Configure `pyenv`:**
   ```bash
   curl -fsSL https://pyenv.run | bash
   export PYENV_ROOT="$HOME/.pyenv"
   [[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
   eval "$(pyenv init --path)"
   eval "$(pyenv init -)"
   eval "$(pyenv virtualenv-init -)"
   source ~/.bashrc
   ```

6. **Install Python 3.10:**
   ```bash
   pyenv install 3.10.0
   pyenv global 3.10.0
   ```

7. **Run the Setup Script:**
   ```bash
   chmod +x setup.sh
   ./setup.sh
   ```

8. **Install Node Version Manager (NVM) and Node.js:**
   ```bash
   curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
   export NVM_DIR="$HOME/.nvm"
   [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
   nvm install --lts
   nvm alias default 'lts/*'
   nvm use default
   ```

9. **Enable Corepack and Install Yarn:**
   ```bash
   corepack enable
   corepack prepare yarn@stable --activate
   node -v
   npm -v
   yarn -v
   ```

10. **Install Poetry (Python Dependency Manager - Optional):**
    ```bash
    curl -sSL https://install.python-poetry.org | python3 -
    echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
    source ~/.bashrc
    poetry --version
    ```
    > **Note:** Poetry is optional and mainly for advanced dependency management. This guide uses pip/venv which works perfectly fine for BlockAssist.

### Step 4: Install CUDA/cuDNN for GPU Support (Optional)

**For users with NVIDIA GPUs who want hardware acceleration:**

1. **Download and Install cuDNN:**
   ```bash
   wget https://developer.download.nvidia.com/compute/cudnn/9.11.0/local_installers/cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
   sudo dpkg -i cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
   sudo cp /var/cudnn-local-repo-ubuntu2204-9.11.0/cudnn-local-4EC753EA-keyring.gpg /usr/share/keyrings/
   ```

2. **Add cuDNN Repository:**
   ```bash
   echo "deb [signed-by=/usr/share/keyrings/cudnn-local-4EC753EA-keyring.gpg] file:///var/cudnn-local-repo-ubuntu2204-9.11.0 /" | sudo tee /etc/apt/sources.list.d/cudnn-local.list
   ```

3. **Install cuDNN Packages:**
   ```bash
   sudo apt update
   sudo apt install -y libcudnn9 libcudnn9-dev
   ```

4. **Configure CUDA Libraries:**
   ```bash
   echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
   source ~/.bashrc
   ```

### Step 5: Configure Display Settings (Optional)

Windows 11 WSL2 handles GUI applications automatically, but you can optionally configure display settings:

1. **Test GUI Support:**
   ```bash
   sudo apt install -y x11-apps
   xeyes
   ```
   If the `xeyes` application appears, GUI support is working correctly

2. **If GUI doesn't work, set DISPLAY variable:**
   ```bash
   export DISPLAY=:0
   echo 'export DISPLAY=:0' >> ~/.bashrc
   source ~/.bashrc
   ```

### Step 6: Login to Gensyn Modal

1. **Navigate to Modal Login Directory:**
   ```bash
   cd ~/blockassist/modal-login
   ```

2. **Install Dependencies and Start Login Server:**
   ```bash
   yarn install
   yarn dev
   ```

3. **Login to Gensyn:**
   - Open your browser at `http://localhost:3000`
   - Log in to your Gensyn Testnet account
   - After login, press `Ctrl+C` to stop the server

### Step 7: Create Virtual Environment and Install Dependencies

1. **Navigate to the BlockAssist Directory:**
   ```bash
   cd ~/blockassist
   ```

2. **Create and Activate Virtual Environment:**
   ```bash
   python3 -m venv blockassist-venv
   source blockassist-venv/bin/activate
   ```

3. **Install BlockAssist Python Dependencies:**
   ```bash
   pip install --upgrade pip setuptools wheel
   pip install -e .
   ```

4. **Install Additional Required Packages:**
   ```bash
   pip install psutil readchar rich
   ```

5. **Install Zip Utilities:**
   ```bash
   sudo apt update
   sudo apt install -y zip unzip
   which zip && zip -v
   ```
   > **Verification:** The commands above should display the path to zip and its version information

6. **Verify Malmo Package Installation:**
   ```bash
   python - <<'PY'
   import pkgutil
   print('malmo' in [m.name for m in pkgutil.iter_modules()])
   PY
   ```
   This should output `True` if the installation was successful

### Step 8: Run BlockAssist

1. **Start BlockAssist:**
   ```bash
   python3 run.py
   ```
   - When prompted, enter your **Hugging Face** API token
   - The system will open two Minecraft windows. Do not close them manually; the application manages them
   - Follow the on-screen instructions to interact with BlockAssist

## 🔧 Troubleshooting

### Minecraft Window Not Opening
- Test GUI support with: `xeyes`
- If needed, set DISPLAY variable: `export DISPLAY=:0`
- Ensure WSL2 is running: `wsl --status`

### Black Screen or OpenGL Crash
Enable software rendering by setting the following environment variables:
```bash
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```

### "pyenv: command not found"
Reinitialize `pyenv` and reload the shell:
```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc
```

### WSL Not Starting
- Ensure WSL 2 is enabled: `wsl --set-default-version 2`
- Update WSL: `wsl --update`
- Restart WSL: `wsl --shutdown` then restart Ubuntu

### Permission Denied Errors
- Ensure you're using the correct user permissions
- Try running commands with `sudo` when necessary
- Check file permissions: `ls -la`

## 🚀 Daily Startup Process

When you want to restart BlockAssist after shutdown:

1. **Navigate to BlockAssist Directory:**
   ```bash
   cd ~/blockassist
   ```

2. **Activate Virtual Environment:**
   ```bash
   source blockassist-venv/bin/activate
   ```

3. **Run BlockAssist:**
   ```bash
   python3 run.py
   ```

## 📚 Additional Resources

### Official Documentation
- [Gensyn BlockAssist Documentation](https://docs.gensyn.ai/testnet/blockassist/getting-started/windows-wsl-2)
- [Gensyn Official Website](https://www.gensyn.ai/)
- [WSL Official Documentation](https://docs.microsoft.com/en-us/windows/wsl/)

### Tools & Platforms
- [Hugging Face Platform](https://huggingface.co/)
- [Hugging Face API Tokens](https://huggingface.co/settings/tokens)
- [Python Poetry Documentation](https://python-poetry.org/docs/)
- [Node.js Official Site](https://nodejs.org/)
- [pyenv GitHub Repository](https://github.com/pyenv/pyenv)

### GPU Support (Optional)
- [NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit)
- [cuDNN Downloads](https://developer.nvidia.com/cudnn)
- [WSL GPU Support Guide](https://docs.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute)

## 🤝 Getting Help

If you encounter issues not covered in this guide:

1. Check the [official Gensyn documentation](https://docs.gensyn.ai/)
2. Visit the [Gensyn Discord community](https://discord.gg/gensyn)
3. Open an issue on the [BlockAssist GitHub repository](https://github.com/gensyn-ai/blockassist)

## 📝 Notes

- Windows 11 WSL2 provides native GUI support - no additional X server needed
- The first run may take longer as it downloads Minecraft and other dependencies
- Ensure you have sufficient disk space (at least 5GB free)
- For best performance, close unnecessary applications while running BlockAssist
- **Important:** BlockAssist v0.1.0 requires a fresh installation - wipe and reclone if upgrading

---

**Happy building with Gensyn BlockAssist! 🎮✨**
