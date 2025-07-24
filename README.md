## Download PsExec

Updated: July 9, 2025         
[**Download PsExec**](*)

### Introduction

Tools like Telnet and remote control solutions such as Symantec's PC Anywhere allow you to run programs on remote machines, but they often require complex setup and the installation of client software on each target system. PsExec offers a streamlined, lightweight alternative to Telnet, enabling you to launch processes on remote computers with full console interaction — all without needing to manually deploy client applications. Among PsExec's key strengths is its ability to open interactive command prompts on remote machines and enable tools like IpConfig to display remote data, even when those tools aren’t designed for networked use.

> Note: Some antivirus programs may flag one or more of these utilities as containing a "remote admin" virus. PsExec itself is not malicious, but it has been misused by malware in the past, which is why these alerts can occur.

### Using PsExec

Check out the July 2004 edition of *Windows IT Pro Magazine* for [Mark's article](*) that explores advanced PsExec techniques.

**Usage Syntax:**

```cmd
psexec [\\computer[,computer2[,...] | @file]][-u user [-p psswd]][-n s][-r servicename][-h][-l][-s|-e][-x][-i [session]][-c [-f|-v]][-w directory][-d][-
<priority>][-g n][-a n,n,...][-accepteula][-nobanner] cmd [arguments]
```

| Parameter       | Description                                                                                                                                                                                                                          |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **-a**          | Specifies which CPUs the application can utilize by listing them, separated by commas (CPU 1 is the lowest number). For instance, to run a program on CPU 2 and 4, use: `-a 2,4`                                                     |
| **-c**          | Copies the target executable to the remote computer for execution. If you skip this flag, the app must already be available in the system path of the remote machine.                                                                |
| **-d**          | Does not wait for the process to finish (runs non-interactively).                                                                                                                                                                    |
| **-e**          | Skips loading the specified user's profile during execution.                                                                                                                                                                         |
| **-f**          | Forces the file to be copied, even if it already exists on the destination system.                                                                                                                                                   |
| **-i**          | Launches the application so it can interact with the desktop session specified. If no session is provided, it defaults to the console session. This flag is **essential** when running interactive console apps with redirected I/O. |
| **-h**          | On systems running Vista or newer, runs the process with the user account's elevated token, when available.                                                                                                                          |
| **-l**          | Runs the process with limited user rights (removes administrative privileges and only allows standard user rights). On Windows Vista, this results in a Low Integrity process.                                                       |
| **-n**          | Sets a timeout period, in seconds, for connecting to remote systems.                                                                                                                                                                 |
| **-p**          | Optional password for the specified user. If omitted, you'll be prompted to enter the password securely.                                                                                                                             |
| **-r**          | Indicates the name of the remote service to create or communicate with.                                                                                                                                                              |
| **-s**          | Runs the remote task under the System account.                                                                                                                                                                                       |
| **-u**          | Optional user name used for logging into the remote host.                                                                                                                                                                            |
| **-v**          | Only copies the file if it's newer or has a higher version than the one currently on the remote system.                                                                                                                              |
| **-w**          | Defines the working directory for the process on the remote host.                                                                                                                                                                    |
| **-x**          | Shows the application's user interface on the secure Winlogon desktop (local system only).                                                                                                                                           |
| **-priority**   | Allows you to define the process's priority using `-low`, `-belownormal`, `-abovenormal`, `-high`, or `-realtime`. Use `-background` on Vista to apply low resource priority for memory and I/O.                                     |
| **computer**    | Tells PsExec which remote system(s) to run the application on. If no machine is listed, it defaults to the local host. Using a wildcard like (`\\*`) will execute the command on all machines in the current domain.                 |
| **@file**       | Runs the command on each system listed within the specified text file.                                                                                                                                                               |
| **cmd**         | Name of the program to execute.                                                                                                                                                                                                      |
| **arguments**   | Command-line arguments to pass to the application (make sure to use absolute paths valid on the remote system).                                                                                                                      |
| **-accepteula** | Automatically accepts the license agreement, suppressing the EULA dialog.                                                                                                                                                            |
| **-nobanner**   | Suppresses the startup banner and copyright information.                                                                                                                                                                             |

When launching applications with names that contain spaces, enclose them in quotes. For example:

```cmd
psexec \\marklap "c:\\long name app.exe"
```

Remote input is only received after pressing the Enter key. To terminate the remote task, press Ctrl-C.

If no user is specified, the remote process will run under your local credentials but won't have access to network resources (as it will be impersonating your local context). To gain access to network shares or run under a specific account, use the `Domain\User` format. Both the command and password are encrypted during transmission to the remote machine.

Keep in mind that PsExec does not return its own error codes — it simply passes along those returned by the executed application.

### Examples

This article I wrote [explains how PsExec works](*) and offers tips for effective usage:

The command below starts an interactive command prompt session on `\\marklap`:

```cmd
psexec -i \\marklap cmd
```

This line runs IpConfig on the remote system with the `/all` flag and streams the output back locally:

```cmd
psexec -i \\marklap ipconfig /all
```

This example uploads `test.exe` to the remote system and executes it interactively:

```cmd
psexec -i \\marklap -c test.exe
```

If the application is already installed on the target system but not in the path, provide its absolute location:

```cmd
psexec -i \\marklap c:\bin\test.exe
```

To open Regedit with System privileges and access sensitive registry keys like SAM and SECURITY:

```cmd
psexec -i -d -s c:\windows\regedit.exe
```

Use this command to launch Internet Explorer under a restricted user context:

```cmd
psexec -l -d "c:\program files\internet explorer\iexplore.exe"
```
