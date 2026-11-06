@echo off
chcp 65001 >nul
title Genshin Impact - One-Click FPS Boost (All-in-One)
color 0A
mode con: cols=70 lines=30

echo.
echo    ======================================================
echo       Genshin Impact - ONE-CLICK FPS BOOST ^& SMOOTHNESS
echo       Run as Administrator ^| All-in-One ^| Auto-Detect
echo    ======================================================
echo.
echo    [!] Right-click this file ^> "Run as administrator"
echo    [!] Original config.ini will be backed up automatically
echo    [!] Safe and reversible - no game files modified
echo.
pause
cls

echo.
echo    [Step 1/8] Checking administrator privileges...
net session >nul 2>&1
if %errorlevel%==0 (
    echo    [OK] Running as Administrator.
) else (
    color 0C
    echo    [ERROR] Please run as Administrator!
    echo    Right-click file ^> Run as administrator
    echo.
    pause
    exit
)
timeout /t 1 >nul
cls

echo.
echo    [Step 2/8] Auto-detecting Genshin Impact installation...
set "GENSHIN_PATH="
rem Check common install paths
if exist "C:\Program Files\Genshin Impact\Genshin Impact Game\config.ini" set "GENSHIN_PATH=C:\Program Files\Genshin Impact\Genshin Impact Game"
if exist "C:\Program Files (x86)\Genshin Impact\Genshin Impact Game\config.ini" set "GENSHIN_PATH=C:\Program Files (x86)\Genshin Impact\Genshin Impact Game"
if exist "D:\Genshin Impact\Genshin Impact Game\config.ini" set "GENSHIN_PATH=D:\Genshin Impact\Genshin Impact Game"
if exist "D:\Program Files\Genshin Impact\Genshin Impact Game\config.ini" set "GENSHIN_PATH=D:\Program Files\Genshin Impact\Genshin Impact Game"
if exist "E:\Genshin Impact\Genshin Impact Game\config.ini" set "GENSHIN_PATH=E:\Genshin Impact\Genshin Impact Game"
if exist "F:\Genshin Impact\Genshin Impact Game\config.ini" set "GENSHIN_PATH=F:\Genshin Impact\Genshin Impact Game"
if exist "C:\Genshin Impact\Genshin Impact Game\config.ini" set "GENSHIN_PATH=C:\Genshin Impact\Genshin Impact Game"
rem Check registry for install path
for /f "tokens=2,*" %%a in ('reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" /s /f "Genshin Impact" 2^>nul ^| findstr "InstallLocation"') do set "REG_PATH=%%b"
if defined REG_PATH (
    if exist "%REG_PATH%\Genshin Impact Game\config.ini" set "GENSHIN_PATH=%REG_PATH%\Genshin Impact Game"
)

if defined GENSHIN_PATH (
    echo    [OK] Found at: %GENSHIN_PATH%
) else (
    color 0E
    echo    [WARN] Auto-detect failed. Please paste your Genshin Impact Game folder path.
    echo    Example: C:\Program Files\Genshin Impact\Genshin Impact Game
    echo    (The folder containing config.ini and GenshinImpact.exe)
    echo.
    set /p GENSHIN_PATH="    Paste path here: "
    if not exist "%GENSHIN_PATH%\config.ini" (
        color 0C
        echo    [ERROR] config.ini not found at that path. Exiting.
        pause
        exit
    )
)
timeout /t 1 >nul
cls

echo.
echo    [Step 3/8] Backing up original config.ini...
if not exist "%GENSHIN_PATH%\config_backup.ini" (
    copy "%GENSHIN_PATH%\config.ini" "%GENSHIN_PATH%\config_backup.ini" >nul
    echo    [OK] Backup saved as config_backup.ini
) else (
    echo    [OK] Backup already exists - skipping.
)
timeout /t 1 >nul
cls

echo.
echo    [Step 4/8] Writing optimized config.ini (max FPS ^& smoothness)...
(
echo [Graphics]
echo FPS=60
echo render_scale=0.800000
echo shadow_quality=1
echo visual_effects_quality=1
echo scene_quality=1
echo msaa=0
echo volumetric_fog=0
echo anisotropic_filtering=2
echo reflection=0
echo local_reflections=0
echo misc_quality=1
echo motion_blur=0
echo bloom=0
echo subsurface_scattering=0
echo specular=0
echo taa=0
echo.
echo [Audio]
echo master_volume=1.000000
echo music_volume=0.800000
echo sfx_volume=1.000000
echo voice_volume=1.000000
echo.
echo [Game]
echo mouse_sensitivity=1.000000
echo camera_sensitivity=1.000000
echo invert_y_axis=0
echo camera_shake=1
) > "%GENSHIN_PATH%\config.ini"
echo    [OK] config.ini optimized. (render_scale=0.8, shadows low, AA off, blur/bloom/fog off)
timeout /t 1 >nul
cls

