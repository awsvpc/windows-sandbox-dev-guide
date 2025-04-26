Using custom configurations
Windows Sandbox can be configured by using XML formatted configuration files with a .wsb extension.  Windows will automatically open .wsb files using Windows Sandbox when you double click them, which is the easiest way to start a session with that configuration.  More information can be found in Microsoft's documentation.

You can download these example configurations.  Note these come with no warranty of any kind, and you will need to change paths to suit your environment.

Sharing a folder
You might want to pass a folder through to the sandbox so you can access files from your host system.  If you're going to do this, I recommend passing through a subfolder (not your whole C:\ !) that you've specifically designated for that purpose.  You can also use ReadOnly mode to help protect the host system.  Passing folders through is called mapping and is done using the MappedFolder directive.

An example configuration with a read only folder passed through from the host:

<Configuration>
<MappedFolders>
   <MappedFolder>
     <HostFolder>C:\Sandbox\Readonly</HostFolder>
     <ReadOnly>true</ReadOnly>
   </MappedFolder>
</MappedFolders>
</Configuration>
Configuration with a read only shared folder (ReadonlyFolder.wsb).
Alternatively you might want a folder set up to allow the sandbox to write into:

<Configuration>
<MappedFolders>
   <MappedFolder>
     <HostFolder>C:\Sandbox\FromSandbox</HostFolder>
     <ReadOnly>false</ReadOnly>
   </MappedFolder>
</MappedFolders>
</Configuration>
Configuration with a writeable shared folder (WriteableFolder.wsb).
You can also combine these:

<Configuration>
<MappedFolders>
   <MappedFolder>
     <HostFolder>C:\Sandbox\Readonly</HostFolder>
     <ReadOnly>true</ReadOnly>
   </MappedFolder>
   <MappedFolder>
     <HostFolder>C:\Sandbox\FromSandbox</HostFolder>
     <ReadOnly>false</ReadOnly>
   </MappedFolder>
</MappedFolders>
</Configuration>
Configuration with two mapped folders: one read only and one that's writeable (ReadonlyAndWriteableFolders.wsb).
Inside Windows Sandbox you can then access these folders from the desktop.  If you want the folder to appear in a particular place you can specify a full path using the <SandboxFolder> directive (e.g. <SandboxFolder>C:\readonly</SandboxFolder>):

Screenshot of Windows Sandbox with two mapped folders ("ReadOnly" and "FromSandbox") shown on the desktop.  Each folder is open in a window.  The read only folder shows an error explaining that a file cannot be created there.
Screenshot showing ReadonlyAndWriteableFolders.wsb in use.
Running commands
If there's something that you want to happen immediately after the sandbox starts you can include a LogonCommand section in the configuration and use the Command directive.  You can pair this with the mapped folder technique (above) to allow you to pass through an installation file.  I've done this in the past to install a fresh Mozilla Firefox in my sandbox, which is the example I give below.  You could also tell the sandbox to run a script on starting, rather than a single installer.

In the example below, the Firefox installer will be started automatically and run in silent mode.  We won't be shown any installation progress, but as the defaults are used we will get a Firefox shortcut on the desktop once installation completes.

<Configuration>
<MappedFolders>
   <MappedFolder>
     <HostFolder>C:\Sandbox\Readonly</HostFolder>
     <ReadOnly>true</ReadOnly>
   </MappedFolder>
</MappedFolders>
<LogonCommand>
  <Command>C:\Users\WDAGUtilityAccount\Desktop\Readonly\FirefoxSetup.exe /S</Command>
</LogonCommand>
</Configuration>
Configuration to pass in a read only folder and install Mozilla Firefox in silent mode (Firefox.wsb).
