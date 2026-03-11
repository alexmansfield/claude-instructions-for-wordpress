# Streaming-Safe API Key Setup

When given this file, perform the following setup tasks for this project folder.

---

## What You're Doing & Why

This project is used during livestreams. API keys must never appear on screen.
The goal is to keep keys in a `.env` file (sandboxed to this folder) while
ensuring you always reference variable *names* — never raw key values — in
any commands you write or suggest.

---

## Setup Steps

### 1. Create `.env` if it doesn't exist

If there is no `.env` file in this directory, create one:

```
# API Keys — never share or display these values
SERVICE_API_KEY=your-key-here
```

If `.env` already exists, leave its contents untouched.

---

### 2. Create `.claudeignore` if it doesn't exist

Create a `.claudeignore` file in this directory with the following contents:

```
.env
.env.local
.env.*.local
```

If `.claudeignore` already exists, check that these three lines are present
and add any that are missing.

---

### 3. Create `.gitignore` entry

Ensure `.gitignore` exists and contains `.env`. If `.gitignore` doesn't exist,
create it. If it exists but is missing `.env`, append it.

---

### 4. Add Ongoing Behaviour Rules to `CLAUDE.md`

Create or update `CLAUDE.md` in this directory so the rules below persist
across all future sessions. If `CLAUDE.md` already exists, append the rules
under a new `## API Key Safety` heading — do not overwrite existing content.

Add the following rules to `CLAUDE.md`:

---

### 5. Confirm setup is complete

After completing the above, report back with:
- Whether `.env` was created or already existed
- Whether `.claudeignore` was created or already existed
- Whether `.gitignore` was updated or already correct
- Whether `CLAUDE.md` was created or updated with the API key safety rules
- A reminder that you will always use variable names (e.g. `$SERVICE_API_KEY`)
  in curl commands and scripts, and will never inline key values

---

## Ongoing Behaviour Rules

Follow these rules in every session:

- **Never read or display the contents of `.env`**
- **Always use the variable name** (e.g. `$SERVICE_API_KEY`) in any curl
  commands, scripts, or code you write — never substitute the actual value
- If you need to reference an API key in a command, write it like this:

```bash
curl https://api.example.com/endpoint \
  -H "Authorization: Bearer $SERVICE_API_KEY" \
  -d '{"message": "hello"}'
```

- If asked to add a new key, instruct the user to add it to `.env` manually,
  then tell you the **variable name only** so you can reference it going forward
