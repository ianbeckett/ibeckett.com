# ibeckett.com — Setup Guide

## What you need
- A GitHub account
- Access to your GoDaddy account for ibeckett.com
- Git installed on your computer ([git-scm.com](https://git-scm.com))

---

## Part 1 — GitHub: Create repo and push site

### Step 1 — Create the repo

1. Go to [github.com/new](https://github.com/new)
2. Repository name: **`ibeckett.com`**
3. Set to **Public** (required for free GitHub Pages)
4. Do NOT check "Add README" or any other initializers
5. Click **Create repository**

### Step 2 — Push your site files

Open Terminal and run these commands from your project folder:

```bash
cd "C:\Users\ibeck\Claude\Projects\ibeckett.com"

git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/ibeckett.com.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

### Step 3 — Enable GitHub Pages

1. Go to your repo on GitHub → **Settings** → **Pages** (left sidebar)
2. Under **Source**, select **Deploy from a branch**
3. Branch: **main** / folder: **/ (root)**
4. Click **Save**

GitHub will show a URL like `https://YOUR_USERNAME.github.io/ibeckett.com` — ignore this, your custom domain will override it.

---

## Part 2 — GoDaddy: Point your domain to GitHub Pages

GitHub Pages uses these four IP addresses. You'll add them as **A records** in GoDaddy.

GitHub Pages IPs:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

### Step 1 — Open DNS settings in GoDaddy

1. Log in to [godaddy.com](https://godaddy.com)
2. Go to **My Products** → find **ibeckett.com** → click **DNS**

### Step 2 — Delete any existing A records for `@`

Look for existing **A** records with name **@** and delete them (there may be GoDaddy placeholder records).

### Step 3 — Add the four GitHub A records

Click **Add New Record** four times, once for each IP:

| Type | Name | Value              | TTL  |
|------|------|--------------------|------|
| A    | @    | 185.199.108.153    | 1 hr |
| A    | @    | 185.199.109.153    | 1 hr |
| A    | @    | 185.199.110.153    | 1 hr |
| A    | @    | 185.199.111.153    | 1 hr |

### Step 4 — Add a CNAME record for `www`

| Type  | Name | Value                        | TTL  |
|-------|------|------------------------------|------|
| CNAME | www  | YOUR_USERNAME.github.io      | 1 hr |

Replace `YOUR_USERNAME` with your GitHub username.

### Step 5 — Set custom domain in GitHub Pages

1. Go back to GitHub → your repo → **Settings** → **Pages**
2. Under **Custom domain**, type `ibeckett.com`
3. Click **Save**
4. Check **Enforce HTTPS** (may take a few minutes to appear)

---

## Part 3 — Wait and verify

DNS changes take 5–30 minutes (sometimes up to a few hours).

To check if it's working:
```bash
dig ibeckett.com +noall +answer
```

You should see the four GitHub IPs in the response.

Once DNS resolves, visiting `ibeckett.com` and `www.ibeckett.com` should both load your site over HTTPS.

---

## Updating the site later

Any time you want to update the site, edit files in your project folder and run:

```bash
git add .
git commit -m "describe your change"
git push
```

GitHub Pages will redeploy automatically within ~1 minute.

---

## File reference

| File         | Purpose                                      |
|--------------|----------------------------------------------|
| `index.html` | Main page — edit this to update your site    |
| `CNAME`      | Tells GitHub Pages your custom domain        |
