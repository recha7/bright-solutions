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

## Logos
`media/logo-light.png` shows in light mode, `media/logo-dark.png` in dark mode. Replace the files, keeping the same names.

## Publish for free
- Netlify Drop: go to app.netlify.com/drop and drag the whole folder in. You get a web address right away.
- GitHub Pages: create a repository, upload the folder contents, then turn on Pages in Settings.
After publishing, add a preview image for WhatsApp sharing: put `<meta property="og:image" content="https://YOUR-ADDRESS/media/logo-light.png">` next to the other `og:` lines.

## Good to remember
- Make a copy of `index.html` before big edits.
- Policies are starter text. Adjust them to how you really work.
- Only publish photos and videos you have permission to show.
