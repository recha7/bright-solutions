# Bright Solutions website: editing guide

## What is in this folder
- `index.html`  the whole website (text, layout, colours, effects). Open it in any text editor (Notepad, VS Code).
- `media/`      all pictures and videos the page uses. Keep this folder next to `index.html`.

To preview: double-click `index.html`. To publish: upload the whole folder (not only index.html).

## Where to change things (use Search / Ctrl+F in your editor)
| To change                 | Search for                          |
|---------------------------|-------------------------------------|
| WhatsApp number (buttons) | `254796419377` (format: country code, no + or spaces) |
| Number shown on the page  | `0796 419 377`                      |
| WhatsApp group link       | `chat.whatsapp.com`                 |
| Catalogue link            | `wa.me/c/`                          |
| Till / Paybill details    | `Pay by Till`                       |
| Location and hours        | `Location:` and `Hours:`            |
| Services text             | `What we do`                        |
| Policies                  | `Privacy policy`                    |
| Page title and sharing text | `<title>` and `og:description`    |
| Feedback database         | `FB_URL` and `FB_KEY`               |
| Colours                   | the `:root{` line at the top of the `<style>`: `--green`, `--red` |

## Add a video
1. Make it small: portrait, about 360x640, 10 to 15 seconds, no sound. With ffmpeg:
   `ffmpeg -i input.mp4 -t 15 -an -vf scale=360:640 -c:v libx264 -crf 30 -pix_fmt yuv420p -movflags +faststart video-4-name.mp4`
2. Also save one frame as a picture (`video-4-name-poster.jpg`).
3. Put both files in `media/`.
4. In `index.html`, find `<div class="vids">`, copy one `<video ...></video>` line, and change its `src`, `poster` and `aria-label`.
Videos play only while on screen. Keep the page to about 3 to 6 clips so it loads fast on phones.

## Add a poster or design
In `index.html`, find `Posters and branding`, copy one `<figure>...</figure>` block, then change the image file name, the `alt` text and the caption. Put the picture in `media/`.

## Feedback section (posts by itself, with photos)
Visitors rate, write their name and location, add up to 3 photos as evidence, and tap "Post my feedback". Their post appears on the site straight away, stamped with the date and time (Nairobi time).
Until you connect a database, tapping "Post my feedback" opens WhatsApp with the feedback already written for the visitor (they can attach photos there). To make it post by itself, connect a free Supabase project (about 10 minutes):
1. Go to supabase.com, create a free account and a new project. Pick the region closest to Kenya and save the database password.
2. In the project, open SQL Editor, then New query. Open the file `supabase-setup.sql` (it is in the resources download), paste all of it, and press Run.
3. Open Project Settings, then API. Copy the Project URL and the public key (shown as "anon" or "publishable"). Never use the secret or service_role key.
4. In `index.html` search for `FB_URL`. Paste the URL and the key between the quotes. Save, and upload the site again.
5. Post a test feedback on your site, then delete that row in the dashboard.
You are the only one who can edit it. Visitors can only add new feedback; they cannot change or delete anything. In Supabase, open Table Editor, then `feedback`:
- Tick `hidden` to unpublish a post. Untick it to show it again.
- Type in `reply` to answer a post. It shows under the post as "Reply from Bright Solutions".
- Edit the text, or delete the row. Photos are in Storage, then `feedback-photos`.
Good to know:
- Posts go live immediately, so check the table often and hide anything inappropriate.
- The date and time are set by the server, so visitors cannot change them.
- Photos are shrunk before upload. The limit is 3 per post, images only.
- A free Supabase project pauses after 7 days with no database activity. Visitors loading the page count as activity. If it ever pauses, open the dashboard and press Restore.
- The free plan includes 500 MB of database space, 1 GB of photos and 5 GB of traffic a month.
- The Privacy policy on the site already mentions that feedback is shown publicly.

## Find a book (header button, hero book bar and Soft-copy books service)
- The glass search bar under the hero buttons, the quick picks under it (Fiction, Non-Fiction, Self-Help, Textbooks, Soft copy) and the "Soft-copy books" service card all open the same search panel. Visitors close it with the X, the Esc key, or by tapping outside it.
- In the panel visitors type a book, pick a bookshop, a genre and a format, then tap "Request book on WhatsApp". WhatsApp opens with this message already written (the Format line only appears if they chose one):
```
Hello! I am searching for a book on your website.
- Book / Keyword: ...
- Preferred Bookshop: ...
- Genre: ...
- Format: ...
```
To edit it, search `index.html` for `BK_PHONE`. The settings just below it:
- `BK_PHONE`    the WhatsApp number (country code, no + or spaces).
- `BK_SHOPS`    your bookshops, for example `["Shop One","Shop Two"]`. Visitors can also choose "Any bookshop" or type another name.
- `BK_FORMATS`  the format choices (soft copy, hard copy).
- `BK_GENRES`   the genres and the titles shown under each one. Each line is `["Title","Author or note"]`.
The titles that come with the site are popular examples, not your stock. Replace them with books you can really supply, then change the sentence "Titles shown are popular requests, not a stock list" (search for `not a stock list`) if you want to say "available now".
The reply-time promise ("within 1 hour") is in the line that starts `Direct Search`. Change it if you cannot keep it.
Only offer soft copies you have the right to share: your own, public-domain, open-access or properly licensed books.
To change the Soft-copy books card, search for `Soft-copy books`.

