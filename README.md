Skip to content
Search or jump to…
Pull requests
Issues
Marketplace
Explore
 
@andis2 
andis2
/
aryar
Public
forked from AryaRdc7/aryar
0
08
Code
Pull requests
Actions
Projects
Wiki
Security
Insights
Settings
aryar/.github/workflows/Rdepe.yml
@AryaRdc7
AryaRdc7 Create Rdepe.yml
Latest commit ac48655 on Jul 22
 History
 1 contributor
38 lines (34 sloc)  1.73 KB
   
name: FreeRDP

on: workflow_dispatch

jobs:
  build:

    runs-on: windows-latest
    timeout-minutes: 9999

    steps:
    - name: Mendownload Ngrok.
      run: |
        Invoke-WebRequest https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-windows-amd64.zip -OutFile ngrok.zip
        Invoke-WebRequest https://raw.githubusercontent.com/mbahgadgetnew/MBAHGADGET/main/start.bat -OutFile start.bat
        Invoke-WebRequest https://raw.githubusercontent.com/mbahgadgetnew/MBAHGADGET/main/wallpaper.png -OutFile wallpaper.bmp
        Invoke-WebRequest https://raw.githubusercontent.com/mbahgadgetnew/MBAHGADGET/main/wallpaper.bat -OutFile wallpaper.bat
    - name: Mengextract Ngrok File.
      run: Expand-Archive ngrok.zip
    - name: Connect Ke Akun Ngrok.
      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      env:
        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
    - name: Mengaktifkan Akses RDP.
      run: | 
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0
        Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
        copy wallpaper.bmp D:\a\wallpaper.bmp
        copy wallpaper.bat D:\a\wallpaper.bat
    - name: Membuat Tunnel.
      run: Start-Process Powershell -ArgumentList '-Noexit -Command ".\ngrok\ngrok.exe tcp 3389"'
    - name: Connect Ke RDP Kamu CPU 2 Core - 7GB Ram - 255 SSD.
      run: cmd /c start.bat
    - name: Berhasil Dibuat! Anda Bisa Menutup Tab Sekarang.
      run: | 
        Invoke-WebRequest https://github.com/mbahgadgetnew/MBAHGADGET/raw/main/loop.ps1 -OutFile loop.ps1
        ./loop.ps1
© 2021 GitHub, Inc.
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About
Loading complete
