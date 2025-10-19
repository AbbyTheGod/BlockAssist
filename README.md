# Gensyn BlockAssist Installation Guide for Windows 11 WSL

This guide will walk you through installing and setting up Gensyn BlockAssist on Windows 11 using the Windows Subsystem for Linux (WSL).

## 📋 Prerequisites

Before you begin, ensure you have:

- **Windows 11** with WSL 2 enabled
- **Ubuntu 22.04 LTS** installed within WSL
- At least **12 GB RAM** (32 GB recommended)
- **Git** installed on both Windows and WSL
- A **Hugging Face** account with a Write-access API token
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

### Step 2: Install VcXsrv (X Server for Windows)

1. **Download and Install VcXsrv:**
   - Download VcXsrv from [SourceForge](https://sourceforge.net/projects/vcxsrv/)
   - Run the installer and follow the installation prompts

2. **Configure VcXsrv:**
   - Launch **XLaunch** from the Start menu
   - Select **Multiple windows** and click **Next**
   - Choose **Start no client** and click **Next**
   - On the **Extra settings** page:
     - ✅ Check **Disable access control**
     - ❌ Uncheck **Native OpenGL**
   - Click **Finish** to start the server

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

4. **Run the Setup Script:**
   ```bash
   ./setup.sh
   ```

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

7. **Install Required Python Packages:**
   ```bash
   pip install psutil readchar rich
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

### Step 4: Configure Display Settings

1. **Find Your Windows IP Address:**
   - Open **Command Prompt** on Windows and run:
     ```cmd
     ipconfig
     ```
   - Note the **IPv4 Address** (e.g., `192.168.1.10`)

2. **Set the DISPLAY Variable in WSL:**
   ```bash
   export DISPLAY=<WINDOWS_IP>:0
   ```
   Replace `<WINDOWS_IP>` with your actual Windows IP address

3. **Make DISPLAY Setting Permanent:**
   ```bash
   echo 'export DISPLAY=<WINDOWS_IP>:0' >> ~/.bashrc
   source ~/.bashrc
   ```

4. **Test X11 Forwarding:**
   ```bash
   sudo apt install -y x11-apps
   xeyes
   ```
   If the `xeyes` application appears, the X11 forwarding is working correctly

### Step 5: Run BlockAssist

1. **Navigate to the BlockAssist Directory:**
   ```bash
   cd ~/blockassist
   ```

2. **Activate the Virtual Environment:**
   ```bash
   source blockassist-venv/bin/activate
   ```

3. **Start BlockAssist:**
   ```bash
   python3 run.py
   ```
   - When prompted, enter your **Hugging Face** API token
   - A browser window will open for you to log in to your **Gensyn Testnet** account

4. **Launch Minecraft:**
   - The system will open two Minecraft windows. Do not close them manually; the application manages them
   - Follow the on-screen instructions to interact with BlockAssist

## 🔧 Troubleshooting

### Minecraft Window Not Opening
- Ensure the `DISPLAY` variable is set correctly
- Verify that VcXsrv is running without errors
- Check that your Windows firewall isn't blocking the connection

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

## 📚 Additional Resources

- [Gensyn BlockAssist Documentation](https://docs.gensyn.ai/testnet/blockassist/getting-started/windows-wsl-2)
- [WSL Official Documentation](https://docs.microsoft.com/en-us/windows/wsl/)
- [VcXsrv Documentation](https://sourceforge.net/projects/vcxsrv/)

## 🤝 Getting Help

If you encounter issues not covered in this guide:

1. Check the [official Gensyn documentation](https://docs.gensyn.ai/)
2. Visit the [Gensyn Discord community](https://discord.gg/gensyn)
3. Open an issue on the [BlockAssist GitHub repository](https://github.com/gensyn-ai/blockassist)

## 📝 Notes

- Keep VcXsrv running while using BlockAssist
- The first run may take longer as it downloads Minecraft and other dependencies
- Ensure you have sufficient disk space (at least 5GB free)
- For best performance, close unnecessary applications while running BlockAssist

---

**Happy building with Gensyn BlockAssist! 🎮✨**
