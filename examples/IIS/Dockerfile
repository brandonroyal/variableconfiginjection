FROM microsoft/windowsservercore
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

# Wait-Service is a tool from Microsoft for monitoring a Windows Service
ADD https://raw.githubusercontent.com/Microsoft/Virtualization-Documentation/live/windows-server-container-tools/Wait-Service/Wait-Service.ps1 /

# Inject-Variables added here
ADD https://raw.githubusercontent.com/Inject-Variables.ps1

# Install Windows features for IIS
RUN Add-WindowsFeature Web-server, NET-Framework-45-ASPNET, Web-Asp-Net45

RUN Remove-Website 'Default Web Site'

# Set up website: aspnet-webforms
COPY website /websites/website
RUN New-Website -Name 'website' -PhysicalPath "C:\websites\website" -Port 80 -Force
EXPOSE 80

# Inject-Variables added *before* default run commands
CMD /Inject-Variables.ps1 -ConfigPath "./Web.config"; /Wait-Service.ps1 -ServiceName W3SVC -AllowServiceRestart