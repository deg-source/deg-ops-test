# Stock & costing: put it online with GitHub and Supabase

When you finish, you'll have:

- a web address your whole team can open on phones, tablets and computers
- a sign-in for each person, so every entry shows who made it
- live sync: a sale on the counter tablet appears on the office computer right away
- offline use: once installed and signed in, the app opens and records sales, deliveries and counts without internet, then syncs when the connection returns

It takes about 30 minutes. Supabase and GitHub both change their menus from time to time, so a label may read slightly differently than below.

---

## Step 1. Set up the database in Supabase

1. Go to **supabase.com**, sign in, and open your project. If you don't have one yet, click **New project**. Pick **Southeast Asia (Singapore)** as the region, since it's closest to the Philippines. Save the database password somewhere safe.
2. In the left menu, open **SQL Editor**, then click **New query**.
3. Open the file **supabase-setup.sql** from this folder, copy everything in it, paste it into the editor, and click **Run**. You should see "Success". It's safe to run again if you're unsure.
4. Copy two values; you'll need them in Step 2:
   - **Project URL**, which looks like `https://abcdefghijklmnop.supabase.co`. It's on the project home page under **Connect**, or in **Project Settings → Data API**.
   - The **anon public** key, called the **publishable** key on newer projects. It's in **Project Settings → API Keys**.

Never copy the **service_role** or **secret** key. That key bypasses all security and must not go into the app.

## Step 2. Put the app on GitHub

1. On **github.com**, click **New repository**. Name it, for example, `stock-costing`, choose **Public**, and click **Create repository**.
   GitHub's free plan only publishes websites from public repositories. Only the app's code is public. Your business data stays in Supabase behind sign-in.
2. On the new repository's page, click **uploading an existing file**. Drag in **everything** from this folder: `index.html`, `config.js`, `sw.js`, `manifest.webmanifest`, `supabase-setup.sql`, `README.md`, and the `icons` and `vendor` folders. Click **Commit changes**.
   Dragging folders works in Chrome and Edge. If a folder won't drag, open it and drag its files after creating the same folder name.
3. Click **config.js**, then the pencil icon to edit it. Replace the two placeholder values with your Project URL and anon/publishable key from Step 1, keeping the quote marks. Click **Commit changes**.

## Step 3. Turn on the website (GitHub Pages)

1. In the repository, open **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**, choose **main** and **/ (root)**, then click **Save**.
3. Wait a minute or two and refresh. The address appears at the top, for example `https://yourname.github.io/stock-costing/`. That's the address you'll share with your team.

## Step 4. Connect the address to Supabase and lock sign-ups

1. In Supabase, open **Authentication → URL Configuration**. Set **Site URL** to your GitHub Pages address, and add the same address under **Redirect URLs**. This makes password-reset links come back to your app.
2. Open **Authentication → Sign In / Providers**. Keep **Email** turned on, and turn **off** "Allow new users to sign up". This way only people you add can get in.

## Step 5. Create accounts for yourself and your staff

1. In Supabase, open **Authentication → Users → Add user → Create new user**.
2. Enter the email and a password, tick **Auto Confirm User**, and click **Create user**.
3. **Create your own account first**, because the first account becomes the **Owner**. Then create one for each staff member and give them their email and password. They can change their password in the app under **Setup → Your account**.
4. To make someone **view-only**, for example an accountant: open **Table Editor → profiles**, find their row, and change **role** to `viewer`. The options are `owner`, `staff` and `viewer`.

## Step 6. Move your current data across

Do this when you're ready to switch, so nothing gets left behind.

1. In the current version of the app on Claude, go to **Setup → Download backup**. The file includes materials, recipes, stock movements, purchase orders, sales, customers and scheduled orders.
2. Open your new GitHub Pages address and sign in with the owner account.
3. Go to **Setup → Restore from backup**, choose the file, and confirm.
4. From now on, use only the new address. The Claude version and the new one don't sync with each other.

Entries made in the Claude version show the person as "Someone", because the sign-in accounts are new.

