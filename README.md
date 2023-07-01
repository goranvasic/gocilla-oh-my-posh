# Gocilla theme for Oh My Posh

![Screenshot](https://raw.githubusercontent.com/goranvasic/gocilla-oh-my-posh/main/screenshot.png "Screenshot")

## Installation

1. Install [Oh My Posh](https://ohmyposh.dev/) and add `%LOCALAPPDATA%\Programs\oh-my-posh\bin` to your `PATH` environment variable.
2. Run Windows Terminal as administrator, then install the `FiraCode` Nerd Font using the following command:
	```shell
	oh-my-posh font install
	```
3. Configure Windows Terminal to use previously installed `FiraCode Nerd Font` and set a custom theme for Git Bash, for example:
    ```json
    {
      "colorScheme": "VibrantInk",
      "commandline": "%PROGRAMFILES%/git/usr/bin/bash.exe -il",
      "font": 
      {
        "face": "FiraCode Nerd Font"
      },
      "guid": "{12398ec4-4e3d-5e58-b989-0a998ec441b1}",
      "hidden": false,
      "icon": "%PROGRAMFILES%/Git/mingw64/share/git/git-for-windows.ico",
      "name": "Git Bash",
      "opacity": 70,
      "startingDirectory": "%USERPROFILE%",
      "useAcrylic": true,
      "bellStyle": "none"
	}
    ```
4. Copy [gocilla.omp.json](https://raw.githubusercontent.com/goranvasic/gocilla-oh-my-posh/main/gocilla.omp.json) from this repo to `$LOCALAPPDATA/Programs/oh-my-posh/themes/gocilla.omp.json` and configure the following line in your `~/.bash_profile` file:
    ```shell
    # Run "Oh My Posh"
    eval "$(oh-my-posh init bash --config $LOCALAPPDATA/Programs/oh-my-posh/themes/gocilla.omp.json)"
    ```
