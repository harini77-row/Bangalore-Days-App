# Bangalore Days 💖

One shared trip site for the four of you: packing, spending (with who owes who), shopping list, route, and a photo journal.

- **Packing**: ticks save on each person's own phone (so everyone has her own list).
- **Spending, Shopping, Route, Album**: shared live through Firebase.
- Photos are shrunk in the browser and stored in Firestore, so you stay on the **free Spark plan** (no Firebase Storage, no card needed).

## Files
| File | What it is |
|---|---|
| `index.html` | The whole site (no build step) |
| `firestore.rules` | Security rules to paste into Firebase |

---

## 1. Create the Firebase project
1. Go to https://console.firebase.google.com and click **Add project**. Name it `bangalore-days`. Turn Google Analytics off.
2. In the project, click the **`</>` (Web)** icon to add a web app. Nickname: `bangalore-days`. Skip Firebase Hosting.
3. Firebase shows a `firebaseConfig` block. Keep that page open.

## 2. Turn on the two services
1. **Build > Firestore Database > Create database**. Choose a location near you (`asia-south1` is Mumbai), start in **production mode**.
2. Open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**.
3. **Build > Authentication > Get started > Sign-in method > Anonymous > Enable > Save**.
   (Nobody has to make an account. The site quietly signs each phone in so the rules can tell it apart from the open internet.)

## 3. Put your details in `index.html`
Near the top of the `<script type="module">` block you'll find:

```js
const FIREBASE_CONFIG = { apiKey: "YOUR_API_KEY", ... };
const MEMBERS = ["Girl 1","Girl 2","Girl 3","Girl 4"];
```
- Paste your real `firebaseConfig` values over the placeholders.
- Replace the four names in `MEMBERS`. These power "Paid by", the split, the balances and the photo credits.

Until you do, the site runs in **demo mode** (saves on that device only), which is handy for a test drive.

> The Firebase web config is not a secret, it's fine to commit it. The rules in step 2 are what protect your data.

## 4. Test locally
Just double-click `index.html`, or run `npx serve` in this folder. Open it on two devices and add an expense on one, it should appear on the other instantly.

## 5. Push to GitHub
```bash
cd bangalore-days
git init
git add .
git commit -m "Bangalore Days"
git branch -M main
# create an empty repo on github.com first (e.g. bangalore-days), then:
git remote add origin https://github.com/<your-username>/bangalore-days.git
git push -u origin main
```

## 6. Host on Vercel
1. Go to https://vercel.com, sign in with GitHub, click **Add New > Project**, and import `bangalore-days`.
2. **Framework Preset: Other**. Leave Build Command and Output Directory empty. Click **Deploy**.
3. You get a link like `bangalore-days.vercel.app`. Send it to the group chat.
4. If sign-in ever fails: Firebase **Authentication > Settings > Authorized domains > Add domain**, and add your `.vercel.app` domain.

Every `git push` to `main` redeploys automatically. Changing the code later is: edit, `git add .`, `git commit`, `git push`.

## 7. First visit
Each person opens the link, taps her name on the "Who's this?" screen, and she's in. Use the **switch** link under the title if someone borrows a phone.

---

## How the money works
- Every expense has a **payer** and a **split** (everyone by default, untick anyone who wasn't part of it).
- **Who owes who** shows the fewest payments needed to square everyone up. When someone actually pays, tap **Mark paid**. It's recorded as a 🤝 entry and balances update for all four.
- **Everyone's spending** shows what each person paid vs her fair share.

## Good to know
- **Privacy**: anyone who has the link can open the site, so only share it with the group. For something tighter later, switch Anonymous sign-in for Google sign-in and limit the rules to your four emails.
- **Photos** are compressed to about 1000px (roughly 100 to 200 KB each). The free tier holds 1 GB, which is thousands of photos. Anyone can delete her own memories.
- **Map**: the Route tab uses Google Maps links and embeds. No Maps API key needed.
- To change the suggested stops, edit the `SEED` list in `index.html` (or just edit the stops in the app).