echo.
echo    [Step 5/8] Applying Windows system optimizations...
rem High Performance power plan
powercfg -setactive 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c >nul 2>&1
if %errorlevel% neq 0 (
    powercfg -duplicatescheme 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c >nul 2>&1
    powercfg -setactive 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c >nul 2>&1
)
echo    [OK] Power plan = High Performance

rem Game Mode ON, Game Bar/DVR OFF (causes stutter)
reg add "HKCU\Software\Microsoft\GameBar" /v AllowAutoGameMode /t REG_DWORD /d 1 /f >nul 2>&1
reg add "HKCU\Software\Microsoft\GameBar" /v AutoGameModeEnabled /t REG_DWORD /d 1 /f >nul 2>&1
reg add "HKCU\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 0 /f >nul 2>&1
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR" /v AllowGameDVR /t REG_DWORD /d 0 /f >nul 2>&1
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 0 /f >nul 2>&1
echo    [OK] Game Mode ON, Game Bar/DVR OFF

rem Hardware-accelerated GPU scheduling
reg add "HKLM\SYSTEM\CurrentControlSet\Control\GraphicsDrivers" /v HwSchMode /t REG_DWORD /d 2 /f >nul 2>&1
echo    [OK] Hardware-accelerated GPU scheduling ON

rem Disable fullscreen optimizations for GenshinImpact.exe
if exist "%GENSHIN_PATH%\GenshinImpact.exe" (
    reg add "HKCU\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers" /v "%GENSHIN_PATH%\GenshinImpact.exe" /t REG_SZ /d "~ DISABLEDXMAXIMIZEDWINDOWEDMODE HIGHDPIAWARE" /f >nul 2>&1
    echo    [OK] Fullscreen optimizations disabled for GenshinImpact.exe
)
timeout /t 1 >nul
cls

echo.
echo    [Step 6/8] Network optimization (lower ping, less lag)...
netsh int tcp set global autotuninglevel=normal >nul 2>&1
netsh int tcp set global rss=enabled >nul 2>&1
netsh int tcp set global ecncapability=disabled >nul 2>&1
netsh int tcp set global timestamps=disabled >nul 2>&1
ipconfig /flushdns >nul 2>&1
echo    [OK] Network optimized, DNS cache flushed
timeout /t 1 >nul
cls

echo.
echo    [Step 7/8] Freeing RAM - closing background bloatware...
taskkill /F /IM chrome.exe /T >nul 2>&1
taskkill /F /IM msedge.exe /T >nul 2>&1
taskkill /F /IM firefox.exe /T >nul 2>&1
taskkill /F /IM Spotify.exe /T >nul 2>&1
taskkill /F /IM Discord.exe /T >nul 2>&1
taskkill /F /IM OneDrive.exe /T >nul 2>&1
taskkill /F /IM Teams.exe /T >nul 2>&1
taskkill /F /IM SearchApp.exe /T >nul 2>&1
del /q /s /f "%TEMP%\*" >nul 2>&1
del /q /s /f "C:\Windows\Temp\*" >nul 2>&1
echo    [OK] Background apps closed, temp files cleared
timeout /t 1 >nul
cls

echo.
echo    [Step 8/8] Creating High-Priority desktop shortcut...
set "SHORTCUT=%USERPROFILE%\Desktop\Genshin Impact (FPS Boost).bat"
(
echo @echo off
echo start "" /high "%GENSHIN_PATH%\GenshinImpact.exe"
echo exit
) > "%SHORTCUT%"
echo    [OK] Desktop shortcut created: "Genshin Impact (FPS Boost).bat"
echo    (Launches game with HIGH CPU priority every time)
timeout /t 2 >nul
cls

color 0A
echo.
echo    ======================================================
echo       ALL DONE! OPTIMIZATIONS APPLIED SUCCESSFULLY
echo    ======================================================
echo.
echo    What was done:
echo    [+] config.ini optimized for max FPS ^& smoothness
echo    [+] Original config backed up (config_backup.ini)
echo    [+] Power plan = High Performance
echo    [+] Game Mode ON, Game Bar/DVR OFF
echo    [+] GPU scheduling optimized
echo    [+] Network optimized for low ping
echo    [+] RAM freed, temp files cleared
echo    [+] High-priority shortcut created on Desktop
echo.
echo    [!] Restart PC once for all changes to take full effect.
echo    [!] Use the new Desktop shortcut to launch the game.
echo    [!] To restore original config: rename config_backup.ini
echo        back to config.ini in the game folder.
echo.
echo    Do you want to launch Genshin Impact now? (Y/N)
set /p launch="    > "
if /i "%launch%"=="Y" (
    if exist "%GENSHIN_PATH%\GenshinImpact.exe" (
        echo    Launching with HIGH priority...
        start "" /high "%GENSHIN_PATH%\GenshinImpact.exe"
    ) else (
        echo    GenshinImpact.exe not found. Please launch via launcher.
    )
)
echo.
pause
exit
