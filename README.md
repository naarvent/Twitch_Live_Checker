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

3. Replace the browser process name if you want to close the stream tab automatically (e.g. chrome.exe, opera.exe):

        os.system("taskkill /IM opera.exe /F")

4. Run the script:

        python twitch_checker.py


## 💾 Token Storage (Default Behavior)

By default, this program saves your Twitch access token in a safe local folder:

        Documents/
        └── naarvent's projects/
        └── TwitchLiveChecker/
        └── token.txt


This prevents generating a new token every time you launch the script.


## 🌐 Browser Window Behavior

When the streamer goes live, their Twitch page will open in a **new separate window**, keeping your existing browser tabs untouched.

When the stream ends, this dedicated window can be closed automatically using `os.system("taskkill /IM your_browser.exe /F")`.


## 🖥️ Run on Windows Startup (Optional)

1. Convert the script into a .exe using PyInstaller:

        pip install pyinstaller
        pyinstaller --noconsole --onefile twitch_checker.py

2. Press Win + R and type:

        shell:startup

3. Place a shortcut to the .exe file in that folder.


## 🛠 Customization

Change CHECK_INTERVAL to control how frequently it checks for the stream status.

Adjust the browser process name based on your setup.

You can modify the webbrowser.open() behavior to use specific browsers if needed.


## 📄 License
This Twitch Live Checker is open-source and free to use and modify.
Feel free to improve it or integrate it with your own automation workflows.


## 🧠 Credits
Developed in Python using:

        Twitch API
        requests

Built with ♥ by naarvent


## Code

        import requests
        import time
        import webbrowser
        import os
        from pathlib import Path
        
        # --- Safe directory for storing the token ---
        documents_path = Path.home() / "Documents"
        project_dir = documents_path / "naarvent's projects" / "TwitchLiveChecker"
        project_dir.mkdir(parents=True, exist_ok=True)
        
        token_path = project_dir / "token.txt"
        
        # --- Configuration ---
        CLIENT_ID = "YOUR_CLIENT_ID"
        CLIENT_SECRET = "YOUR_CLIENT_SECRET"
        STREAMER_NAME = "STREAMER_NAME"
        CHECK_INTERVAL = 30  # seconds between checks
        stream_url = f"https://www.twitch.tv/{STREAMER_NAME}"
        opened = False  # To track if the stream tab is open
        
        # --- Token handling ---
        def get_access_token():
            if token_path.exists():
                with open(token_path, "r") as f:
                    token = f.read().strip()
                    if token:
                        return token
        
            url = "https://id.twitch.tv/oauth2/token"
            payload = {
                "client_id": CLIENT_ID,
                "client_secret": CLIENT_SECRET,
                "grant_type": "client_credentials"
            }
            response = requests.post(url, data=payload)
            token = response.json().get("access_token")
            if token:
                with open(token_path, "w") as f:
                    f.write(token)
            return token
        
        # --- Check if the streamer is live ---
        def is_stream_live(token):
            url = f"https://api.twitch.tv/helix/streams?user_login={STREAMER_NAME}"
            headers = {
                "Client-ID": CLIENT_ID,
                "Authorization": f"Bearer {token}"
            }
            response = requests.get(url, headers=headers)
            data = response.json()
            return bool(data.get("data"))
        
        # --- Main loop ---
        access_token = get_access_token()
        
        while True:
            try:
                if is_stream_live(access_token):
                    if not opened:
                        webbrowser.open_new(stream_url)  # Open in a new separate window
                        opened = True
                        print(f"{STREAMER_NAME} is live. Opening Twitch...")
                else:
                    if opened:
                        os.system("taskkill /IM chrome.exe /F")  # Replace with your browser's EXE name
                        opened = False
                        print(f"{STREAMER_NAME} ended the stream. Closing Twitch...")
            except Exception as e:
                print(f"Error: {e}")
        
            time.sleep(CHECK_INTERVAL)
