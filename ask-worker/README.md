# Ask the bench — putting the AI assistant online

Your website has an **Ask the bench** button in the bottom-right corner. It talks to
`worker.js`, a small program that runs on Cloudflare and uses Cloudflare's free AI models.
There is **no API key** to manage, and on the free plan it **cannot bill you**.

Your website itself is hosted on **GitHub Pages** (`index.html` in this repository).
You'll do three things in the Cloudflare website (no installs):

1. Put the assistant (`worker.js`) online.
2. Tell your website where the assistant lives.
3. Lock the assistant to your website.

Cloudflare sometimes renames buttons. If a label below doesn't match exactly, look for the
closest one.

---

## 1. Put the assistant online (about 5 minutes)

1. Create a free account at **https://dash.cloudflare.com/sign-up** and verify your email.
   Stay on the **Free** plan, and don't add a payment method.
2. In the left menu open **Workers & Pages** (under *Compute*), then click **Create** and choose **Create Worker**
   (or *Start with Hello World*).
3. Name it `trimdiam-ask` and click **Deploy**.
4. Click **Edit code**. Delete everything in the editor, then open `worker.js` from this folder,
   copy all of it, and paste it in. Click **Deploy**.
5. Go back to the Worker's page and open **Settings → Bindings → Add binding → Workers AI**.
   Set the variable name to exactly **`AI`** and save. (It redeploys by itself.)
6. Copy the Worker's address shown on its page. It looks like
   `https://trimdiam-ask.YOUR-NAME.workers.dev`.
7. Open that address in your browser. You should see:
   `{"status":"The bench is on. POST a question to this address."}`

## 2. Connect your website to it

1. Open `index.html` (in the folder above this one) in a text editor (Notepad works).
2. Search for `const ASK={url:''}` near the bottom.
3. Paste your address between the quotes, so it reads, for example:
   `const ASK={url:'https://trimdiam-ask.YOUR-NAME.workers.dev'};`
4. Save, then publish the change to GitHub (commit and push). GitHub Pages updates
   the live site within a minute or two.

## 3. Lock the assistant to your site (recommended)

So other websites can't use your free allowance:

1. Open the `trimdiam-ask` Worker → **Settings → Variables and Secrets → Add**.
2. Type **Text**, name **`ALLOWED_ORIGINS`**, value your site's *origin*, which is just the
   domain part: `https://YOUR-GITHUB-NAME.github.io` (no `/trimdiam`, no slash at the end).
   Add more than one with commas.
3. Save and deploy.

Note: this stops other *websites* from using it in a browser. It can't stop someone
scripting requests directly, which is why there is also a per-visitor limit
(8 questions a minute) and the daily free cap.

---

## Costs and limits

- Workers AI Free gives **10,000 neurons a day**, reset at **00:00 UTC** (5:30 am in India).
- With this setup, one question uses roughly 20–25 neurons, so about **400+ questions a day**.
- On the **Free** plan, when the allowance runs out the assistant replies
  *"The bench is resting right now"* until the reset. **Nothing is charged.**
- If you ever upgrade to Workers Paid, extra usage is billed. Stay on Free if you want a hard
  zero-cost guarantee.

## Changing what it knows

Everything the assistant may say is in the `FACTS` block at the top of `worker.js`.
Edit it when your work changes, then paste the file into the Worker editor again and deploy.
It is told to say "email Trimdiam" for anything not in `FACTS`, and never to invent prices,
timelines or experience.

## Changing the model

The default is `@cf/meta/llama-3.1-8b-instruct-fp8`. To try another, add a variable named
`MODEL` with a model id from **https://developers.cloudflare.com/workers-ai/models/**.
If Cloudflare ever retires the default model, do this and the assistant keeps working.

## If something goes wrong

| What you see in the chat | What to do |
|---|---|
| "The assistant isn't connected yet" | Step 2: the address in `index.html` is empty. |
| "The bench is offline right now" | The address is wrong, or you're offline. Open the address in your browser to check. |
| "The Workers AI binding is missing" | Step 1.5: add the Workers AI binding named `AI`. |
| "This site is not allowed" | Step 3: set `ALLOWED_ORIGINS` to exactly `https://YOUR-GITHUB-NAME.github.io`. |
| "The bench is resting right now" | Daily free allowance used up. It returns after 00:00 UTC. |

## Optional: deploy from the command line instead

If you prefer Cloudflare's `wrangler` tool, this folder has a `wrangler.toml`.
Run `npx wrangler login` and then `npx wrangler deploy` inside this folder.
