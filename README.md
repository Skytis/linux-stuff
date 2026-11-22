# What is this?
A collection of Linux programs, utilities, workarounds and other relevant notes. Mainly for myself for future reference, but hopefully other people will find this useful too.

# General notes
* I use CachyOS (since October 2026), so newer notes will be for that
* I used Kubuntu (26.04) previously and hate X11, so stuff might be different if you're on another distro, especially if it's not Arch-based (for the newer entries) or Debian-based and/or if you use X11 (ew)
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

## [Constrict](https://flathub.org/en/apps/io.github.wartybix.Constrict)
* Compress videos to a target file size (for example to be under the 20 MB file limit for non-Nitro users on Discord)

## [GPU Screen Recorder](https://flathub.org/en/apps/com.dec05eba.gpu_screen_recorder)
* Replay buffer, recording, can also stream but no scene composition. Replay buffer had no noticeable performance impact, can set it up to automatically start on boot
* Consider changing the video frame rate to constant if you want to edit them with Kdenlive, otherwise you will have to transcode every clip before you can edit it

## [Kdenlive](https://kdenlive.org/)
* Video editor
* Don't use DaVinci Resolve
  * It's not officially supported on any distro except (I think) Rocky and RHEL. There are community-made workarounds but they tend to be somewhere from a little to a lot of hassle - if they even still work. Would probably recommend [davincibox](https://github.com/zelikos/davincibox) if you must have Resolve.
  * You don't get H264 & H265 support in the free version on Linux (you do on Windows), you'd have to buy a Studio license (~300$ one time purchase), see https://www.reddit.com/r/davinciresolve/comments/1fj02pg/davinci_resolve_19_works_on_linux_with_amd_gpu/

