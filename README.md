# Gocilla theme for Oh My Posh

![Screenshot](https://raw.githubusercontent.com/goranvasic/gocilla-oh-my-posh/main/screenshot.png "Screenshot")

## Configure Windows Terminal to use Oh My Posh with Zsh and `zsh-autosuggestions`

Some people recommend using [ble.sh – Bash Line Editor](https://github.com/akinomyoga/ble.sh), a command line editor written in pure Bash which replaces the default GNU Readline (mentioned in [Bash vs ZSH vs Fish: What's the Difference?](https://www.youtube.com/watch?v=dRdGq8khTJc)). However, on Windows I still prefer using Zsh with `zsh-autosuggestions`.

## Configuration Instructions

- Download and install Git from [git-scm](https://git-scm.com/). Make sure to uncheck suggested options for Git Bash (e.g. the 2 options under "Windows Explorer integration"). You can select the option to add a Git Bash Profile to Windows Terminal, we will modify it manually later. When asked about adjusting your PATH environment, I like to use the Recommended setting, and I also like to enable symbolic links. I usually keep everything else on default.
- From Microsoft Store, install [Windows Terminal](https://www.microsoft.com/store/productId/9N0DX20HK701) and `winget` [App Installer](https://www.microsoft.com/store/productId/9NBLGGH4NNS1). Check if everything has been installed properly by opening a new Windows Terminal session and typing `winget --version`.
- Open your Windows Start menu, and search for "PowerShell".
- Run Windows PowerShell as Administrator, then install [Oh My Posh](https://ohmyposh.dev/docs/installation/windows) using `winget`, for example:
	```shell
	winget install JanDeDobbeleer.OhMyPosh -s winget
	```
- Close PowerShell, then add `oh-my-posh` path to your Windows environment variables (User):
	1. Press `Win + r`.
	2. Type in `control` and hit `ENTER`.
	3. Navigate to "User Accounts".
	4. Click on "User Accounts" one more time, then click the "Change my environment variables" link on the left side to open the "Environment Variables" window.
	5. In the upper (User) part of the window, scroll down and select `Path`, then click the `Edit...` button.
	6. Depending on the way your variables are displayed, either click `New` to add a new variable, or add a `;` at the end of the line, then type in `%LOCALAPPDATA%\Programs\oh-my-posh\bin` for the value. It might be that this value already exists, in which case you don't need to edit anything.
	7. Press `OK`, then confirm every previously opened window by clicking `OK`.
- Run PowerShell one more time with Administrator privileges, then install either the `FiraCode Nerd Font` or `SauceCodePro Nerd Font` (I prefer `SourceCodePro`) from [Nerd Fonts](https://www.nerdfonts.com/font-downloads) using the following command (follow their instructions in order to select the desired font):
    ```shell
    oh-my-posh font install
    ```
- Start a new Git Bash terminal by double-clicking on `git-bash.exe` located under `C:\Program Files\Git\`.
- Download the [Zsh for Windows package](https://packages.msys2.org/package/zsh?repo=msys&variant=x86_64) `zsh~x86_64.pkg.tar.zst` by executing the following command:
	```shell
	curl -fsLo $HOME/zsh-5.9-2-x86_64.pkg.tar.zst https://mirror.msys2.org/msys/x86_64/zsh-5.9-2-x86_64.pkg.tar.zst
	```
- In Windows Explorer, locate the downloaded `.zst` file in your home directory and extract it either using:
	- [PeaZip](https://peazip.github.io/), which can be installed using the `winget install -e peazip` command, or
	- [7-Zip](https://www.7-zip.org/download.html) (I prefer this option, since I already have it installed).
- You only need the `etc` and `usr` directories, so copy these two directories from the extracted folder into `C:\Program Files\Git` and when prompted, select the option to overwrite all existing files – don't worry, these two directories (`etc` and `usr`) contain completely new files, no existing files will be overwritten.
- Open `%PROGRAMFILES%/Git/etc/profile` in your text editor as Administrator (because you might need elevated privileges in order to save the changes) and:
    - Comment out the entire block near the bottom of the file that starts with line `if [ ! "x${BASH_VERSION}" = "x" ]; then` and ends with line `fi`. At the moment of writing the article, I commented out lines `111-133`. We don't need all of this code since we know we will be using Zsh.
    - Below the commented out block, add the following 3 lines:
        ```shell
        HOSTNAME="$(exec /usr/bin/hostname)"
        profile_d zsh
        SHELL='/usr/bin/zsh'
        ```
    - Save the changes.
- Open the Windows Start menu, search for "Terminal" and start it.
- Open Windows Terminal's settings using the `Ctrl + Shift + ,` keyboard shortcut, then under the `profiles > list` array modify the existing Bash profile, or add a new profile:
    ```json
    {
      "name": "Git Bash",
      "guid": "{12398ec4-4e3d-5e58-b989-0a998ec441b1}",
      "commandline": "%PROGRAMFILES%/git/usr/bin/zsh.exe -il",
      "icon": "%USERPROFILE%/git.ico",
      "startingDirectory": "%USERPROFILE%",
      "colorScheme": "VibrantInk",
      "font": 
      {
        "face": "SauceCodePro NFM"
      },
      "useAcrylic": true,
      "opacity": 70,
      "adjustIndistinguishableColors": "always",
      "bellStyle": "none",
      "hidden": false
    }
    ```
- HINT: For the code mentioned above, make sure to add the `,` character after the closing curly brace, if needed, so that the settings file syntax is valid. 
- The profile above uses a custom `~/git.ico` icon, so you can either reference the default `git-for-windows.ico` icon file under `C:\Program Files\Git\mingw64\share\git` or find another one you like.
-  If you wish, you can also set the above profile to be your default Windows Terminal profile by setting the same `guid` as the `defaultProfile` value in the settings JSON.
- Restart Windows Terminal and verify that everything is working (configure `zsh` using the interactive menu).
- Execute the following command from any location to install [Oh My Zsh](https://ohmyz.sh/) under your `$HOME/.oh-my-zsh` directory:
    ```shell
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```
- Clone `zsh-autosuggestions` using the following command:
    ```shell
    git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
    ```
- Open `~/.zshrc` file, comment out the line that sets `ZSH_THEME` on line  11 (we will be using Oh My Posh theme, not Oh My Zsh), then below on line 73, add the `zsh-autosuggestions` plugin to `oh-my-zsh` plugins:
    ```text
    plugins=(
      git
      zsh-autosuggestions
    )
    ```
- Save the changes and verify that autosuggestions are working (you need to have something in your history first, so type a couple of commands in Windows Terminal, exit with `logout`, then start the Terminal again). NOTE: On first run, `~/.zcompdump` file will be created, so it is normal for Terminal to start a bit slower.
- Grab the [Gocilla theme](https://github.com/goranvasic/gocilla-oh-my-posh/blob/main/gocilla.omp.json) for `Oh My Posh` and save the file under `oh-my-posh` themes:
    ```shell
    curl -fsLo $HOME/AppData/Local/Programs/oh-my-posh/themes/gocilla.omp.json https://raw.githubusercontent.com/goranvasic/gocilla-oh-my-posh/main/gocilla.omp.json
    ```
- Navigate to your Windows `%USERPROFILE%` directory, and open the `.zshrc` file one more time.
- Add the following line below the `source $ZSH/oh-my-zsh.sh` line:
    ```shell
    eval "$(oh-my-posh init zsh --config $HOME/AppData/Local/Programs/oh-my-posh/themes/gocilla.omp.json)"
    ```
- Restart Windows Terminal and verify that everything is working properly.

That's it. Hopefully this works on your side. You can run this customized Zsh in IntelliJ or VS Code by setting the built-in terminal to open `"C:\Program Files\Git\usr\bin\zsh.exe" -il`.

Any comments or suggestions for improvement will be much appreciated.
