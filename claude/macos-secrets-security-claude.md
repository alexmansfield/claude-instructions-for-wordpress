# Project Instructions

## First Time Setup
If this project has not been set up yet, follow the instructions in macos-secrets-security-setup.md first.

---

## Session Start
Before doing anything else, run:
`source .load_secrets`

If you see `✓ Secrets loaded` you are ready to proceed.
If you see any errors, stop and tell me before continuing.

---

## WordPress API Access
- Site URL is available as $WP_URL
- Authentication is handled automatically via ~/.netrc
- Always use --netrc in every curl command

## Curl Command Format
Always write curl commands like this:

curl --netrc $WP_URL/wp-json/wp/v2/posts

Never like this:

curl -u username:password $WP_URL/wp-json/wp/v2/posts

---

## Security Rules
- Never read, display, or print the contents of ~/.netrc or .env
- Never expand or substitute actual credential values in any command
- Never print or echo any environment variables
- If unsure whether something would expose a credential, ask me first
- If a new credential is needed, tell me and I will add it to Keychain manually

---

## Live Streaming
- Assume everything in the terminal may be visible to an audience
- Warn me before displaying any output that might contain sensitive data
