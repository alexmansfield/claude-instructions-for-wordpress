# WordPress API Credential Setup

This project uses macOS Keychain to store credentials securely. 
Credentials are never stored in plain text on disk.

---

## First Time Setup

### 1. Add Your Credentials to Keychain
Open Terminal and run the following command, replacing the values in quotes with your actual credentials:
```bash
security add-generic-password -a "your-username" -s "your-site-domain" -w "your-app-password"
```

- `-a` is your WordPress username
- `-s` is your site domain (e.g. `mysite.local` or `mysite.com`)
- `-w` is your WordPress application password

You can generate a WordPress application password in your WordPress dashboard under:
**Users → Profile → Application Passwords**

### 2. Create Your .env File
Create a `.env` file in the project folder with your site URL:
```bash
LOCAL_SITE_URL=https://yoursite.com
```

### 3. Configure the Load Secrets Script
Open `.load_secrets` and update the two variables at the top to match 
your Keychain entry and .env file:
```bash
KEYCHAIN_ENTRY="your-site-domain"   # must match the -s value from step 1
ENV_URL_KEY="LOCAL_SITE_URL"        # must match the key name in your .env file
```

### 4. Make the Script Executable
Run this once:
```bash
chmod +x .load_secrets
```

---

## Every Session

Before opening Claude Code, run this in your terminal from the project folder:
```bash
source .load_secrets
```

You should see:
```
✓ Secrets loaded
```

Then open Claude Code:
```bash
claude
```

---

## How It Works

- Credentials are stored encrypted in macOS Keychain
- At session start, `.load_secrets` pulls credentials from Keychain
- A temporary `~/.netrc` file is written so curl can authenticate silently
- The `~/.netrc` file is automatically deleted when the terminal closes
- Credentials never appear in plain text in any command Claude Code runs

---

## Adding a New Project

1. Add new credentials to Keychain with `security add-generic-password`
2. Copy `.load_secrets` into the new project folder
3. Update the two variables at the top of `.load_secrets`
4. Create a `.env` file with the site URL
5. Run `chmod +x .load_secrets` once

---

## Troubleshooting

**`✓ Secrets loaded` appears but ~/.netrc is empty**
The Keychain entry name in `.load_secrets` doesn't match what was used 
in the `security add-generic-password` command. Check that `KEYCHAIN_ENTRY` 
matches the `-s` value exactly.

**`security: SecKeychainSearchCopyNext: The specified item could not be found`**
The Keychain entry doesn't exist or the name doesn't match. Re-run the 
`security add-generic-password` command from Step 1.

**Credentials appearing in curl commands**
Make sure Claude Code's `CLAUDE.md` file is present in the project folder 
and contains the security rules. See `CLAUDE.md` for details.
