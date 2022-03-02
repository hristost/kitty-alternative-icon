Alternative icon for the [kitty](https://github.com/kovidgoyal/kitty) terminal
emulator. Depicts a cat sitting on top of a ADM-3A terminal.

Currently, I only have the source as an Affinity Designer file that makes
liberal use of blur effects. Thus, I can't export to svg without losing some of
the fluffiness of the cat.

## Original:

![Before](screenshot_old.png) 

## Alternative:

![After](screenshot_new.png)

# Using the icon

While it is unlikely that this icon gets merged upstream (see
[kitty#3310](https://github.com/kovidgoyal/kitty/issues/3310)), you might be able
to patch your kitty installation using `./patch.diff` and install from source.
Or, if using homebrew on mac, you can just change the icon after installing the
cask:

1. Edit the `kitty` formula: `brew edit kitty`
2. Add the following block after `preflight`:
  ```
    postflight do
      system_command 'curl',
                     args: ["https://raw.githubusercontent.com/hristost/kitty-alternative-icon/main/kitty.icns", "--output", "#{appdir}/kitty.app/Contents/Resources/kitty.icns"],
                     sudo: false
      system_command 'touch',
                     args: ["#{appdir}/kitty.app"],
                     sudo: false
      # It seems that replacing the icon breaks some security check for MacOS, and removing attributes
      # using the following command fixes it. Use at your own risk
      system_command 'xattr',
                     args: ["-cr", "#{appdir}/kitty.app"],
                     sudo: false
    end
  ```
3. Save the file and run `brew reinstall kitty`.
4. The postflight script we added changed the icon in the `.app` package after
  installing. If you can't see the icon, run `killall Finder && killall Dock` to
  clear the app icon cache.
