ARG tagversion="ltsc2019"
FROM mcr.microsoft.com/windows/servercore:${tagversion}

# Powershell
SHELL [ "powershell", "-Command" ]
ENTRYPOINT ["powershell.exe"]

# Set Execution Policy And install Chocolatey
RUN Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))