# WinSpotlightGNOME
A utility for automatically downloading Windows Spotlight images, and setting them as your desktop background in GNOME. Modified from [my own tool for doing the same thing on KDE](https://github.com/ryangwsimmons/WinSpotlightKDE).

This project is specifically for Linux systems using systemd and running GNOME, such as Ubuntu, Manjaro GNOME, or Fedora Workstation. However, with a little work, it could easily be modified to work with other desktop environments, or with an alternative to systemd timers such as cron.
<br/><br/><br/>

Thank you to [ORelio](https://github.com/ORelio/Spotlight-Downloader) on GitHub for their documentation on how the Spotlight API works. Without it, this project would have been significantly harder to finish.

## Dependencies
- Bash (may work with other shells, but this has not been tested)
- [cURL](https://curl.haxx.se/) (for sending an http request to the Spotlight API)
- [Jq](https://stedolan.github.io/jq/) (for parsing JSON returned by the API)

## Installation
1. Clone or download this repo.
2. Open a terminal and `cd` into the repo folder that you downloaded.
3. Make the `winspotlightgnome` script executable:
    ```
    chmod +x winspotlightgnome
    ```
4. Copy the `winspotlightgnome` script to the `opt` folder on your system:
    ```
    sudo cp winspotlightgnome /opt/
    ```
5. Copy the WinSpotlightgnome `.service` and `.timer` files to the systemd user unit directory:
    ```
    sudo cp winspotlightgnome.service winspotlightgnome.timer /usr/lib/systemd/user/
    ```
6. Reload the systemd user daemon:
    ```
    systemctl --user daemon-reload
    ```
7. Enable and start the WinSpotlightgnome systemd timer
    ```
    systemctl --user enable winspotlightgnome.timer
    systemctl --user start winspotlightgnome.timer
    ```

## Configuration
The following options can be changed by modifying the `winspotlightgnome` script, changing the various variables at the top of the file:

### **hRes**
The horizontal resolution of the image that will be downloaded. Normally, this should be set to the horizontal resolution of your monitor.

### **vRes**
The vertical resolution of the image that will be downloaded. Same deal as above.

### **locale**
An option sent to the Spotlight API. Since the Spotlight API isn't well documented, I'm not sure exactly what this does, but presumably it affects what kind of images the API sends to you.

### **country**
Same as above.

### **imgPath**
The path where images downloaded by the utility will be stored.

### **deleteImagesAfterNumDays**
When the utility runs, all images as old or older than the number of days specified here will be deleted from the directory specified previously. If the value specified is less than or equal to 0, images will be preserved indefinitely.

### **setDesktop**
If set to yes, the desktop background will be changed when the utility downloads a new image.

## Updates
To update the utility, replace all of the files that have changed since the last version (or just replace them all, much easier) and run the following commands:

```
systemctl --user daemon-reload
systemctl --user reenable winspotlightgnome.timer
```
