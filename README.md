# Stream_Live_Checker

A simple Stream Checker for Twitch coded in Python.

This program works in the background of your PC checking if the streamer that you like opened stream in Twitch.

To work you have to install:

    requests
    time
    webbrowser
    os

To install them you have to execute:

    pip install requests

If you want to execute this with Windows when it starts, you have to convert it in to an .exe, to make this you have to install pyinstaller, so put into the console:

    pip install pyinstaller
    
Later, you have to navigate to the file directory using:

    cd "File_Directory"

Finally, to convert the .py into an .exe, you have to put:

    pyinstaller --onefile "Name_Of_The_File.py"

And to execute him with Windows press Win+R and type:

    shell:startup

Make a direct access (more recomendated) or put the .exe into the folder.

But if you want to make the program working in the background of your PC, you only have to change pyinstaller --onefile "Name_Of_The_File.py" for:

    pyinstaller --noconsole --onefile tu_script.py

This Twitch checker is free to use, it is this simple code:

    import requests
    import time
    import webbrowser
    import os
    
    # Configuración
    CLIENT_ID = "YOUR_CLIENT_ID"  # Reemplázalo con el Client ID de Twitch
    CLIENT_SECRET = "YOUR_CLIENT_SECRET"  # Reemplázalo con el Client Secret de Twitch
    STREAMER_NAME = "STREAMER_NAME"  # Reemplázalo con el nombre del streamer
    CHECK_INTERVAL = TIME_SINCE_LAST_CHECK # (more recomendated 30)  # Segundos entre cada comprobación
    stream_url = f"https://www.twitch.tv/{STREAMER_NAME}"
    opened = False  # Indica si la pestaña ya está abierta
    
    # Función para obtener el token de acceso de Twitch
    def get_access_token():
        url = "https://id.twitch.tv/oauth2/token"
        payload = {
            "client_id": CLIENT_ID,
            "client_secret": CLIENT_SECRET,
            "grant_type": "client_credentials"
        }
        response = requests.post(url, data=payload)
        return response.json().get("access_token")
    
    # Función para verificar si el streamer está en vivo
    def is_stream_live(token):
        url = f"https://api.twitch.tv/helix/streams?user_login={STREAMER_NAME}"
        headers = {
            "Client-ID": CLIENT_ID,
            "Authorization": f"Bearer {token}"
        }
        response = requests.get(url, headers=headers)
        data = response.json()
        return bool(data.get("data"))
    
    # Obtener el token al inicio
    access_token = get_access_token()
    
    while True:
        try:
            if is_stream_live(access_token):
                if not opened:
                    webbrowser.open(stream_url)  # Abre la pestaña en el navegador predeterminado
                    opened = True
                    print(f"{STREAMER_NAME} está en vivo. Abriendo Twitch...")
            else:
                if opened:
                    os.system("taskkill /IM CHANGE_THIS_TO_THE_NAME_OF_THE_EXE_OF_YOUR_BROWSER_SOMETHING_LIKE_OPERA.EXE /F")  # Cierra el navegador en Windows
                    opened = False
                    print(f"{STREAMER_NAME} terminó el directo. Cerrando Twitch...")
                    
        except Exception as e:
            print(f"Error: {e}")
    
        time.sleep(CHECK_INTERVAL)