## [OBS](https://obsproject.com/download#linux)
* Flat version only supports specific plugins, not recommended for tinkerers/advanced users
* Ubuntu users can just add the official ppa, for other distros check for community builds
* Kubuntu: Launch with `QT_QPA_PLATFORM=xcb` (launches OBS with Xwayland)
  * Gives custom browser docks (which are not available on Wayland until https://github.com/chromiumembedded/cef/issues/2804 is done and implemented in OBS)
  * Allows input overlay (the plugin, not the separate program) to work
  * Allows global hotkeys without extra plugins/software
* The Xwayland workaround doesn't work on CachyOS for me, so I use the Flat version. Global hotkeys do work here as well now, the only limitation is you can't put the same action on one hotkey (i.e. same key for hide and show preview to have a toggle button)

### Plugins

#### [input-overlay](https://obsproject.com/forum/resources/input-overlay.552/) (OBS Plugin)
* Not recommended. Use girlglock's input overlay (see Companion Software)
* Shows the keys you are pressing
* Only works under X11/Xwayland, you need the xcb launch option (see above) and must allow X11 apps to read keystrokes (at least on KDE Plasma)

#### [Muted notification](https://obsproject.com/forum/resources/muted-notification.1706/)
* Adds an audio "filter" that plays a sound when there's output on the device but it is muted (basically a reminder to unmute)

#### [obs-multi-rtmp](https://github.com/sorayuki/obs-multi-rtmp)
* For multi-streaming
* I couldn't do more than 2 platforms. When I did 3 (first one directly from OBS, second one with separate settings via multi-rtmp, third one using the same encode as the second also via multi-rtmp), I'd get lots of dropped frames, regardless of bitrate

#### [obs-vkcapture](https://github.com/nowrep/obs-vkcapture)
* Have not tested this much
* Allows Windows-style captures that don't need to be configured every time the game/program is restarted
* Building (on Kubuntu)
  * I was missing SIMDe dependencies, `sudo apt install libsimde-dev`
  * Follow instructions from the readme
    * `mkdir build && cd build`
    * `cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release ..`
    * `make && make install` (I had to run `make && sudo make install`)
* Add the game capture source to your scene(s) and configure it if desired
* If I understand it correctly, it does not capture hardware cursors, so the cursor only shows up in games with software cursors (some games might have a setting to switch this). See https://github.com/nowrep/obs-vkcapture/issues/259

#### [PipeWire Audio Capture](https://obsproject.com/forum/resources/pipewire-audio-capture.1458/)
* Allows whitelist/blacklist audio capture (for example an audio source that captures everything except Discord, Steam and Firefox)
* Will probably be added to OBS someday, see https://github.com/obsproject/obs-studio/pull/6207 (currently waiting "due to lack of upstream consensus on the correct implementation details")

### Companion Software

#### [Firebot](https://firebot.app/)
* Only used it to have a hotkey to place a stream marker, but can also do other stuff
 
#### [input-overlay](https://github.com/girlglock/input-overlay) (Separate Program)
* Shows the keys you are pressing
* Does not require special workarounds

#### [Social Stream Ninja](https://github.com/steveseguin/social_stream)
* Unified chat from multiple platforms (Twitch, YouTube, Kick, TikTok, etc.)
* You can configure messages to fade after x seconds for your on stream chat and have a custom browser dock in OBS that doesn't. They don't have to share the same configuration

#### [Streamer.bot](https://github.com/Streamerbot/sb-linux-installer)
* Currently only using it for sound redemptions/commands, but can also do plenty of other useful things
* Playing sound files doesn't work properly on CachyOS for me, might investigate at some point

# Gaming

## Cheat Engine and similar tools

### Cheat Engine
* **I'd recommend to avoid it, read [here](https://www.reddit.com/r/linux_gaming/comments/1tzpfk9/comment/oqdwrbv/), especially the linked [GitHub issue](https://github.com/cheat-engine/cheat-engine/issues/3049)**
* If you really want to/must use Cheat Engine, it did work without major issues based on my experience

#### [protonhax](https://github.com/jcnils/protonhax) & [Cheat Engine](https://www.cheatengine.org/)
* Allows Cheat Engine to see the game's process so you can actually use it. Probably works with other similar software
* Download the Windows version of Cheat Engine, run the installer via Wine (`wine ./whateverthesetupiscalled.exe`)
* Set the game's launch parameters to `protonhax init %COMMAND%` in Steam, launch it
* Go to the Cheat Engine folder `/home/skytis/.wine/drive_c/Program Files/Cheat Engine/` and run `protonhax cmd <steamAppId>` (obviously replace the Id), then in that cmd run `start "" "Cheat Engine.exe"`. Do not omit the quotation marks

### [Squalr](https://github.com/Squalr/Squalr)
* Tested super briefly, does work without any further setup

#### Linux version of Cheat Engine
* Very new (May 2026), currently only available as trial mode (not sure what the limitations are, if there are any, might be a WinRAR type of situation) unless you support on Patreon
* Gives a hint/warning to use the Windows version when selecting Proton games but does work without any further setup/launch options (tested ultra briefly, literally one value in one game)

## Gamescope
* Only used it to limit the cursor to the game's window. Might need further parameters to prevent performance issues (see https://wiki.archlinux.org/title/Gamescope#Launching_gamescope_from_Steam,_stuttering_after_~24_minutes_(Gamescope_Lag_Bomb)). Here's what I used for an old version of Left 4 Dead 2 (for speedrunning) `env -u LD_PRELOAD gamescope -w 2560 -h 1440 --fullscren --force-grab-cursor -- env LD_PRELOAD="$LD_PRELOAD" %command% -steam -insecure -novid -high -lv -console`

## [Heroic Games Launcher](https://flathub.org/en/apps/com.heroicgameslauncher.hgl)
* For non-Steam stores (GOG etc.)
* I only tested one game (Pinball Spire) but no issue there

## [Lutris](https://flathub.org/en/apps/net.lutris.Lutris)
* For older and standalone games
* I only tested Diablo 3 (via Battle.net app), Diablo 2 LoD and Project Diablo 2

## Path of Exile
* The game ran better for me in DX12 mode than Vulkan, see my [ProtonDB report](https://www.protondb.com/app/238960#JAWOsZVYaH)
* [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade)
  * Need to adjust the command-line argument to `--no-sandbox %U`; if using the KDE Plasma "start menu" you have to change the entry after every update because it reverts to `--sandbox %U`
  * Currently only works for me if PoE is launched via Proton 10
  * If it doesn't work for you, check the github issues. Search for Linux and see what other workarounds people share. Check the date, so you don't try a 3 year old fix that is obsolete by now anyway
* [Path of Building](https://flathub.org/en/apps/community.pathofbuilding.PathOfBuilding)
* [clickr](https://github.com/Wavefire5201/clickr) for spam clicking (for example to move stuff to stash)

## [ProtonUp-Qt](https://flathub.org/en/apps/net.davidotek.pupgui2)
* Download other Proton flavors and versions (i.e. non-vanilla, like GE or Cachy Proton)

## Rocksmith, Slopsmith/[fee\[dB\]ack](https://github.com/got-feedBack)
* Getting Rocksmith 2014 running on Linux is a massive pain, I eventually gave up because I didn't care enough (haven't tried 2012 because it's simply a worse version and I don't think I've heard a single good thing about Rocksmith+)
* fee\[dB\]ack (formerly known as Slopsmith) is kind of like an open source Rocksmith. Currently still early in development and there are quite a few minor issues but it does work (I've used the AppImage of the desktop version).
  * They renamed it after [getting DMCA'd by Ubisoft](https://www.reddit.com/r/rocksmith/comments/1u6jg96/dmca_takedown_notice_for_slopsmith/) because Slopsmith could read and convert the Rocksmith song files
  * If you can get Slopsmith you can use that to convert your Rocksmith DLCs (official and custom) into sloppaks (which fee\[dB\]ack can read). You will need version 0.2.9 or older as it was renamed with 0.3.0 (and the conversion functionality removed). Unfortunately most (if not all) repos of it and its forks got taken down
  * There is probably other software/plugins that allow you to use Rocksmith DLCs in fee\[dB\]ack
  * Use [FeedForge](https://github.com/balki97/FeedForge) to convert Rocksmith DLCs to feedpaks

## Speedrunning
* Diablo https://www.speedrun.com/diablo/guides/muv82 (currently doesn't work with newer LiveSplit versions, must install an older one that doesn't require .NET 4.8.1 (4.8 is fine))
* Left 4 Dead 2 https://l4dsr.github.io/l4dsr-wiki/docs/tutorials/welcome/linux
* Neon White https://docs.google.com/document/d/12-BM0nusC79bhog5vBabLPoMeZBy2WoBKVKndpVXOJs (only for getting mods running, I couldn't get autosplitting to work)

## Wine
* Run Windows programs. For most games you want to use Proton (which is based on Wine), but some stuff only works with Wine directly. I only really use it for Cheat Engine and Diablo
* Might also want to install [Winetricks](https://github.com/Winetricks/winetricks) and protontricks for tinkering

# General programs

## [calibre](https://flathub.org/en/apps/com.calibre_ebook.calibre)
* Anything e-book related

## Discord
* Avoid the Snap version because it spams your journalctl every 5 seconds if you have apparmor (because Discord wants to read your processes but it is blocked). Workaround is either allowing it (if you want that) or using the flat version (note that you can't change the default keybinds in the flat version, you have to set up an additional hotkey, so for example you have the default mute hotkey ctrl+shift+M but also the custom one alt+numpad4 which you use)
* [Vencord](https://flathub.org/en/apps/dev.vencord.Vesktop) is a 3rd party Discord client that allows you more customization and allows you to select which audio to share when you stream (Discord announced that this feature will be coming though)

## [Easy Effects](https://flathub.org/en/apps/com.github.wwmm.easyeffects)
* Audio filters and such. Only tested briefly because it seemed worse than Discord's and OBS' noise filters

## [Fluxer](https://fluxer.com/)
* Open source Discord alternative/clone/whatever you want to call it. Can be self hosted (though it's currently fairly complicated but should be easier in the future)!
* Currently still in beta but most of Discord's features are replicated already
* Obviously doesn't have the userbase of Discord, but nothing is stopping you from using both

## [footswitch](https://github.com/rgerganov/footswitch)
* Software to remap my foot pedal (iKKEGOL FS2016-A (I don't recommend to use Amazon, but [here's the link](https://www.amazon.de/dp/B01NAL3DV6) in case you can't find it and want the exact same one for whatever reason)). The official software is only available for Windows, see [their website](https://pcsensor.com/download/)
* My foot pedal only has one switch/button, other pedals might need other software. Check [footswitch-rs](https://github.com/Silicon1602/footswitch-rs) (haven't tested) or [pedalctl](https://github.com/Schmoller/pedalctl/) (had to fix some errors before I could build it and it didn't work with my pedal)

## [FreeTube](https://flathub.org/en/apps/io.freetubeapp.FreeTube)
* Privacy focused YouTube app. I like to use it to watch videos that I don't want to give a "view" (usually ones with terrible titles)

## [Gear Lever](https://flathub.org/en/apps/it.mijorus.gearlever)
* Manage and update AppImages on Kubuntu (on CachyOS I use Shelly)

## [KDE Connect](https://kdeconnect.kde.org/)
* Connect your computer and phone (or other computers) to send files, share the clipboard or send remote inputs

## [KeePassXC](https://keepassxc.org/)
* Password manager. IIRC the flat version has issues with the Waterfox browser extension, so I use the snap version

## [Keyboard Center](https://github.com/zocker-160/keyboard-center)
* G Hub alternative made for Linux, allows macro mapping and such for Logitech keyboards
* Note to self: make sure to enable OpenRGB integration in the settings

## [NoiseTorch](https://github.com/noisetorch/NoiseTorch)
* Provides a noise filtered virtual microphone. Useful if you have a sensitive, proper microphone and want to play things like Lethal Company or R.E.P.O. (or you just don't to be a nuisance to others due to constant background noise)
* Project seems to no longer be updated. There might be other/better/newer alternatives. Perhaps [crimson-mute](https://github.com/bashmd/crimson-mute) but I couldn't get it running on Kubuntu (haven't tried on CachyOS)
* You can also use Easy Effects but that is applied on the microphone itself. From my testing (in maybe early to mid 2026) Easy Effects' noise suppression worsens the quality a bit more than Discord's or OBS' noise filters, so I use those when I can and use Noise Torch for the other situations

## [OpenRGB](https://openrgb.org/)
* Manage RGB lighting. I have a profile to have my keyboard's RGB turned off on launch via Keyboard Center. Using non-flat version because I can't set up the udev rules with it

## [Pipeweaver](https://github.com/pipeweaver/pipeweaver)
* (not tested yet) Should allow advanced audio routing, somewhat similar to VoiceMeeter on Windows. I plan to test it to separate my music into another track so my GPU SR replay buffer videos can easily have it removed

## [Solaar](https://github.com/pwr-Solaar/Solaar)
* Also for Logitech devices under Linux. I'm only using it to display my mouse's battery level

## [Rclone](https://rclone.org/)
* To sync my kdbx file elsewhere

## [Sunshine](https://flathub.org/en/apps/dev.lizardbyte.app.Sunshine) & [Moonlight](https://flathub.org/en/apps/com.moonlight_stream.Moonlight)
* Stream the desktop/game from one computer to another. Similar to TeamViewer (ew) or Steam's streaming. Sunshine is the host software, Moonlight the client that connects to it

## [Waterfox](https://www.waterfox.com/download/)
* Firefox fork without the telemetry stuff
* Recommended extensions: Dark Reader (dark mode), DeArrow (replace clickbait thumbnails), KeePassXC-Browser, PocketTube (hide specific types of videos, like live or shorts, from your subscription page, also gets rid of the "Most relevant" shelf), Return YouTube Dislike (to help spot bad videos), SponsorBlock (skip sponsor segments aka ads inside of videos), uBlock Origin, uMatrix (only for advanced users, will break lots of websites, requires tinkering), YouTube Search Fixer
* For CachyOS: The version from the Seafoam Labs recommended repo works out of the box with KeePassXC and has proper icons
### For Kubuntu
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
