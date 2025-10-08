# Twitch Live Checker

A lightweight and silent background tool built in Python to monitor if your favorite Twitch streamer goes live.  
When the streamer starts streaming, it will automatically open their Twitch channel in your browser. When the stream ends, it can optionally close the browser tab for you.


## 🎯 Features

- ✅ Works silently in the background
- 🔁 Automatically checks Twitch every X seconds
- 🌐 Opens the streamer's channel when live
- ❌ Optionally closes the browser when the stream ends (Windows only)
- 🧪 Uses Twitch's official API for accurate status detection


## 📦 Requirements

Install the following Python library:

    pip install requests


(Optional: `pyinstaller` for `.exe` generation)


## 🚀 How to Use

1. Create a Twitch Developer Application at:  
   https://dev.twitch.tv/console/apps  
   And note your:
   - `Client ID`
   - `Client Secret`

2. In the script, set your values:

    ```python
    CLIENT_ID = "YOUR_CLIENT_ID"
    CLIENT_SECRET = "YOUR_CLIENT_SECRET"
    STREAMER_NAME = "streamer_username"
    CHECK_INTERVAL = 30  # In seconds


3. Run the script:

        python twitch_checker.py


## 💾 Token Storage (Default Behavior)

By default, this program saves your Twitch access token in a safe local folder:

        Documents/
        └── naarvent's projects/
        └── TwitchLiveChecker/
        └── token.txt


This prevents generating a new token every time you launch the script.


## 🖥️ Run on Windows Startup (Optional)

1. Convert the script into a .exe using PyInstaller:

        pip install pyinstaller
        pyinstaller --noconsole --onefile twitch_checker.py

2. Press Win + R and type:

        shell:startup

3. Place a shortcut to the .exe file in that folder.


## 🛠 Customization

Change CHECK_INTERVAL to control how frequently it checks for the stream status.


## 📄 License
This Twitch Live Checker is open-source and free to use and modify.
Feel free to improve it or integrate it with your own automation workflows.


## 🧠 Credits
Developed in Python using:

        Twitch API
        requests
## Code

