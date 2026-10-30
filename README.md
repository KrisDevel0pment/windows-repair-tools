# windows-repair-tools


@echo off
setlocal
title Reparation du spouleur d'impression

fltmc >nul 2>&1
if errorlevel 1 (
    powershell -NoProfile -Command "Start-Process -FilePath '%~f0' -Verb RunAs"
    exit /b
)

net stop spooler >nul 2>&1
timeout /t 20 /nobreak >nul
net start spooler >nul 2>&1
timeout /t 3 /nobreak >nul

sc query spooler | findstr /C:"RUNNING" >nul
if errorlevel 1 (
    powershell -NoProfile -Command "Add-Type -AssemblyName System.Windows.Forms; [System.Windows.Forms.MessageBox]::Show('Echec du redemarrage du spouleur.', 'Erreur', 'OK', 'Error')"
) else (
    powershell -NoProfile -Command "Add-Type -AssemblyName System.Windows.Forms; [System.Windows.Forms.MessageBox]::Show('Spouleur redemarre avec succes.', 'Imprimante', 'OK', 'Information')"
)
exit
  