# Publishing Panel Forge so other people can install it

Panel Forge is a **Progressive Web App (PWA)**: a website that installs like a normal app, with its own window, a Start-menu or Dock icon, and offline start-up. You publish it once, and from then on people can get it in three ways:

| Where people install it | Who it reaches | Cost | Needed |
|---|---|---|---|
| **Straight from the website** (Chrome / Edge "Install" button) | Windows, Mac, Linux, Chromebook | Free | Step 1 only |
| **Microsoft Store** | Windows PCs and laptops | Free individual developer account | Steps 1 + 2 |
| **Google Play** | Chromebooks and Android | US$25 one-time | Steps 1 + 3 |

> **Google Play and Windows PCs:** Google Play does not install apps on normal Windows or Mac computers. On a PC, people install from the website or from the Microsoft Store. Google Play covers Chromebooks and Android phones and tablets.
>
> **Chrome Web Store:** it no longer accepts apps, only browser extensions, so it isn't an option here.

---

## Step 1: Put the app online (free, about 5 minutes)

The repo already has a workflow (`.github/workflows/deploy-app.yml`) that publishes the `app/` folder with GitHub Pages.

1. Merge the `claude/session-options-m08pfw` branch into `main` (open a pull request and merge it).
2. On GitHub, open **Settings → Pages** for this repository.
3. Under **Build and deployment → Source**, pick **GitHub Actions**.
4. Open the **Actions** tab. Run **Deploy Panel Forge** (or push any change to `app/`).
5. When it turns green, the app is live at:
   **https://jiradej925.github.io/jiradej925/**

Test it: open the link in Chrome or Edge. An **Install** icon appears at the right of the address bar, and an **Install app** button appears in Panel Forge's top bar. Click it, and Panel Forge opens in its own window with a Start-menu icon.

**You can share that link today.** Anyone can install from it.

### Updating the app later
Edit files in `app/`, then open `app/sw.js` and change `VERSION = 'pf-v1'` to `'pf-v2'` (and so on). Push to `main`. Installed copies update the next time they open online.

---

## Step 2: Microsoft Store (Windows PCs and laptops)

1. Create a free developer account at **https://storedeveloper.microsoft.com** (individual developers pay no fee).
2. In **Partner Center**, create a new app and **reserve the name** "Panel Forge" (or another free name). Note the **Package ID**, **Publisher ID** and **Publisher display name**.
3. Go to **https://www.pwabuilder.com**, paste `https://jiradej925.github.io/jiradej925/` and press **Start**.
4. PWABuilder checks the app. The manifest, service worker and icons are already in place.
5. Press **Package for stores → Windows**, enter the three values from step 2, and download the package.
6. In Partner Center, create a submission and upload the `.msixbundle`. Fill in:
   - **Privacy policy URL:** `https://jiradej925.github.io/jiradej925/privacy.html`
   - **Screenshots:** use `app/icons/screenshot-wide.png`, or take your own after making a page.
   - **Category:** Photo & video, or Productivity.
7. Submit. Review usually takes 1 to 3 business days.

---

## Step 3: Google Play (Chromebooks and Android)

1. Create a Google Play Console account at **https://play.google.com/console** (US$25 one-time; identity checks can take a few days).
2. On **https://www.pwabuilder.com**, enter the app URL and choose **Package for stores → Android**.
   - Package ID: for example `io.github.jiradej925.panelforge`.
   - Keep the default signing key option and **save the signing key file and passwords safely**. You need them for every future update.
3. Download the zip. It contains an `.aab` file and an `assetlinks.json` file.
4. **Remove the browser address bar (recommended).** Google checks that you own the website through `assetlinks.json`, which must live at the **root** of the domain: `https://jiradej925.github.io/.well-known/assetlinks.json`. The root of `jiradej925.github.io` is served by a separate repository named exactly **`jiradej925.github.io`**:
   1. Create a public repository named `jiradej925.github.io`.
   2. Add the file `.well-known/assetlinks.json` from the PWABuilder zip, plus an empty file named `.nojekyll` (GitHub Pages hides folders starting with `.` without it).
   3. Enable Pages for that repository (deploy from the `main` branch).
   Without this step the app still works, but shows a small address bar at the top.
5. In Play Console: **Create app → Production (or Internal testing first) → Create release**, and upload the `.aab`.
6. Complete the store listing: description, the 512×512 icon (`app/icons/icon-512.png`), screenshots, **privacy policy URL** (`https://jiradej925.github.io/jiradej925/privacy.html`), content rating questionnaire, and the data safety form. For data safety: the app collects no data itself; pictures and prompts go to Google Gemini only when the user asks.
7. New personal developer accounts must run a **closed test with at least 12 testers for 14 days** before going to production. Invite friends through the **Closed testing** track first.

---

## What users need

- A **free Gemini API key** from https://aistudio.google.com/apikey. The app asks for it on first use (**Settings → Test key**). Each user's key and usage are their own; nothing runs on your servers.
- A current Chrome, Edge, or other Chromium browser. Safari on Mac can use the site too (**File → Add to Dock**).

## Before you submit: checklist

- [ ] The live site opens and the **Install** button works in Chrome and Edge.
- [ ] You tested one full panel with your own Gemini key: Upload → Pose → Generate → Letter → Export.
- [ ] The privacy policy link opens.
- [ ] You changed the contact line in `app/privacy.html` if you want a different address than GitHub Issues.
- [ ] The store description says users need their own Gemini API key.