## Template 2: guidance, showcase and library
- "What brings you here?" buttons sit under the hero. Each scrolls to its section and changes the floating WhatsApp message to match. Links like `.../bright-solutions/#graphics` open the site on one service (`#graphics`, `#data`, `#printing`, `#documents`, `#video`, `#library`).
- "Our work" has filter chips and a full-screen preview. To add a piece, copy a `<figure>` block as before. Names starting `poster-`, `banner-` or `logo-` sort into the right filter.
- The Library searches 221 titles. Visitors can type a title, author or topic, or open the "Browse the library" dropdown, which shows the count and the genre filters. Titles not found are searched live on Open Library, and every request is logged.
- To add books, edit the block of lines starting `Pride and Prejudice|Jane Austen` in `index.html`. Each line is `Title|Author|Genre|tags` with an optional `|pd` for public-domain books (F fiction, N non-fiction, S self-help, T textbooks). The page and the admin page count them automatically.
- Mark a book Available or Reserved in the owner admin page (below). Everything else shows "On request".
- Soft copies must be public-domain, openly licensed, or legitimately purchased or licensed. Add only titles you can supply.

## Owner admin page (admin.html)
Address: https://recha7.github.io/bright-solutions/admin.html. It is not linked from the site and is hidden from search engines. Only your email can use it.
Set it up once:
1. In Supabase, open Authentication, then Users, and click Add user. Create a user with your email (use the invite option, or set a password you will not use).
2. Open Authentication, then Providers (or Sign In / Providers), then Email. Turn OFF "Allow new users to sign up".
3. Open Authentication, then URL Configuration. Set Site URL to https://recha7.github.io/bright-solutions/ and add https://recha7.github.io/bright-solutions/admin.html under Redirect URLs.
4. Open `supabase-admin.sql`, replace YOUR_EMAIL_HERE@example.com with your email, then run it in the SQL Editor (after the two earlier setup files).
5. Open the admin page, enter your email, tap Send sign-in link, and open the link on the same device.
In the admin page you can read all feedback, hide or show a post, write a public reply, delete a post, see the most requested books, and set books to Available or Reserved.
Supabase's built-in email sends only a few emails per hour. If a link does not arrive, wait and try again, or set up your own email sender in Supabase later.

## Website assistant (Cloudflare Workers AI)
The "Ask our assistant" section stays hidden until you connect it, so visitors never see a broken box.
1. Create a free account at cloudflare.com and open Workers and Pages.
2. Create a Worker (start with the Hello World template) and deploy it. Click Edit code, replace everything with the contents of `cloudflare-worker.js`, then Deploy.
3. In the Worker's Settings, open Bindings, Add, choose Workers AI, name the variable AI, and save and deploy.
4. Copy the Worker address (it ends in workers.dev).
5. In `index.html`, search for `AI_URL` and paste the address between the quotes. Save and upload the site again.
The Worker only accepts requests from https://recha7.github.io. The free plan has a daily allowance. If Cloudflare retires the AI model named in the file, choose a current text model in Workers AI, Models and change the `MODEL` line.

## Logos
`media/logo-light.png` shows in light mode, `media/logo-dark.png` in dark mode. Replace the files, keeping the same names.

## Publish on GitHub Pages (your live address)
Your site is at https://recha7.github.io/bright-solutions/ (GitHub account recha7, repository bright-solutions, kept Public).
To change the site with GitHub Desktop:
1. Copy the new or changed files into your `bright-solutions` folder (Repository, then Show in Explorer). A file with the same name replaces the old one.
2. In GitHub Desktop, open the Changes tab, type a short summary, and click Commit to main.
3. Click Push origin. The live site updates in about a minute. Press Ctrl+F5 to see it.
Keep `sitemap.xml` and any `google....html` verification file in the folder. They help Google find and trust your site.
Never put your Supabase database password or secret key into this folder. The public key already in `index.html` is safe.

## Good to remember
- To preview, use `bright-solutions-single-file.html`. `index.html` shows pictures and videos only when the `media` folder is beside it.
- Make a copy of `index.html` before big edits.
- Policies are starter text. Adjust them to how you really work.
- Only publish photos and videos you have permission to show.