## Step 7. Install it on phones and tablets

- **Android (Chrome):** open the address, tap the ⋮ menu, then **Install app** or **Add to Home screen**.
- **iPhone or iPad (Safari):** open the address, tap **Share**, then **Add to Home Screen**.

Sign in once while connected. After that, the app opens from the home screen even without internet.



## Units, SKUs and exact quantities

- **Exact quantities:** quantities are stored to 6 decimal places and shown to 3, like 5.780 kg, so totals never drift. This applies to receiving, issuing, counting, waste and the Sell screen.
- **Other units:** when receiving, issuing or logging waste, pick another unit next to the quantity. Grams and kilos (and millilitres and litres) convert automatically. Stock is always kept in the item's stock unit, and the movement history shows what was typed, like "Entered as 2 sack".
- **Purchase units:** an item can also be bought in a purchase unit, like a sack of 25 kg. Set this in **Also buys in** when editing the item.
- **Item settings:** each item can have a **SKU**, a **purchase tax class**, whether it **can be used as a recipe ingredient**, and whether it's **sold directly**.
- **Selling steps:** items sold directly have a selling step, like 0.250 kg. One tap adds one step, and cashiers can also type any amount in the order.
- **Movement history:** **Movements** shows stock sold through the Sell screen alongside deliveries, issues, batches and waste, each linked to its order.


## Cooked food: batches and made-to-order

- **Batch-cooked dishes:** **Record batch** takes the ingredients out of stock once and adds the prepared servings. Each batch gets a number, like B1023-BD-01. Sales and dish waste use the oldest batch first, and the dish's recipe page lists what's left in each batch. Selling a prepared serving never takes ingredients out again.
- **Correcting a batch:** if a batch made more or fewer servings than recorded, use **Correct yield** on the recipe page or in **Movements**. It adds a linked correction and keeps the original record.
- **Made-to-order dishes:** these take their ingredients out at checkout. The Sell screen shows how many can be made from the ingredients in stock ("Can make 12"), or which ingredient has run out. The cart checks all its lines together, so a dish and fresh produce sold directly can't both count on the same stock.
- **Voiding an order:** choose what happened. If nothing was prepared or handed over, everything goes back into stock. If food was already cooked or can't be resold, it's recorded as waste linked to the order.


## One checkout for cooked food and products

- **Two tabs:** the Sell screen has **Cooked foods** (dishes from your recipes) and **Products** (items you sell directly, like fresh produce, meat, drinks and dry goods), plus **Everything**. Switching tabs never clears the order, so one customer gets one order, one number, one total and one slip.
- **Split payment:** choose **Split payment** to take an order in more than one way, like part cash and part GCash. **Rest** fills in what's left. Change is worked out on the cash part only.
- **Returns and refunds:** open an order and use **Return items**. Enter how much of each line is coming back (part of a line is fine), and whether it goes **back to stock** or is **written off as waste**. A refund is recorded as a payment on the order. On an unpaid order, the amount owed goes down instead. Reports count the order after returns.


## Tax and stock rules

Set these under **Setup → Tax and stock rules**. They apply to everyone.

- **VAT registration:** **VAT-registered** (prices include VAT) or **Non-VAT registered**. Non-VAT sellers charge no VAT, so suggested prices drop by the VAT and slips show "Non-VAT sales" and "Non-VAT registered seller".
- **VAT rate:** there's no built-in rate. Type your own under **VAT rate** (also editable under **Pricing**). Until a rate is entered, VATable items can't be checked out, though VAT-exempt and zero-rated items can. Each sale keeps the rate it was sold at.
- **Sales tax classes:** every dish and every product sold directly has its own class: VATable, VAT-exempt or zero-rated. It isn't copied from its category or ingredients. Items nobody has reviewed yet are flagged "Tax class not reviewed". Use **Review tax classes** to set them all in one place.
- **Unreviewed items:** either **sell as VATable, with a warning**, or **don't sell until reviewed**.
- **Mixed orders:** VAT is worked out line by line. Slips mark each line **V** (VATable), **E** (VAT-exempt) or **Z** (zero-rated) and show the VATable sales, VAT, VAT-exempt sales and zero-rated sales totals.
- **When stock runs short at checkout:**
  - **Warn, but allow the sale.**
  - **Block, but an owner can allow it.** The owner's approval is saved on the order and noted on the slip.
  - **Always block.**
