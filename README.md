# ProductivityTools.StartApplication

A PowerShell module to simplify starting, stopping, and auto-launching applications using simple, memorable keys.

## Overview

This module allows you to define a list of applications in an XML configuration file. You can then use the `Start-Application` command with a short key to launch an application, terminate its process, or run a set of predefined "autostart" applications.

It's designed to streamline workflows where you frequently open or close the same set of applications.

## Features

*   **Launch by Key**: Start any configured application using a simple key (e.g., `Start-Application -Key Notepad`).
*   **Terminate by Key**: Stop the process associated with an application key (e.g., `Start-Application -Key Notepad -Kill`).
*   **Autostart**: Launch a group of applications marked for autostart with a single command (`Start-Application -Autostart`).
*   **Simple Configuration**: Manage applications through an easy-to-edit XML file.
*   **Automatic Configuration**: Creates a default `StartApplication.xml` on first run if one is not found.


## Setup

1.  **Import the Module**:
    ```powershell
    Import-Module -Path ".\Start-Application\Start-Application.psd1"
    ```

2.  **Configuration File**:
The module relies on an XML file named `StartApplication.xml`. On its first run, the module will automatically create a default version of this file in the module's directory.

    For more control, you can place this file in a custom directory and point the module to it using the `Set-StartApplicationConfigurationPath` command:
    ```powershell
    # This command depends on the PSGet-Configuration module
    Set-StartApplicationConfigurationPath -Path "C:\Your\Custom\Path"
    ```

### Configuration File Format

The `StartApplication.xml` file contains a list of `<Application>` nodes. Each node must have the following structure:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Applications>
  <Application>
    <Key>Notepad</Key>
    <Description>Windows Notepad</Description>
    <ProcessName>notepad</ProcessName>
    <ApplicationPath>C:\Windows\System32\notepad.exe</ApplicationPath>
    <AutoStart>false</AutoStart> <!-- Set to 'true' to include in autostart -->
  </Application>
  <Application>
    <Key>Calc</Key>
    <Description>Windows Calculator</Description>
    <ProcessName>CalculatorApp</ProcessName>
    <ApplicationPath>calc.exe</ApplicationPath>
    <AutoStart>false</AutoStart>
  </Application>
</Applications>
```

*   **Key**: The short, unique identifier you will use in commands.
*   **Description**: A friendly name for the application.
*   **ProcessName**: The process name used when terminating (`-Kill`) the application.
*   **ApplicationPath**: The full path or executable name to be launched.
*   **AutoStart**: `true` or `false`. Determines if the application is launched with the `-Autostart` switch.

## Usage

### List Available Application Keys
Running the command without parameters displays the configured applications.
```powershell
Start-Application
```

### Start an Application
Use the `-Key` parameter to specify which application to launch.
```powershell
Start-Application -Key Notepad
```

### Kill an Application
Use the `-Kill` switch along with the `-Key`. This will stop the process identified by `<ProcessName>` in the XML.
```powershell
Start-Application -Key Notepad -Kill
```

### Run Autostart Applications
The `-Autostart` switch launches all applications where `<AutoStart>` is set to `true`.
```powershell
Start-Application -Autostart
```

## Development

The `Start-Application\Runner.ps1` script provides a simple way to test the module's functionality during development. You can modify it to quickly import the module and test different commands.

```