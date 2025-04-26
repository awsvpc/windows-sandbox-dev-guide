<pre>
  Add Extension to Edge in Windows Sandbox
1. Create a folder on your host machine with the following files:
Let's say: C:\Sandbox\EdgeSetup

a) Script to install the extension (InstallExtension.ps1)
powershell

# ID for AdBlock extension
$extensionID = "gighmmpiobklfepjocnamgkkbiglidom"
$regPath = "HKLM:\Software\Policies\Microsoft\Edge\ExtensionInstallForcelist"

# Create registry key
New-Item -Path $regPath -Force | Out-Null
Set-ItemProperty -Path $regPath -Name "1" -Value "$extensionID;https://edge.microsoft.com/extensionwebstorebase/v1/crx"
b) Startup batch file (Startup.cmd)
cmd
powershell -ExecutionPolicy Bypass -File InstallExtension.ps1
start msedge
2. Create the Windows Sandbox Configuration File (EdgeWithExtension.wsb)
xml
</pre>
<Configuration>
  <MappedFolders>
    <MappedFolder>
      <HostFolder>C:\Sandbox\EdgeSetup</HostFolder>
      <SandboxFolder>C:\EdgeSetup</SandboxFolder>
      <ReadOnly>false</ReadOnly>
    </MappedFolder>
  </MappedFolders>
  <LogonCommand>
    <Command>C:\EdgeSetup\Startup.cmd</Command>
  </LogonCommand>
</Configuration>
<pre>
3. Launch Sandbox with the .wsb file
Save the .wsb file and double-click it to launch Windows Sandbox.

It will:
Map your folder into the sandbox.
Run the PowerShell script to install the extension via policy.
Launch Edge with AdBlock auto-installed.

</pre>
