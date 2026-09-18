# Valkyria — website

One file, no build step, no dependencies. `index.html` is the whole site.

The "Join the waitlist" and "Book a demo" pop-up sends name, phone and email
to a Google Form in the background. The visitor never sees Google — they get
a "Thank you" panel on your own page. Every submission lands in a Google Sheet
and, once notifications are on, as an email to **biocanvasprivatelimited@gmail.com**.

**Until you finish Part 1, the pop-up falls back to opening a pre-filled Gmail
message instead, so nothing is lost while you set it up.**

---

## Part 1 — Build the Google Form (10 minutes)

Do this signed in as **biocanvasprivatelimited@gmail.com**, so the responses
land in the right account.

### 1. Create the form

Go to [forms.google.com](https://forms.google.com) → **Blank form**.
Name it something like `Valkyria — website enquiries`.

### 2. Add exactly these six questions, in this order

| # | Question title | Type |
|---|----------------|------|
| 1 | Name | Short answer |
| 2 | Phone | Short answer |
| 3 | Email | Short answer |
| 4 | Organisation | Short answer |
| 5 | Notes | Paragraph |
| 6 | Request type | Short answer |

**Leave every "Required" toggle OFF.** If any question is required, Google
rejects submissions that skip it — and Organisation and Notes are optional on
the site.

The titles are for your eyes only; you can word them however you like. The
*order* is what matters for step 4.

### 3. Turn off anything that forces a sign-in

Click the **⚙ Settings** tab at the top, then under **Responses** make sure all
of these are **off**:

- Collect email addresses
- Limit to 1 response
- Restrict to users in *(your organisation)* — only appears on Workspace accounts

A visitor to your site is not signed into your Google account, so any of these
being on will block their submission.

### 4. Get your `formId`

Click **Send** (top right) → the **🔗 link** icon → **Copy**.

You get something like:

```
https://docs.google.com/forms/d/e/1FAIpQLSc7Xk9QqW2mFbT4vN8pLzY6RdHgKjA3sEuC1oB5nD/viewform
```

Your **formId** is the long chunk between `/d/e/` and `/viewform`:

```
1FAIpQLSc7Xk9QqW2mFbT4vN8pLzY6RdHgKjA3sEuC1oB5nD
```

It always starts with `1FAIpQL`. Keep it somewhere handy.

### 5. Get your six `entry.` numbers

Back on the form, click the **⋮** menu (top right, next to Send) →
**Get pre-filled link**.

A copy of your form opens. Type a different dummy word into each box:

- Name → `AAA`
- Phone → `BBB`
- Email → `CCC`
- Organisation → `DDD`
- Notes → `EEE`
- Request type → `FFF`

Click **Get link** at the bottom, then **COPY LINK** in the little bar that
appears. Paste it into any notes app. It looks like this:

```
https://docs.google.com/forms/d/e/1FAIpQLSc.../viewform?usp=pp_url
&entry.1834729183=AAA
&entry.0937261845=BBB
&entry.1129384756=CCC
&entry.2018374659=DDD
&entry.1746382910=EEE
&entry.0918273645=FFF
```

Each `entry.NUMBER` is the ID for the box you typed that dummy word into. Match
them up by the dummy words, not by position in the URL.

### 6. Turn on email notifications

Open the **Responses** tab → **⋮** menu → tick
**Get email notifications for new responses**.

That is the bit that makes every submission arrive in your inbox.

While you're there, click **Link to Sheets** to create the spreadsheet of all
responses.

---

## Part 2 — Put your IDs into the site

Open `index.html`, search for `LEADS` (it's near the bottom, around line 1040,
in the `<script>` block). You'll find this:

```js
var LEADS = {
  formId : 'PASTE_FORM_ID_HERE',
  name   : 'entry.0000000001',
  phone  : 'entry.0000000002',
  email  : 'entry.0000000003',
  org    : 'entry.0000000004',
  notes  : 'entry.0000000005',
  type   : 'entry.0000000006'
};
```

Replace all seven values with your real ones. Using the example above it would
become:

```js
var LEADS = {
  formId : '1FAIpQLSc7Xk9QqW2mFbT4vN8pLzY6RdHgKjA3sEuC1oB5nD',
  name   : 'entry.1834729183',
  phone  : 'entry.0937261845',
  email  : 'entry.1129384756',
  org    : 'entry.2018374659',
  notes  : 'entry.1746382910',
  type   : 'entry.0918273645'
};
```

Keep the quotes and the commas exactly as they are. That is the only edit you
need to make anywhere in the file.

---

## Part 3 — Put it on GitHub Pages

### 1. Create the repository

github.com → **New repository** → name it `valkyria-site` → **Public** →
**Create repository**.

### 2. Upload the files

On the new repo page: **Add file → Upload files**. Drag in:

- `index.html`
- `.nojekyll` *(an empty file; it stops GitHub reprocessing your HTML)*
- `README.md` *(optional)*

Then **Commit changes**.

> If you can't see `.nojekyll` in your file picker, it's hidden because the name
> starts with a dot. On Mac press `Cmd + Shift + .` in the file dialog to reveal
> hidden files. Or skip it — this site works without it.

### 3. Switch Pages on

Repo **Settings** → **Pages** (left sidebar) → under **Build and deployment**:

- Source: **Deploy from a branch**
- Branch: **main**, folder: **/ (root)**
- **Save**

Wait a minute or two, refresh, and GitHub shows your live URL:

```
https://<your-username>.github.io/valkyria-site/
```

### 4. Test it properly

Open that live URL — **not** the file on your computer. Google blocks form posts
from `file://` pages, so testing locally will look broken even when everything is
right.

Press "Join the waitlist", fill it in with your own details, submit. You should
see the Thank you panel, and within a few seconds a new row in the Sheet and an
email in the inbox.

If you instead get a panel saying it couldn't submit automatically, jump to
Troubleshooting below.

---

## Part 4 — Point valkyria.health at it (optional)

Repo **Settings → Pages → Custom domain** → type `valkyria.health` → **Save**.

Then at whoever you bought the domain from (GoDaddy, Namecheap, BigRock…), in
the DNS settings, add:

**Four A records**, host `@`, pointing to:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**One CNAME record**, host `www`, pointing to `<your-username>.github.io`

DNS takes anywhere from ten minutes to a few hours. Once it resolves, go back to
Settings → Pages and tick **Enforce HTTPS**.

---

## Troubleshooting

**"We couldn't submit that automatically"**

Work through these in order — it's nearly always the first two:

1. A question in your Google Form is marked **Required**. Turn every one off.
2. **Collect email addresses** or **Limit to 1 response** is on in Settings →
   Responses. Turn both off.
3. One of the `entry.` numbers is wrong or has a typo. Redo step 5 and compare
   character by character.
4. You're testing the file from your computer rather than the live GitHub URL.
5. The form is closed — check "Accepting responses" is on.

Nothing is lost while you debug: that panel gives the visitor a Gmail and a
mailto button so they can send you the same details by hand.

**Rows appear in the Sheet but no email arrives**

Notifications aren't on. Responses tab → ⋮ → Get email notifications for new
responses. Check spam once, then mark as not spam.

**I changed index.html but the live site looks the same**

GitHub Pages caches for a minute or two, and your browser caches harder. Wait,
then hard-refresh: `Ctrl + Shift + R` on Windows, `Cmd + Shift + R` on Mac.

---

## What each button sends

All of these open the same pop-up; the "Request type" field tells them apart in
your Sheet:

| Button on the site | Request type recorded |
|---|---|
| Join the waitlist | `Waitlist` |
| Book a demo / Book a clinical demo / Request a demo | `Book a demo` |
| Enquire about a pilot | `Pilot programme` |
| Write to us about a clinical role | `Clinical roles` |

---

## Editing the site later

Everything lives in `index.html`. Useful search terms:

- `FOUNDERS' CONTACT` — the founders' contact section at the bottom
- `LEAD CAPTURE MODAL` — the pop-up form's HTML
- `KINDS` — the headings and button text for each type of request
- `data-page="/about"` — each page of the site is one `<main data-page="...">`
- `:root{` — all colours and fonts, at the top of the `<style>` block

To edit on GitHub directly: open `index.html` in the repo, click the pencil
icon, make the change, and commit. Pages redeploys on its own.