- **Stock labels:** product tiles on the Sell screen show **In stock**, **Low stock** (at or below the item's minimum) or **Out of stock**, with the quantity.

This app prints order slips, not BIR-registered invoices. Check your VAT setup and slip wording with your accountant.


## At a glance: summary cards

- **Stock, Finished goods, Movements and Recipes:** each opens with summary cards.
- **Cards as filters:** click a card to filter the list below it, for example **Received this month** on Movements or **High food cost** on Recipes. Click it again to clear the filter.
- **Long lists:** these have pages, with **Rows per page** at the bottom.

## Printing order slips

- On each device, go to **Setup → Order slips and printing**. Choose the paper size there, and decide whether to print automatically after each sale: the order slip, a kitchen ticket, or both. You can also set 1 to 3 copies (customer, store and delivery).
- **Print** opens your device's print dialog; pick your printer there. Browsers always show this dialog first, for security.
- **Thermal printers on a computer:** install the printer's driver and make it the default printer. In the print dialog, choose the 80 mm or 58 mm paper and set margins to **None**. Each slip is sized to its content, so no blank paper is fed.
- **Skipping the dialog on a counter computer:** start Chrome with the `--kiosk-printing` option, so slips go straight to the default printer.
- **Android:** Wi-Fi printers usually work directly. For Bluetooth thermal printers, install the printer's print service app from the Play Store, then choose it in the print dialog.
- **iPhone and iPad:** AirPrint printers work directly. Most Bluetooth thermal printers don't support AirPrint; use **PDF** and print from the printer's own app instead.

---

## Good to know

**Working offline.** Everything you record offline is kept on that device and sent automatically when the internet returns; the sync indicator shows how many changes are waiting. Don't sign out while changes are waiting, because signing out clears the device's copy. The app warns you if you try.

**Printing and exports.** PDF slips, forms and reports, plus Excel exports, all work offline, because their libraries are included in the `vendor` folder.

**Backups.** Supabase's free plan doesn't include restorable daily backups. Download a backup from **Setup** every week or so and keep it somewhere safe, such as Google Drive.

**Free plan limits.**
- A Supabase project on the free plan pauses after about a week with no activity. Daily use keeps it awake; if it does pause, click **Restore project** in the dashboard.
- The free database holds 500 MB, which is years of sales for a small business.

**Forgotten passwords.** Supabase's built-in email sending is very limited, so reset emails may not arrive. For reliable reset emails, set up your own email sender in **Authentication → Emails → SMTP settings**; a Gmail app password or a free Brevo account works. Otherwise, delete the user and create them again with a new password. Their past entries will then show as "Someone".

**Updating the app later.** Upload the new `index.html` (and any other changed files) to the repository, replacing the old ones. Devices pick up the new version the next time they open the app online; sometimes it takes closing and reopening twice.

## If something isn't working

- **"Connect your database" screen:** `config.js` still has the placeholders, or a value was pasted with a typo or without quote marks.
- **"That email and password don't match":** the user wasn't created, wasn't auto-confirmed, or the password is different. Check **Authentication → Users**.
- **Changes are refused as "view-only" for everyone:** the setup script didn't finish, so there are no rows in **profiles**. Run `supabase-setup.sql` again. People created before the script ran are added when it runs.
- **Other people's changes only appear after reopening:** live updates aren't on. Run the setup script again, or check **Database → Publications** that the `docs` table is included in `supabase_realtime`.
- **The address shows "404":** GitHub Pages is still publishing (wait two minutes), or `index.html` isn't at the top level of the repository.
