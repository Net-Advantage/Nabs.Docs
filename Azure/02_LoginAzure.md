# Login to Azure

> Assume we are using Windows and Powershell.

- Authenticate using your Azure Account.
    ```powershell
    az login
    ```
- A browser window will open for you to sign in. Follow the prompts.
- Verify the active account:
    ```powershell
    az account show
    ```
- If you need to switch subscription:
    - List all the account:
        ```powershell
        az account list --output table
        ```
    - Switch to the account:
        ```powershell
        az account set -- subscription
        ```

