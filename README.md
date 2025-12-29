# Using Bitwarden CLI as a Git Password Manager

How to use the `git-credential-bw` tool so that Git automatically retrieves your credentials from Bitwarden. This avoids manually entering your password for every Git operation requiring authentication.

## Prerequisites

Before starting, make sure you have:

### **Bitwarden CLI** installed and configured on your computer.

- Bitwarden CLI is a command-line tool to access your Bitwarden vault.
- To install and configure it, follow the official guide: [Bitwarden CLI Documentation](https://bitwarden.com/help/cli/)

### **Git** installed on your system.
 
- To check, type in a terminal:

```shell
git --version
```
- If not installed, see [git](https://git-scm.com/install/)

## Installing git-credential-bw

### 1. **Download the script**

Open a terminal and run:

```shell
curl -o $HOME/.local/bin/git-credential-bw https://github.com/tsenay/git-credential-bw/raw/main/git-credential-bw
```

- This command downloads the script into `$HOME/.local/bin`, which is usually included in your `PATH`. If the folder does not exist, create it:

```shell
mkdir -p $HOME/.local/bin
```

### 2. **Make the script executable**

```shell
chmod +x $HOME/.local/bin/git-credential-bw
```

### 3. **Ensure the folder is in PATH**

Check:

```shell
echo $PATH
```
If not, add it to your `~/.bashrc` or `~/.zshrc`:

```shell
export PATH="$HOME/.local/bin:$PATH"
```

Then reload:
```shell
source ~/.bashrc
```

## Configuration

### 1. Prepare your Bitwarden vault

- Log in to your Bitwarden vault (via app or website).
- Create a new **Login** entry for each Git server (e.g., `gitlab.com`, `github.com`).
    - **Item name**: Git server hostname (e.g., `gitlab.com`)
    - **Username**: your Git username
    - **Password**: your Git password or personal access token

### 2. Configure Git to use Bitwarden

Set credential helper for your git server 

```shell
git config --global credential."https://gitlab.com".helper bw
```

- Replace `gitlab.com` with your Git server name if needed.
- This tells Git to use the `git-credential-bw` script to retrieve credentials.

## Example Usage

1. When performing a Git operation requiring authentication (e.g., `git clone`, `git push`), Git will call the script, which fetches credentials from Bitwarden.
2. If you are not logged in via CLI, you will be prompted:

```shell
bw login
bw unlock
```

## Tips

- For security, lock your Bitwarden vault after use:

```shell
bw lock
```
- To list all vault entries:

```shell
bw list items
```
