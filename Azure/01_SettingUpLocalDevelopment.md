# Setting Up Local Development with the Latest CLI

## Objective

Install and configure the latest version of the Azure CLI for local development to manage Azure infrastructure.

## Prerequisites

- A supported operating system (Windows, macOS, or Linux).
- Internet access.
- An Azure account with active credentials.
- Administrative privileges on the local machine (if required for installation).

## Steps

### Check for Existing Azure CLI Installation

- Open a terminal or command prompt.
- Run `az --version` to check if Azure CLI is already installed.
- If installed but outdated, proceed to update (Step 3). If not installed, continue to Step 2.

### Install the Latest Azure CLI

- **Windows**:
    - Download the installer from [Microsoft's Azure CLI page](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?pivots=winget).
    - Run the .msi installer and follow the prompts.
    - Alternatively, install via PowerShell:
        ```powershell
        Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
        ```

- **macOS**:
    - Use Homebrew (recommended):
        ```bash
        brew install azure-cli
        ```
    - Or download the installer from the [Azure CLI page](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-macos).

- **Linux (e.g., Ubuntu, Debian)**:
    - Run the installation script:
        ```bash
        curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
        ```

- Verify Installation:
    ```bash
    az --version
    ```

### Update Azure CLI (If Already Installed):

- **Windows**:
    - Update to the latest version:
        ```powershell
        az upgrade
        ```

- **macOS or Linux**:
    - Update to the latest version:
        ```bash
        az updgrade
        ```

## Verify PATH Configuration

- Ensure the Azure CLI path (typically `C:\Program Files\Microsoft SDKs\Azure\CLI2\wbin`) is in your system's PATH.
- Check PATH by running:
    ```powershell
    $Env:PATH
    ```
- If missing, add it manually:
    ```powershell
    $env:PATH += ";C:\Program Files\Microsoft SDKs\Azure\CLI2\wbin"
    ```
- To make it permanent, add it to your user or system environment variables via System Settings or with:
    ```powershell
    [Environment]::SetEnvironmentVariable("Path", $env:PATH + ";C:\Program Files\Microsoft SDKs\Azure\CLI2\wbin", "User")
    ```
