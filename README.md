# brew-autoupdate

A script and launchd agent for automatically installing Homebrew updates, on
macOS.

This tool is built for my preferences, but maybe it's flexible enough to be useful
for other people too.

## What it does

* Provides wrapper script for doing a full homebrew upgrade
  1. Updates Homebrew
  2. Checks for outdated formulae
  3. Runs the upgrade
  4. Cleans up afterward
* Provides launchd agent and installer to let launchd run upgrades
  automatically
* Allows for "blocker formulae" that stop autoupgrades, in case you have
  formulae you want to manage on your own timeline
* Runs upgrade in Terminal.app so you can see all output
* Shows every `brew` command that's run in the script, to demystify things
* Keeps your computer awake while running upgrade

## Install

1. Clone this repo somewhere on your machine
   ```
   git clone git@github.com:rosston/brew-autoupdate.git
   cd brew-autoupdate
   ```
2. Install the launchd agent
   ```
   ./brew-autoupdate install
   ```
3. If you want to stop autoupdate from running when certain formula are
   available for upgrade (e.g., you often detach from tmux sessions with vim
   running in them and don't want tmux or vim updated out from underneath you),
   you can add a "blocker formulae" list
   ```
   mkdir -p ~/.config/brew-autoupdate
   touch ~/.config/brew-autoupdate/blocker-formulae
   # Then put whatever formulae names you want in this file,
   # one formula per line.
   ```
4. If you want to run the full update process manually, do
   ```
   ./brew-autoupdate run
   ```
5. If you don't like when the autoupdate happens, you can edit the installed
   plist file at
   `~/Library/LaunchAgents/com.thinkingsauce.brew_autoupdate.plist` and change
   the interval settings (HINT: run `man launchd.plist` and search for
   `StartCalendarInterval` to see  your options)

NOTE: I think you need to have your screen on for the first time the launchd agent runs so that you can approve a prompt to let "sh control Terminal". That's because the plist [uses some AppleScript to run the autoupdate script in Terminal.app](https://github.com/rosston/brew-autoupdate/blob/main/run-in-terminal).

## Uninstall

1. Run the uninstall script
   ```
   # Wherever your brew-autoupdate repo is cloned
   ./brew-autoupdate uninstall
   ```
2. Delete the repo
3. If you want to clean up _everything_, remove the blocker-formulae file
   (which may or may not exist, depending on how you set things up)
   ```
   rm -f ~/.config/brew-autoupdate/blocker-formulae
   ```
