# 📱gallery-dl-on-android ⬇️

# The Ultimate Guide to Automated gallery-dl Downloads on Android with Termux

**(Placeholder: [Link to Your Video Guide Will Be Added Here])**

This guide will walk you through setting up `gallery-dl`, a powerful command-line program, on your Android device using Termux for automated downloads. We'll focus on a specific setup that leverages root access to use your Chrome browser's cookies for sites requiring login and saves downloads to a designated folder.

---

## Table of Contents

1.  [Introduction](#1-introduction)
2.  [Important Disclaimer](#2-important-disclaimer)
3.  [Requirements](#3-requirements)
4.  [Step-by-Step Instructions](#4-step-by-step-instructions)
    * [Part 1: Setting Up Termux](#part-1-setting-up-termux)
    * [Part 2: Installing gallery-dl](#part-2-installing-gallery-dl)
    * [Part 3: Creating the `termux-url-opener` Automation Script](#part-3-creating-the-termux-url-opener-automation-script)
5.  [How to Use Your Automated Downloader](#5-how-to-use-your-automated-downloader)
6.  [Customizing the Script (For Other Browsers/Preferences)](#6-customizing-the-script-for-other-browserspreferences)
7.  [Frequently Asked Questions (FAQs)](#7-frequently-asked-questions-faqs)
8.  [Troubleshooting Common Issues](#8-troubleshooting-common-issues)
9.  [Contributing & Support](#9-contributing--support)
10. [Image Integration Suggestions (For Guide Maintainers)](#10-image-integration-suggestions-for-guide-maintainers)

---

## 1. Introduction

Welcome! This guide will help you get `gallery-dl` running on your Android device using the Termux application.

**What is `gallery-dl`?**
`gallery-dl` is a versatile command-line program designed to download image galleries and collections from a multitude of websites. You can find more about it on its official GitHub page: [gallery-dl on GitHub](https://github.com/mikf/gallery-dl).

**What will this guide help you achieve?**
* Install `gallery-dl` in Termux on your Android device.
* Set up a `termux-url-opener` script for automated downloads: when you "Share" a URL from your browser to Termux, the script will automatically run `gallery-dl`.
* Configure `gallery-dl` to use your **Chrome browser's** cookies for accessing content requiring login (this specific method requires your device to be **rooted**).
* Ensure downloads are saved to a specific folder (`gallery-dl_downloads`) within your main Android "Downloads" folder.

**Brief overview of Termux and `termux-url-opener`:**
* **Termux:** A powerful terminal emulator and Linux environment for Android. It allows you to run many command-line tools. It's recommended to get it from [F-Droid](https://f-droid.org/packages/com.termux/) for the most up-to-date and reliable version. More info can be found on the [Termux GitHub](https://github.com/termux/termux-app).
* **`termux-url-opener`:** A special script Termux uses to handle URLs shared to it from other apps. We'll customize this script to trigger `gallery-dl`.

This guide aims to be step-by-step and accessible, even if you have limited experience with command-line interfaces.

## 2. Important Disclaimer

* **Root Requirement:** The specific script and cookie access method detailed in this guide for Chrome requires your Android device to be **rooted**. If your device is not rooted, you can still use `gallery-dl`, but you will need to explore alternative authentication methods (e.g., manually exporting cookies to a file, using username/password if supported by `gallery-dl` for the site, etc.), which are beyond the primary scope of this guide.
* **Script Specificity:** The provided `termux-url-opener` script is tailored for using Chrome cookies on a rooted device. While it works effectively for this scenario (as confirmed by the author of this guide request), you might need to adapt it if your setup or preferences differ (see the [Customizing the Script](#6-customizing-the-script-for-other-browserspreferences) section).
* **Use Responsibly:** Always respect website terms of service and copyright when downloading content.

## 3. Requirements

* **Android Device (Rooted):** Essential for Chrome cookie access as described.
* **Termux App:** Installed, preferably from [F-Droid](https://f-droid.org/packages/com.termux/). <img src="https://github.com/user-attachments/assets/bbf397e4-2009-466c-a026-302bc7024826" alt="icon" width="25"/>
* **Chrome Browser:** Installed, and you should be logged into any websites (e.g., Instagram) from which you intend to download restricted content.
* **Root Management App (e.g., Magisk, KSU, Apatch or Any Other Of Your Choice):** For granting Termux root permissions.
* **Internet Connection.**
* **Sufficient Device Storage.**

## 4. Step-by-Step Instructions

### Part 1: Setting Up Termux

1.  **Install Termux:**
    * Download and install the Termux app from F-Droid if you haven't already.

2.  **Initial Termux Setup:**
    * Open Termux.

      *<img src="https://github.com/user-attachments/assets/856b0e57-d6b8-4eb8-8cee-6a2f315fea42" alt="Screenshot_20250529-160020 Termux" width="300"/>*
    * **Update Packages:** Keep your environment fresh. Type and run:
        ```bash
        pkg update && pkg upgrade
        ```
        * Confirm with `Y` if prompted. For configuration file prompts, typing `N` (to keep current version) is usually safe.
    * **Request Storage Access:** This allows Termux to save files to your shared "Downloads" folder.
        ```bash
        termux-setup-storage
        ```
        * Tap "Allow" on the Android permission pop-up.

          *<img src="https://github.com/user-attachments/assets/67f3a0eb-da4c-4982-b5f6-15d04912ae6d" width="300"/>*

### Part 2: Installing gallery-dl

1.  **Install Python:** `gallery-dl` needs Python. In Termux:
    ```bash
    pkg install python
    ```
    * Confirm with `Y` if prompted.

2.  **Install `gallery-dl` using pip:** `pip` is Python's package manager.
    ```bash
    pip install --upgrade gallery-dl
    ```

### Part 3: Creating the `termux-url-opener` Automation Script

This script will enable the "Share to Termux" automation.

1.  **Create the `bin` directory:** This is where Termux looks for user scripts.

    ```bash
    mkdir -p ~/bin
    ```

2.  **Create the script file:**
    The `termux-url-opener` script needs to be created in the `~/bin/` directory. Its full path will be: `/data/data/com.termux/files/home/bin/termux-url-opener`.

      * **Method A: Using `nano` (command-line text editor in Termux)**
        This is the primary method described for creating the script within Termux.

        ```bash
        nano ~/bin/termux-url-opener
        ```

          * This command opens (or creates if it doesn't exist) the `termux-url-opener` file in the `nano` editor directly within Termux.
            *<img src="https://github.com/user-attachments/assets/1dcd181f-e4e6-49be-b494-34b7e59f86b2" alt="Nano Text Editor" width="300"/>*

      * **Method B: Using a GUI Text Editor or File Manager App (Alternative for easier editing)**
        If you find command-line text editing challenging, you can create or edit the `termux-url-opener` script using a graphical application.

          * **Suitable Apps:**
              * A simple text editor app installed on your Android device.
              * A root-enabled file manager that includes a text editor, such as MT Manager or ZArchiver. These apps can navigate to Termux's internal storage if they have root access.
          * **Steps for GUI method:**
            1.  Using your chosen app, navigate to the script's directory path: `/data/data/com.termux/files/home/bin/`
                *<img src="https://github.com/user-attachments/assets/5a66e27f-d047-4748-b440-864c71701cd1" alt="Path of Script" width="300"/>*
            2.  Create a new file named `termux-url-opener` (ensure it has no hidden `.txt` extension if your editor tends to add one). Or, if you've already created it with `nano` but want to edit it graphically, simply open the existing file.
            3.  Paste or type the script content (from the next step) into this file using the app's text editor.
            4.  **Important for external editors:** Ensure the file is saved with Unix-style line endings (LF). Some text editors, especially if you were to create the file on a Windows PC and transfer it, might use Windows-style line endings (CRLF), which can cause scripts to fail on Linux-based systems like Termux. Most good mobile text editors or file manager editors should handle this correctly by default.

3.  **Paste the script content:**
    Whether you are using `nano` (as per Method A above) or an external GUI editor (Method B), this is the specific script tailored for this guide. Copy and paste it exactly into the `termux-url-opener` file:

    ```bash
    #!/data/data/com.termux/files/usr/bin/bash
    # Ensure the Termux binary directories are in PATH
    export PATH=/data/data/com.termux/files/usr/bin:/data/data/com.termux/files/home/bin:$PATH

    # Full path to gallery-dl to avoid "not found"
    GALLERY_DL=/data/data/com.termux/files/usr/bin/gallery-dl

    # Define the target download directory
    DOWNLOAD_DIR_GALLERY_DL="/data/data/com.termux/files/home/storage/downloads/gallery-dl_downloads"
    # Note: gallery-dl will create this directory if it doesn't exist.

    # Use root to run gallery-dl and specify the output directory
    exec su -c "$GALLERY_DL --directory \"$DOWNLOAD_DIR_GALLERY_DL\" --cookies-from-browser chrome:/data/data/com.android.chrome/app_chrome/Default \"$1\""
    ```
    *<img src="https://github.com/user-attachments/assets/3a9dcc91-0f76-471d-8cfe-0a8a8d9dfc57" alt="Command Line" width="300"/>* *<img src="https://github.com/user-attachments/assets/a78c4ee0-0cd0-494a-b62d-1fd0341a0ffd" alt="GUI Based" width="300"/>*

4.  **Understanding the script:**

      * `#!/data/data/com.termux/files/usr/bin/bash`: The "shebang" line, tells Android to use Termux's `bash` to run the script.
      * `export PATH=...`: Ensures the script can find commands like `su` and `gallery-dl`, as the environment when called via "Share" can be minimal.
      * `GALLERY_DL=...`: Defines the full path to `gallery-dl` for reliability.
      * `DOWNLOAD_DIR_GALLERY_DL=...`: Sets the download location to `~/storage/downloads/gallery-dl_downloads` (a folder named `gallery-dl_downloads` in your main Android Downloads folder).
      * `exec su -c "$GALLERY_DL ..."`: The core command.
          * `su -c`: Executes the following command string as **root (superuser)**, which is vital for accessing Chrome's protected cookie files.
          * `--directory \"$DOWNLOAD_DIR_GALLERY_DL\"`: Instructs `gallery-dl` to save files to our defined download folder.
          * `--cookies-from-browser chrome:/data/data/com.android.chrome/app_chrome/Default`: Tells `gallery-dl` to use cookies from Chrome's default profile path (requires root).
          * `\"$1\"`: Passes the URL you shared (the first argument to the script) to `gallery-dl`.

5.  **Saving the script:**

      * **If using `nano`:**
        1.  Press **Volume Down + X** (this acts as Ctrl+X in Termux, which is the command to Exit).

            *<img src="https://github.com/user-attachments/assets/1378c2dc-8d99-448d-ab10-5321515371d2" alt="Ctrl+X" width="300"/>*
        2.  `nano` will now ask at the bottom of the screen: "Save modified buffer? (Y/N)" (This means: do you want to save your changes?). Type **`Y`** (for Yes).

            *<img src="https://github.com/user-attachments/assets/5ac72236-84d1-4d7b-ae80-7ed1e7e9a368" alt="Ctrl+O" width="300"/>*
        3.  Next, `nano` will show "File Name to Write: `~/bin/termux-url-opener`". The filename should already be correct as you opened it with this name. Press **Enter** to confirm and save the file.
           
        <!-- end list -->
          * After pressing Enter, `nano` will close, and you'll be returned to the Termux command prompt. Your script is now saved with the changes.
      * **If using a GUI Text Editor / File Manager App:**
        1.  Use your app's specific "Save" function or option to save the changes you made to the `termux-url-opener` file.
        2.  Double-check that the file is saved in the correct location: `/data/data/com.termux/files/home/bin/` and with the exact name `termux-url-opener` (no extra extensions like `.txt`).

6.  **Making the script executable:**
    For the script to run, it needs execute permissions. Based on practical experience that indicates comprehensive permissions are sometimes needed for reliable operation when invoked via Android's share mechanism, the following methods are provided.

      * **Method 1: Using `chmod` command in Termux (Recommended)**
        This command sets full read, write, and execute permissions for owner, group, and others, plus special SUID, SGID, and Sticky bits.

        ```bash
        chmod 7777 ~/bin/termux-url-opener
        ```

          * **Explanation:** `7777` sets special bits (SUID, SGID, Sticky) and `rwxrwxrwx` permissions. While highly permissive, this is suggested based on user experience for this specific script's invocation context. The `su -c` within the script is what primarily handles privilege escalation for `gallery-dl`.

      * **Method 2: Setting Permissions via a Root-Enabled File Manager (e.g., MT Manager)**

        1.  Navigate to `/data/data/com.termux/files/home/bin/` in your root file manager.
            *(Image Suggestion: Screenshot of MT Manager at this path)*
        2.  Long-press `termux-url-opener`, select "Properties".
            *(Image Suggestion: Screenshot of MT Manager context menu)*
        3.  Find "Permissions", tap "Modify".
            *(Image Suggestion: Screenshot of MT Manager Properties dialog)*
        4.  Check **ALL** boxes for "Read", "Write", "Exec" (for Owner, Group, Other) AND **ALL** "Special Permissions" boxes ("Set UID", "Set GID", "Sticky").
            *(Image Suggestion: Screenshot of MT Manager permissions dialog with ALL boxes checked)*
        5.  Apply/save changes.

## 5. How to Use Your Automated Downloader

1.  **Grant Root Access to Termux:**
    * The first time the script runs `su -c`, your root management app (e.g., Magisk) will ask to grant Termux root permissions. **Grant** this request. If you miss it, open your root app and grant it manually.
        *(Image Suggestion: Screenshot of a Magisk/SuperSU root request pop-up)*

2.  **Sharing a URL to Termux:**
    * In any app (Chrome, Instagram, etc.), find the content you want to download.
    * Use the app's "Share" feature.
        *(Image Suggestion: Screenshot of an app's share button)*
    * From the Android share sheet, select "Termux".
        *(Image Suggestion: Screenshot of the Android share sheet with "Termux" highlighted)*

3.  **How it works:**
    * Android sends the URL to your `~/bin/termux-url-opener` script.
    * The script runs `gallery-dl` as root, using Chrome cookies, and saves files to the folder specified in the script. Termux may show command-line activity.

4.  **Finding your downloaded files:**
    * Files are saved in the `gallery-dl_downloads` folder inside your main Android "Downloads" folder. (Within Termux, the path is `/data/data/com.termux/files/home/storage/downloads/gallery-dl_downloads`). Use your phone's file manager to access them.

## 6. Customizing the Script (For Other Browsers/Preferences)

The `termux-url-opener` script provided in this guide is specifically configured for:
* Using **Chrome browser** cookies.
* A device that is **rooted** (to access Chrome's internal cookie storage).
* Saving to a specific download directory (`~/storage/downloads/gallery-dl_downloads`).

**This setup works very well for the described scenario.** However, you might want to adapt it if:
* You prefer to use a different browser (e.g., Firefox for Android).
* Your device is not rooted (requiring a different cookie/authentication method).
* You want to change other `gallery-dl` options.

**General tips for customization:**

* **Changing the Browser for Cookies:**
    * You'll need to find the cookie database path for your desired browser on Android (this usually requires root).
    * Modify the `--cookies-from-browser` option in the script. For example, for Firefox, it might look something like:
        ```bash
        # Example for Firefox (path may vary, ensure your_profile.default is correct)
        # COOKIE_PATH="firefox:/data/data/org.mozilla.firefox/files/mozilla/your_profile.default" 
        # exec su -c "$GALLERY_DL --directory \"$DOWNLOAD_DIR_GALLERY_DL\" --cookies-from-browser $COOKIE_PATH \"$1\""
        ```
    * Consult the [gallery-dl documentation on Authentication](https://github.com/mikf/gallery-dl/blob/master/docs/configuration.rst#authentication-options) for supported browser keywords and paths.

* **Non-Rooted Devices:**
    * Accessing internal browser cookie databases directly usually requires root.
    * For non-rooted devices, you might need to:
        * Manually export cookies from your desktop browser to a `cookies.txt` file, transfer it to your Android device, and use the `gallery-dl --cookies /path/to/your/cookies.txt` option.
        * Use username/password authentication if `gallery-dl` supports it for the target site (`-u your_username -p your_password`), though this is generally less secure and might trigger 2FA.

* **Changing Download Directory:**
    * As explained in FAQ Q5, you can modify the `DOWNLOAD_DIR_GALLERY_DL` variable in the script.

* **Adding Other `gallery-dl` Options:**
    * You can add any other valid `gallery-dl` command-line options within the `exec su -c "..."` string. For example, `--verbose` for more detailed output, or options to control filename formatting. Refer to the [gallery-dl command-line options documentation](https://github.com/mikf/gallery-dl#command-line-options).

**Always test your script modifications carefully!**

## 7. Frequently Asked Questions (FAQs)

* **Q1:** The `termux-url-opener` script doesn't run when I share a URL.
    * **A:** Check: script location (`~/bin/termux-url-opener`), exact name, `7777` permissions (`ls -l ~/bin/termux-url-opener`), correct shebang (`#!/data/data/com.termux/files/usr/bin/bash`), presence of `export PATH...` line in script, Termux is selected in "Share" menu, Termux has root access (for `su -c`).

* **Q2:** I get "`gallery-dl`: inaccessible or not found" when the script runs.
    * **A:** Ensure `gallery-dl` is installed (`pip install --upgrade gallery-dl`). The `GALLERY_DL` variable and `export PATH` in your script should correctly point to it. Verify with `which gallery-dl` in Termux.

* **Q3:** "HttpError: 401 Unauthorized" or "NotFoundError".
    * **A:** Ensure you're logged into the site in Chrome. Verify the Chrome cookie path in the script (`/data/data/com.android.chrome/app_chrome/Default`) is correct. Confirm Termux has root access. The URL might be private or mistyped.

* **Q4:** Can I use cookies from other browsers?
    * **A:** Yes, but it requires modification. See the [Customizing the Script](#6-customizing-the-script-for-other-browserspreferences) section above.

* **Q5:** Where are my files downloaded? And can I change this location?
    * **A:** With the current script, files download to `gallery-dl_downloads` inside your main Android "Downloads" folder (Termux path: `/data/data/com.termux/files/home/storage/downloads/gallery-dl_downloads`).
    * To change this, edit the `DOWNLOAD_DIR_GALLERY_DL` variable in your `~/bin/termux-url-opener` script.

## 8. Troubleshooting Common Issues

* **`termux-url-opener: command not found` (when running manually in Termux):**
    * Close/reopen Termux. Run `hash -r`. Ensure `~/bin` is in your interactive `$PATH` (add `export PATH=$HOME/bin:$PATH` to `~/.bashrc` and `source ~/.bashrc` if needed). The `export PATH` *inside* the script handles the share intent environment.

* **Permission errors for `su -c` or cookie access:**
    * Verify Termux has root permissions granted in your Magisk/SuperSU app.

* **Script runs but `gallery-dl` fails:**
    * Test the `gallery-dl` command from `exec su -c "..."` manually in Termux (first type `su`, then the command) with `--verbose` for detailed errors. Log output as shown in the main guide if needed.

* **`nano` editor issues:**
    * Volume Down key = Ctrl. Ctrl+O (VolDown+O) to Save (then Enter). Ctrl+X (VolDown+X) to Exit. Enable Termux "Extra Keys Row" for on-screen Ctrl if needed.

## 9. Contributing & Support

* For issues with `gallery-dl` itself, please refer to the [gallery-dl GitHub Issues page](https://github.com/mikf/gallery-dl/issues).
* For issues with Termux, see the [Termux GitHub community](https://github.com/termux/termux-app/issues).
* If you have improvements for this guide, feel free to suggest them!

## 10. Image Integration Suggestions (For Guide Maintainers)

*(This section is a note for those maintaining this guide. These are points where screenshots would be highly beneficial for users.)*
* Termux app icon / F-Droid page for Termux.
* Initial Termux interface after opening.
* Android permission pop-up for storage access after `termux-setup-storage`.
* Empty `nano` editor interface after `nano ~/bin/termux-url-opener`.
* Termux with `nano`, highlighting Volume Down + O combination or on-screen extra keys for Ctrl.
* MT Manager (or similar root file manager) showing:
    * Navigation to `/data/data/com.termux/files/home/bin/`.
    * Context menu with "Properties" selected.
    * "Properties" dialog highlighting permissions/modify button.
    * Detailed permissions dialog with ALL boxes checked (RWE for all, SUID, SGID, Sticky).
* Magisk/SuperSU root request pop-up for Termux.
* An app's "Share" button example.
* Android share sheet with "Termux" highlighted.
* Example of downloaded files in the target `gallery-dl_downloads` directory.

---

This guide should now be more suitable for GitHub. You can copy this content into a `README.md` file in your repository. Remember to replace the video placeholder when you have the link!
