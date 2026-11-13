# Getting Started

## Software

### 1. Visual Studio Code

Download Visual Studio Code by clicking [here](https://code.visualstudio.com/). This will be our primary IDE while developing for the STM32 family of microprocessors.

Once Visual Studio Code has been downloaded, see [Visual Studio Code Extensions](#2-visual-studio-code-extensions).

### 2. Git 

You will need to download Git in order to access the source code. To access the source code, see [Accessing Source Code](#accessing-source-code).

#### On Windows:

To download Git on Windows, download Git Bash [here](https://git-scm.com/install/windows). Get the installer that applies to your device (likely x64 setup, but ARM64 if you have an ARM-based device). Run the installer and allow most default options, but don't download the additional tools (like chocolatey) if you can help it. They are not needed.

### 3. Visual Studio Code Extensions

Below is a list of extensions that will be very helpful. They cover IntelliSense, code formatting, etc.

1. [C/C++ Themes](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools-themes) - Makes C/C++ code much easier to read in VSCode
2. [C/C++ Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) - Adds IntelliSense features for C/C++ to VSCode.
3. [C/C++ DevTools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpp-devtools) - Provides DevTools for AI tools to utilize.
4. [CMake Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools) - Provides tools when making cmake-based projects.
5. [Clang-Format](https://marketplace.visualstudio.com/items?itemName=xaver.clang-format) - Allows clang-format to be used within VSCode.
6. [STM32CubeIDE](https://marketplace.visualstudio.com/items?itemName=stmicroelectronics.stm32-vscode-extension) - Adds STM32CubeIDE tools into VSCode.
7. [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) - Allows for debugging using the onboard STLink V2.

### 4. STM32CubeMX

Software to configure the STM32 family of microprocessors. Won't be used much after the initial stages of development, but required to make tweaks to configurations.

Click [here](https://www.st.com/en/development-tools/stm32cubemx.html#get-software) to donwload. You will have to **download as a guest**, enter your name and email, then download from the link in your email.

Once downloaded, extract the zip file and run the setup utility.

### 5. STM32CubeMX Required Firmware

Depending on the chip you will be developing for, you need to download the required firmware packages. Below are some ones that have been used previously:

- [STM32CubeH7](https://www.st.com/en/embedded-software/stm32cubeh7.html) - For the H7 family of microcontrollers (used in FCM).

### 6. (Optional) GitHub Desktop

I would recommend learning to use Git from the command line as it is much faster and more powerful, but if you would prefer to have a GUI to guide you (like Alec) then you can download GitHub Desktop from [here](https://github.com/apps/desktop).

## Accessing Source Code

You only need to set up an SSH key once. It lets GitHub confirm that it is you when you connect from your computer.

### 1. Create an SSH key

#### Windows

Open **Git Bash** and enter:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

#### Linux

Open **Terminal** and enter:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Replace `your-email@example.com` with the email address on your GitHub account. On either system, when asked where to save the key, press **Enter** to use the suggested location. You can set a passphrase for extra protection; if you do, you may be asked for it when using the key.

The command creates two files. The file ending in `.pub` is the public key you can share with GitHub. **Never share the file without `.pub`**; that is your private key.

### 2. Add the public key to GitHub

Use the instructions for your system to copy the public key:

#### Windows (Git Bash)

Run this command to copy the key to your clipboard:

```bash
clip < ~/.ssh/id_ed25519.pub
```

#### Linux (Terminal)

Print the key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Select and copy the entire line printed in the terminal.

#### Both systems

While signed in to GitHub, open **Settings -> SSH and GPG keys -> New SSH key**. Give the key a name that helps you recognize this computer, paste the public key, and save it.

Check that the connection works:

```sh
ssh -T git@github.com
```

If asked whether to trust GitHub's host key, type `yes`. GitHub should confirm that you authenticated successfully. Your GitHub account also needs permission to access this repository.

### 3. Download the project

Choose a location on your computer where you want the project folder to be created, open a terminal there, and run:

```sh
git clone git@github.com:AereMTU/HPR.git
```

This creates a folder named `HPR` containing the source code. To open it in Visual Studio Code, you can run:

```sh
code HPR
```

### 4. Set User Credentials

In order to push changes, you will need to set your name and email in Git so that changes can be traced to you. To do so, run these commands:

```sh
git config --global user.name "Full Name"
git config --global user.email "email@example.com"
```

Make sure to replace `Full Name` with your first and last name, as well as `email@example.com` with your email. Keep them in double quotes.  

### 5. Get the latest changes later

In the terminal, go into the project folder and pull the latest version:

```sh
cd HPR
git pull
```

Run `git pull` from inside the `HPR` folder whenever you want to update your copy.