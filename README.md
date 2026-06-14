# What is this?
A collection of Linux programs, utilities, workarounds and other relevant notes. Mainly for myself for future reference, but hopefully other people will find this useful too.

# General notes
* I use Kubuntu (currently 26.04) and hate X11, so stuff might be different if you're on another distro, especially if it's not Debian-based and/or if you use X11 (ew)
* Avoid Snaps if possible, they are often outdated (sometimes months, a year or even more) and sometimes not tested properly (for example they rolled out a beta version of Slack to everyone which didn't work)
* I prefer flat as a packaging format because they work on every distro, so most links are flat. Some programs require extra tinkering for the flat version due to permission shenanigans and for some programs I haven't figured that out, so I don't use the flat version

# Kubuntu

## Kernel Panic on boot
* Happens to me after every kernel update. Wasn't always the case. Fix: https://askubuntu.com/questions/41930/kernel-panic-not-syncing-vfs-unable-to-mount-root-fs-on-unknown-block0-0
  * Enter boot menu and note the kernel version of the newest one
  * Boot with a previous kernel version
  * Run `sudo update-initramfs -u -k 7.0.0-22-generic` (put in the newest kernel version)
  * Run `sudo update-grub`
  * Reboot and you can now use the newer kernel version

## AppImages don't work
* You need to allow it in AppArmor, see https://askubuntu.com/questions/1512287/obsidian-appimage-the-suid-sandbox-helper-binary-was-found-but-is-not-configu/1528215#1528215

# Streaming/Recording

## [GPU Screen Recorder](https://flathub.org/en/apps/com.dec05eba.gpu_screen_recorder)
* Replay buffer, recording, can also stream but no scene composition. Replay buffer had no noticeable performance impact, can set it up to automatically start on boot

## [OBS](https://obsproject.com/download#linux)
* Flat version only supports specific plugins, not recommended for tinkerers/advanced users
* Ubuntu users can just add the official ppa, for other distros check for community builds
* Launch with `QT_QPA_PLATFORM=xcb`
  * Gives custom browser docks (which are not available on Wayland until https://github.com/chromiumembedded/cef/issues/2804 is done and implemented in OBS)
  * Allows input overlay to work
  * Allows global hotkeys without extra plugins/software

### Plugins
#### [Muted notification](https://obsproject.com/forum/resources/muted-notification.1706/)
* Adds an audio "filter" that plays a sound when there's output on the device but it is muted (basically a reminder to unmute)

#### [input-overlay](https://obsproject.com/forum/resources/input-overlay.552/)
* Shows the keys you are pressing
* Only works under X11/Xwayland, you need the xcb launch option (see above) and must allow X11 apps to read keystrokes (at least on KDE Plasma)

#### [PipeWire Audio Capture](https://obsproject.com/forum/resources/pipewire-audio-capture.1458/)
* Allows whitelist/blacklist audio capture (for example an audio source that captures everything except Discord, Steam and Firefox)
* Will probably be added to OBS someday, see https://github.com/obsproject/obs-studio/pull/6207 (currently waiting "due to lack of upstream consensus on the correct implementation details")

#### [obs-multi-rtmp](https://github.com/sorayuki/obs-multi-rtmp)
* For multi-streaming
* I couldn't do more than 2 platforms. When I did 3 (first one directly from OBS, second one with separate settings via multi-rtmp, third one using the same encode as the second also via multi-rtmp), I'd get lots of dropped frames, regardless of bitrate

#### [obs-vkcapture](https://github.com/nowrep/obs-vkcapture)
* Have not tested this much
* Allows Windows-style captures that don't need to be configured every time the game/program is restarted
* Building
  * I was missing SIMDe dependencies, `sudo apt install libsimde-dev`
  * Follow instructions from the readme
    * `mkdir build && cd build`
    * `cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release ..`
    * `make && make install` (I had to run `make && sudo make install`)
* Add the game capture source to your scene(s) and configure it if desired
* If I understand it correctly, it does not capture hardware cursors, so the cursor only shows up in games with software cursors (some games might have a setting to switch this). See https://github.com/nowrep/obs-vkcapture/issues/259

### Companion software

#### [Social Stream Ninja](https://github.com/steveseguin/social_stream)
* Unified chat from multiple platforms (Twitch, YouTube, Kick, TikTok, etc.)
* You can configure messages to fade after x seconds for your on stream chat and have a custom browser dock in OBS that doesn't. They don't have to share the same configuration

#### [Streamer.bot](https://github.com/Streamerbot/sb-linux-installer)
* Currently only using it for sound redemptions/commands, but can also do plenty of other useful things

#### [Firebot](https://firebot.app/)
* Only used it to have a hotkey to place a stream marker, but can also do other stuff

## [Kdenlive](https://kdenlive.org/)
* Video editor
* Don't use DaVinci Resolve
  * It's not officially supported on any distro except (I think) Rocky and RHEL. There are community-made workarounds but they tend to be somewhere from a little to a lot of hassle - if they even still work. Would probably recommend [davincibox](https://github.com/zelikos/davincibox) if you must have Resolve.
  * You don't get H264 & H265 support in the free version on Linux (you do on Windows), you'd have to buy a Studio license (~300$ one time purchase), see https://www.reddit.com/r/davinciresolve/comments/1fj02pg/davinci_resolve_19_works_on_linux_with_amd_gpu/

## [Constrict](https://flathub.org/en/apps/io.github.wartybix.Constrict)
* Compress videos to a target file size (for example to be under the 10 MB file limit for non-Nitro users on Discord)

# Gaming
## [ProtonUp-Qt](https://flathub.org/en/apps/net.davidotek.pupgui2)
* Download other Proton versions

## Wine
* Run Windows programs. For most games you want to use Proton (which is based on Wine), but some stuff only works with Wine directly. I only really use it for Cheat Engine and Diablo
* Might also want to install [Winetricks](https://github.com/Winetricks/winetricks) and protontricks for tinkering

## Cheat Engine and similar tools

### [Squalr](https://github.com/Squalr/Squalr)
* Tested super briefly, does work without any further setup

### Cheat Engine
* **I'd recommend to avoid it, read [here](https://www.reddit.com/r/linux_gaming/comments/1tzpfk9/comment/oqdwrbv/), especially the linked [GitHub issue](https://github.com/cheat-engine/cheat-engine/issues/3049)**
* If you really want to/must use Cheat Engine, it did work without major issues based on my experience

#### [protonhax](https://github.com/jcnils/protonhax) & [Cheat Engine](https://www.cheatengine.org/)
* Allows Cheat Engine to see the game's process so you can actually use it. Probably works with other similar software
* Download the Windows version of Cheat Engine, run the installer via Wine (`wine ./whateverthesetupiscalled.exe`)
* Set the game's launch parameters to `protonhax init %COMMAND%` in Steam, launch it
* Go to the Cheat Engine folder `/home/skytis/.wine/drive_c/Program Files/Cheat Engine/` and run `protonhax cmd <steamAppId>` (obviously replace the Id), then in that cmd run `start "" "Cheat Engine.exe"`. Do not omit the quotation marks

#### Linux version of Cheat Engine
* Very new (May 2026), currently only available as trial mode (not sure what the limitations are, if there are any, might be a WinRAR type of situation) unless you support on Patreon
* Gives a hint/warning to use the Windows version when selecting Proton games but does work without any further setup/launch options (tested ultra briefly, literally one value in one game)

## [Heroic Games Launcher](https://flathub.org/en/apps/com.heroicgameslauncher.hgl)
* For non-Steam stores (GOG etc.)
* I only tested one game (Pinball Spire) but no issue there

## [Lutris](https://flathub.org/en/apps/net.lutris.Lutris)
* For older and standalone games
* I only tested Diablo 3 (via Battle.net app), Diablo 2 LoD and Project Diablo 2

## Gamescope
* Only used it to limit the cursor to the game's window. Might need further parameters to prevent performance issues (see https://wiki.archlinux.org/title/Gamescope#Launching_gamescope_from_Steam,_stuttering_after_~24_minutes_(Gamescope_Lag_Bomb)). Here's what I used for an old version of Left 4 Dead 2 (for speedrunning) `env -u LD_PRELOAD gamescope -w 2560 -h 1440 --fullscren --force-grab-cursor -- env LD_PRELOAD="$LD_PRELOAD" %command% -steam -insecure -novid -high -lv -console`

## Path of Exile
* The game ran better for me in DX12 mode than Vulkan, see my [ProtonDB report](https://www.protondb.com/app/238960#JAWOsZVYaH)
* [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade)
  * Need to adjust the command-line argument to `--no-sandbox %U`; if using the KDE Plasma "start menu" you have to change the entry after every update because it reverts to `--sandbox %U`
* [Path of Building](https://flathub.org/en/apps/community.pathofbuilding.PathOfBuilding)

## Speedrunning
* Diablo https://www.speedrun.com/diablo/guides/muv82 (currently doesn't work with newer LiveSplit versions, must install an older one that doesn't require .NET 4.8.1 (4.8 is fine))
* Left 4 Dead 2 https://l4dsr.github.io/l4dsr-wiki/docs/tutorials/welcome/linux
* Neon White https://docs.google.com/document/d/12-BM0nusC79bhog5vBabLPoMeZBy2WoBKVKndpVXOJs (only for getting mods running, I couldn't get autosplitting to work)

# General utilities

## [Gear Lever](https://flathub.org/en/apps/it.mijorus.gearlever)
* Manage and update AppImages

## [Keyboard Center](https://github.com/zocker-160/keyboard-center)
* G Hub alternative made for Linux, allows macro mapping and such for Logitech keyboards
* Note to self: make sure to enable OpenRGB integration in the settings

## [OpenRGB](https://openrgb.org/)
* Manage RGB lighting. I have a profile to have my keyboard's RGB turned off on launch via Keyboard Center. Using non-flat version because I can't set up the udev rules with it

## [Solaar](https://github.com/pwr-Solaar/Solaar)
* Also for Logitech devices under Linux. I'm only using it to display my mouse's battery level

## [calibre](https://flathub.org/en/apps/com.calibre_ebook.calibre)
* Anything e-book related

## [Easy Effects](https://flathub.org/en/apps/com.github.wwmm.easyeffects)
* Audio filters and such. Only tested briefly because it seemed worse than Discord's and OBS' noise filters

## [Pipeweaver](https://github.com/pipeweaver/pipeweaver)
* (not tested yet) Should allow advanced audio routing, somewhat similar to VoiceMeeter on Windows. I plan to test it to separate my music into another track so my GPU SR replay buffer videos can easily have it removed

## [KDE Connect](https://kdeconnect.kde.org/)
* Connect your computer and phone (or other computers) to send files, share the clipboard or send remote inputs

# General programs
## [Waterfox](https://www.waterfox.com/download/)
* Firefox fork without the telemetry stuff
* Recommended extensions: Dark Reader (dark mode), DeArrow (replace clickbait thumbnails), KeePassXC-Browser, PocketTube (hide specific types of videos, like live or shorts, from your subscription page, also gets rid of the "Most relevant" shelf), Return YouTube Dislike (to help spot bad videos), SponsorBlock (skip sponsor segments aka ads inside of videos), uBlock Origin, uMatrix (only for advanced users, will break lots of websites, requires tinkering), YouTube Search Fixer
* The flat version does not work with the KeePassXC extension (without very hacky workarounds I couldn't figure out)
* For the standalone version to work with KeePassXC, this is probably right https://keepassxc.org/docs/KeePassXC_UserGuide#_custom_browser_option but I think I created `/home/skytis/.var/app/net.waterfox.waterfox/.waterfox/native-messaging-hosts/org.kde.plasma.browser_integration.json` but there might have been extra steps I'm forgetting. This is what I think I used https://www.reddit.com/r/KeePass/comments/1j3v14t/comment/mh5kg60/?solution=264dd9fd131ec066264dd9fd131ec066&js_challenge=1&token=7afd7253fec22262ff1c52b1703fe9ecb3bdbc210612db11733dc5c3b9f29c7f&jsc_orig_r=
```
{
    "allowed_extensions": [
        "plasma-browser-integration@kde.org"
    ],
    "description": "Native connector for KDE Plasma",
    "name": "org.kde.plasma.browser_integration",
    "path": "/home/skytis/.var/app/net.waterfox.waterfox/plasma-browser-integration-host",
    "type": "stdio"
}
```
* To give Waterfox an icon in the "task bar", edit the desktop file (/home/skytis/.local/share/applications/Waterfox.desktop). I placed a Waterfox logo svg in the Waterfox folder (/home/skytis/.local/bin/waterfox/)
 ```
[Desktop Entry]
Version=1.0
Type=Application
Name=Waterfox
Exec=/home/skytis/.local/bin/waterfox/waterfox %u
Icon=/home/skytis/.local/bin/waterfox/waterfox.svg
Terminal=false
Categories=Internet;
```
* To give Waterfox an icon when alt-tabbing, I created a window rule
  * Window class: exact match `waterfox`
  * Match whole window class: no
  * Window types: all selected
  * Desktop file name: apply initially `Waterfox`

## Discord
* Avoid the Snap version because it spams your journalctl every 5 seconds if you have apparmor (because Discord wants to read your processes but it is blocked). Workaround is either allowing it (if you want that) or using the flat version (note that you can't change the default keybinds in the flat version, you have to set up an additional hotkey, so for example you have the default mute hotkey ctrl+shift+M but also the custom one alt+numpad4 which you use)
* [Vencord](https://flathub.org/en/apps/dev.vencord.Vesktop) does work, but when you stream something it will have no audio OR it will route the audio through the microphone channel (which doesn't sound great AND means your actual microphone isn't being sent so you are effectively muted). Last tested around late 2025

## [FreeTube](https://flathub.org/en/apps/io.freetubeapp.FreeTube)
* Privacy focused YouTube app. I like to use it to watch videos that I don't want to give a "view" (usually ones with terrible titles)

## [Sunshine](https://flathub.org/en/apps/dev.lizardbyte.app.Sunshine) & [Moonlight](https://flathub.org/en/apps/com.moonlight_stream.Moonlight)
* Stream the desktop/game from one computer to another. Similar to TeamViewer (ew) or Steam's streaming. Sunshine is the host software, Moonlight the client that connects to it

## [KeePassXC](https://keepassxc.org/)
* Password manager. IIRC the flat version has issues with the Waterfox browser extension, so I use the snap version

## [Rclone](https://rclone.org/)
* To sync my kdbx file elsewhere
