# Mizan POS: install and run guide

For shop owners and the people who install Mizan for them.
Arabic words in parentheses are the exact labels you will see on screen.

**One rule runs through this whole guide: the PC that has Mizan installed is the server.** Keep it on, on a UPS, and back it up.

Download the installer from the latest release page:
https://github.com/rogod4rk/mizan-download/releases/latest/download/mizan-setup.exe
(or the release page: https://github.com/rogod4rk/mizan-download/releases/latest)
The file is called `mizan-setup-<version>.exe`. There is one installer for every kind of business. The activation code you get decides if it becomes a shop, restaurant, cafe or school.

---

## 1. What you need

**Do not skip the UPS.**

| Item | What to get |
|---|---|
| PC | Windows 10 or 11, 64-bit. 8 GB RAM and an SSD are the sensible minimum. 4 GB has not been tested. |
| UPS | 650 VA or bigger. It gives you minutes to finish the sale and shut down cleanly. |
| Receipt printer | Thermal, 80 mm, USB, with auto-cutter. Epson TM-T20III is the one we test on. XPrinter and Rongta work too. |
| Barcode scanner | USB, "keyboard wedge" type. It needs no driver. |
| Cash drawer | Plugs into the printer (RJ11), not into the PC. Metal one, 24 V if your printer is Epson. |
| Router | Only if you want a second till, tablets or phones. |
| USB flash drive | For backups. |

Before you install:

1. Plug in the printer, scanner and drawer.
2. Install the printer driver from the maker's website.
3. Print a **Windows test page**. Do not continue until it prints.
4. Make that printer the **default printer** in Windows.
5. Log in to Windows as an Administrator.
6. Remove any old POS program that is running on this PC.

A 32-bit PC, or Windows older than 10, is refused by the installer with a message.
A PC that already has a Mizan demo installed is refused too. Use another PC.

---

## 2. Install on one PC (one till)

1. Copy `mizan-setup-<version>.exe` to the PC.
2. Right-click it, then **Run as administrator**.
3. Windows may show a blue box "Windows protected your PC". Click **More info**, then **Run anyway**. The installer is not signed with a publisher certificate yet.
4. Click Next. Keep the default folder (`C:\Program Files\Mizan`).
5. On the tasks page, **leave the box unticked** ("السماح لأجهزة أخرى في المحل بفتح البرنامج"). That box is for section 3.
6. Shop page:
   - Shop name (اسم المحل): as you want it on receipts.
   - Shop code (رمز المحل): small English letters, digits or underscore, 2 to 50 characters. Example `amana`. **Write it down.**
7. Password page: choose the admin password (كلمة مرور المدير), 8 characters or more. There is no default password. Nobody can read it back for you.
8. Click Install (تركيب). The last step "preparing the database" takes about a minute. **Do not switch the PC off.**
9. Mizan opens by itself.
10. Restart Windows once. Check that Mizan opens from the desktop icon again.

What was installed:

| Thing | Where |
|---|---|
| Program | `C:\Program Files\Mizan` |
| All your data, backups, logs | `C:\MizanData` |
| Database | Windows service `MizanDB` (port 3307, or the next free port) |
| Server | Scheduled task `MizanPOS`, starts when Windows starts, even before anyone logs in |
| Web address | port `5009` on this PC |

The desktop icon "ميزان" opens Mizan in its own window. In that window **receipts print straight to the Windows default printer, with no print dialog.** That is why the default printer matters.

### First login

1. Shop code: leave the field closed if you like, but see the warning in section 7. **We suggest you always open "رمز المتجر" and type your shop code.**
2. User `admin` and the password you chose.
3. The activation screen (تفعيل المنظومة) appears. Go to section 5.
4. After activation: Settings (إعدادات النظام والتهيئة) for the name, address and phone on the receipt. Then the opening balances wizard (معالج أرصدة البداية ورأس المال): your shop, your money, your stock, what people owe you, what you owe suppliers, review.
5. Open the users screen (إدارة المستخدمين). **Create a second admin account in your own name.** It saves you if you forget the `admin` password.
6. Create one account per cashier. Never give cashiers the admin password.

### Printer, scanner, drawer

**Printer.** Sale receipts are laid out for 80 mm paper. There is no setting to switch them to 58 mm. For a proof print go to Settings (إعدادات النظام والتهيئة), tab "الأجهزة", then press "طباعة تجربة 80 ملم" (there is also "طباعة تجربة 58 ملم", a test only). Arabic letters must be joined, text must run right to left, and the dotted frame must show on all four sides. A missing side means the printer or its driver is cutting the paper.

**Scanner.** Open the sale screen, scan a barcode, the item is added. If you get odd letters instead of numbers, set the scanner back to factory default with its manual, then scan the "US Keyboard" code, with Windows typing in English.

**Cash drawer.**

- USB printer: the drawer is opened by the Windows printer driver. In the driver preferences turn on "Open drawer after print". It opens with every receipt, not only cash sales.
- Network printer: Settings, "الأجهزة", turn on "طابعة الإيصال متصلة بالشبكة", type its IP and port 9100, then "فتح الدرج بعد البيع النقدي".

If the drawer does not open, the cause is the driver setting, the cable or the drawer voltage. It is not Mizan.

**Customer display.** Settings, "الأجهزة", "فتح شاشة الزبون". Drag the window to the second screen and press F11. It works on the same PC only.

**Weighing scale.** USB or RS232 scale, Chrome or Edge on Windows only. Settings, "الأجهزة". If the scale does not answer, the cashier types the weight by hand and the sale goes on.

---

## 3. Install as the shop's server with other devices

Use this when you want a second till, a tablet, a kitchen printer, or waiter phones.

**The other devices are only browsers. You install nothing on them.** They open Mizan from the server PC over your shop's router.

### Setup

1. Put the server PC on a cable to the router if you can. Wi-Fi works but a cable is safer.
2. Run the installer as in section 2, but on the tasks page **tick the box** "السماح لأجهزة أخرى في المحل بفتح البرنامج (فتح منفذ الشبكة)".
   That box does two things: the server listens on the whole network instead of this PC only, and it opens Windows Firewall for TCP port 5009.
3. The firewall rule covers Private and Domain networks only. In Windows, set your shop network to **Private** (Settings, Network, your Wi-Fi or Ethernet, Network profile type). On a Public network, other devices cannot connect.
4. Find the address: Start menu, **ميزان**, **جهاز ثانٍ في المحل** (second device in the shop). A page opens with an address like `http://192.168.1.5:5009` and a QR code.
5. On the tablet or second PC, on the same router, type that address in the browser, or scan the QR code with the phone camera. Log in with a normal user account.

### Keep the address from changing

Routers hand out new addresses. If the address changes, devices stop finding the server.

- Best fix: in your router, reserve a fixed address for the server PC (DHCP reservation, or "static lease"). Your installer can do it in two minutes.
- The page in step 4 is created at install time and is not refreshed by the Start menu entry. If the address changed, make a new page: open PowerShell as Administrator and run
  `powershell -ExecutionPolicy Bypass -File "C:\Program Files\Mizan\installer\lan.ps1" -Show`
  Or just run `ipconfig` on the server and read the IPv4 address, then type `http://<that address>:5009`.

### If you forgot to tick the box

Do not run the installer again for this (see section 11). Ask your installer to do it by hand. This is what the box does:

1. Open Notepad as Administrator. Open `C:\MizanData\config\mizan.env`.
2. Change the line `ASPNETCORE_URLS=http://127.0.0.1:5009` to `ASPNETCORE_URLS=http://0.0.0.0:5009`. Save.
3. In an Administrator command prompt:
   `netsh advfirewall firewall add rule name="Mizan POS" dir=in action=allow protocol=TCP localport=5009 profile=private,domain`
4. Restart the PC.

This was written from the installer's own steps. It has not been run on a real shop PC.

### How many devices you can use

Your activation code sets the size of your shop:

| Size | Devices | Users |
|---|---|---|
| S (صغير) | 1 | 3 |
| M (متوسط) | 2 | 15 |
| L (كبير) | 5 | no limit |

**A device is any browser that logged in during the last 30 days.** Three things to know:

- The desktop icon window on the till PC is one device. A normal Chrome or Edge tab on the same PC is another device. Use only the icon on the till.
- Clearing browser data or using a private window makes a new device.
- A device that was seen once and then unused for 30 days frees its place by itself.

More devices or a bigger size: ask your provider for a new code. See section 5.

### Second till

Sign in on the second PC with its own cashier account. Both tills share one stock and one cash book. If both sell the last unit in the same second, one sale goes through and the other is refused with an out-of-stock message.

Receipts print through the browser of the device you sell on. The thermal printer is plugged into the server PC. A tablet cannot print on it unless it is shared on the network. For the kitchen, see below.

### Kitchen printer, waiter phones (restaurant and cafe codes)

- The server prints kitchen tickets itself, over the network, to the kitchen printer. It does not matter which device sent the order.
- Use a **network** ESC/POS thermal printer (LAN cable, port 9100). **Give it a fixed IP.** XPrinter models ship on `192.168.123.100`, outside the usual router range, so set a fixed address inside your router range.
- Settings, tab "طابعة المطبخ": switch on "تفعيل طابعة المطبخ", type the printer IP, port 9100, choose when to print:
  - "عند «الإرسال للمطبخ» في شاشة الطاولات"
  - "عند تسديد بيعة كاونتر تحتوي صنف مطبخ"
  - "الحالتان معاً"
- Press "طباعة ورقة تجربة".
- On each menu item, tick "يُرسل إلى المطبخ" in the item card (بطاقة صنف). An item without it never prints in the kitchen.
- Waiters use the tables screen (الطاولات والطلبات المفتوحة) on a phone or tablet browser with their own login. Each waiter phone counts as a device and each waiter as a user, so check your size.
- If the kitchen printer is off in Settings, Mizan prints the ticket through the browser of the device instead.

**There is no kitchen display screen yet.** The kitchen gets paper tickets. A tablet cannot act as a kitchen screen.

---

## 4. Staying fully offline

**Mizan needs no internet to install, activate, sell, print, take backups or restore.** The page uses no outside fonts or files.

Internet is only used for:

- Sharing by WhatsApp (invoices, statements, reminders, the end-of-day digest). The device opens WhatsApp, so that device needs internet.
- Updates over the internet, if your provider switched that on.
- Backups to your provider's server (Care), if you use that.
- Remote support, when you ask for it.

### Power cuts

- The server starts by itself when Windows starts, before anyone logs in. After the power returns, wait a minute, then open the desktop icon.
- A UPS gives you time to finish the sale and shut down. In Windows power settings, set the PC to shut down at 20% battery.
- A confirmed invoice survives a power cut. An invoice you were still typing does not exist yet.
- **While the server PC is off, tablets and second tills cannot open Mizan.**

### When a device loses the server

A tablet can lose the server for a moment (Wi-Fi drop, server restarting). If the page is already open:

- Sales, sale returns, purchase returns and damage entries are kept in that browser. They show as "لم تصل الخادم بعد".
- They are sent automatically in order when the server is back. Retry runs every 30 seconds, when you return to the window, and when the network comes back. Invoice numbers are fixed first, so nothing is posted twice.
- A real refusal from the server (not enough stock, debt limit, item stopped) is shown to the cashier and not queued.
- **Before logging out, wait until the pending count reaches zero.** Logging out postpones them until the same person logs in again on the same device.
- **Do not clear browser data** on a device that still has pending sales.
- The item list is cached, so the till can still ring up sales if the page is reloaded during a short outage.
- Two devices that sell the last unit while offline can both be accepted. Such sales show up in "مراجعة عمليات المزامنة" with the reason (negative stock, different price, debt limit passed). Check that screen after an outage.

---

## 5. Going online: what exists and what does not

### What exists

**Backup outside the shop.** Screen "النسخ الاحتياطي والاستعادة", section "النسخ خارج المحل".

- "فلاشة أو مجلد شبكة": a USB drive or a shared folder.
- "خادم ميزان للنسخ (رابط وحساب من المزوّد)": the provider's backup server. This is **Mizan Care** (رعاية ميزان). The provider gives you a link, an account name and a password.
- The backup is encrypted on your PC before it leaves. You choose a passphrase of 10 characters or more. **Write it on paper. If you lose both the PC and the passphrase, nobody can open those backups, including us.**
- Press "احفظ الوجهة", then "زامن الآن". Large backups go in pieces and resume after an internet cut.
- If Care has ended, uploads to the provider's server stop. Selling, local backups and USB backups keep working.

**Updates over the internet.** Screen "تحديثات المنظومة". Off unless your provider sets the update link. See section 11.

**Remote support.** On a call, your provider may ask you to run RustDesk (free). It shows a number and a temporary password on your screen, you read them out, and the provider sees your screen only for that call. Close it when done. It leaves nothing running.

**WhatsApp.** Invoices, account statements, instalment and debt reminders, and the end-of-day digest (نشرة نهاية اليوم) open WhatsApp from the browser. You press send.

**More than one branch.** Each branch is its own shop: its own PC, its own database, its own licence (one licence is one location). A branch keeps selling when the link between branches is down.
The screens are "إدارة المتاجر المتعددة" (rollup of all branches' sales) and "تحويل بين الفروع وقوائم موحدة" (stock transfers by signed package, price push, consolidated trial balance, income statement and balance sheet).
Honest limits:

- **Linking branches is technician work.** There is no screen to add a branch.
- The rollup reads each branch's database over a network link between the servers. Mizan does not provide that link. A branch that cannot be reached is shown as unreachable with its last known figures and date. It is never shown as zero.
- A stock transfer travels as a signed file or a WhatsApp text, so it works with no live link.

### What does not exist yet

- **A true online or cloud mode.** There is none. Mizan is not a website you sign up to.
- **Owner access from home.** There is no relay, tunnel or cloud view. The owner dashboard (لوحة المالك) opens from the shop network only. From home you get the WhatsApp end-of-day digest and your off-site backups.
- **Running the server on an online VPS.** Not supported yet. The server and the installer are built for Windows only, and the web server speaks plain http without HTTPS.
- **Do not forward port 5009 from your router to the internet.** It has no HTTPS in front of it.

---

## 6. Activation

Until you activate, Mizan lets you look at everything but blocks new sales and purchases. Backup and restore always work.

1. Log in. The activation screen (تفعيل المنظومة) opens by itself.
   If you ever need it again: side menu, group "الإدارة والإعدادات", screen "الترخيص والأجهزة". Or press F12 and then H for "حالة المنظومة والدعم": the licence row shows the install number.
2. Find the **install number** (رقم التركيب), 16 letters and digits like `A1B2-C3D4-E5F6-7890`.
3. Press **"نسخ الرقم"** (copy). Paste it into WhatsApp and send it to your provider with your shop name.
4. The provider sends back a long activation code.
5. Paste the whole message into the box "رمز التفعيل". Spaces, new lines and Arabic-Indic digits are fixed for you. Do not retype it by hand, one wrong letter is rejected.
6. Press **"تفعيل الترخيص"**. A green screen "تمّ تفعيل الترخيص" shows the edition and the date it is valid to.

If it fails, you can press "الدخول بوضع محدود": reading, reports and backup work, selling does not. Send your provider a photo of the screen.

### Renewals

- When a payment period ends you get a renewal code. Paste it in the same box, or in "الترخيص والأجهزة", the box "رمز من المزوّد", then "طبّق الرمز".
- A banner appears in the last 10 days, and gets louder in the last 3.
- After the end date there are **14 days of grace** with a banner. Selling still works.
- After the grace, new sales, purchases and receipts are blocked. Reports, statements, backup and restore keep working. Paste the new code and you are selling again.
- A permanent licence never expires.
- Care (رعاية ميزان) has its own date on the same screen. When it ends, updates and backups to the provider's server stop. Selling never stops because of Care.
- The same screen lets you apply any other code from your provider: extra device, bigger size, Care.

---

## 7. Daily use basics

- **Login.** Shop code, username, password. The shop code sits in a collapsed line "رمز المتجر (اتركه فارغاً إن كان لديك متجر واحد)". The screen says you can leave it empty with one shop.
  **Type it anyway, every time, on every device.** On older versions, logging in without it can make the backup list look empty, because the daily backups are saved under your shop code.
- **Open the till.** After login, enter the opening cash when asked. The top bar shows "وردية مفتوحة".
- **Sell.** Scan or search, then pay. Keys:

| Key | Does |
|---|---|
| F2 | Cash |
| F3 | Card |
| F6 | Bank transfer |
| F7 | Split payment |
| F8 | On account |
| F4 | Hold the cart |
| F5 | Parked carts list |
| F9 | Invoice history (returns start there) |
| F12 | Menu of all screens |

- **Menu.** The side menu on a PC and the F12 menu on any device list every screen. Phones show a lower bar.
- **Close the day.** Screen "إغلاق اليوم": count the drawer, print the Z report, take a backup, send the digest, close the shift.
- **Backups happen by themselves.** Mizan takes one daily backup when none is newer than 20 hours, and keeps the last 14 daily ones. It needs no login and no internet. It sits on the same disk as your data. See section 8.
- **Users and roles.** Admin, Manager, Cashier, Inventory clerk and Accountant (plus Teacher on school codes). A cashier cannot see profit reports, edit users or change settings. The limit on active users comes from your size.
- **Importing from an old system** is section 10.

---

## 8. Backup and restore

The daily backup on the PC dies with the PC. **Get a copy out of the shop.**

### Make a copy you can restore (simple, do this weekly)

Every backup is two files with the same name:

- `<shopcode>_daily_<date>.sql`
- `<shopcode>_daily_<date>.sql.json` (the "card" that proves the file is whole)

1. Open `C:\MizanData\backups` in File Explorer.
2. Copy the newest `.sql` **and** its `.sql.json` to a USB drive.
3. Take the drive out of the shop.

**Always copy both files.** The restore refuses a `.sql` without its card.

Easier: the "نزّل" (download) button in the backup screen gives one `.zip` holding both files. Save it to the USB drive as is. (Version 0.0.26 gave the `.sql` alone, which cannot be restored: on that version use File Explorer as above.)

### Automatic copy

"النسخ خارج المحل" (section 5) sends each backup encrypted to a USB drive, a shared folder or the provider's server. The files there are encrypted pieces, not a ready `.sql`. **Keep the passphrase: without it nobody can open them.**

To bring one back (for example on a new PC): same screen, set the same destination and passphrase, press "اعرض النسخ في الوجهة", then "انقلها إلى هذا الجهاز" on the one you want. It then appears in the backup list, ready for "استعِد".

### Check and take a backup now

Screen "النسخ الاحتياطي والاستعادة" (group "الإدارة والإعدادات"):

- "خذ نسخة الآن": backup now.
- "افحص": checks a backup is whole. A green "سليمة (فُحصت)" is what you want.

### Restore

This **replaces everything in the shop** with the content of the backup. Sales after the backup date disappear. Mizan takes a safety backup of the current state first and tells you its name.

1. Open "النسخ الاحتياطي والاستعادة".
2. Find the backup and press **"استعِد"**.
3. Type the word **استعادة** in the box. If the backup was taken from a different database, the screen asks for «استعادة من قاعدة أخرى».
4. Press "استعِد الآن".
5. Log in again. After a restore, the passwords are the ones from the backup.

The backup list only shows files from the same shop code. If the list is empty, check that the files are in `C:\MizanData\backups`, both of each pair.

---

## 9. Moving to a new PC

1. **On the old PC** (if it still works): take a backup now. Copy the newest `.sql` and `.sql.json` from `C:\MizanData\backups` to a USB drive. Write down your **shop code** and your passphrase if you use off-site backups.
2. **On the new PC:** do section 1 (printer, default printer), then install as in section 2 with the **same shop code**. The restore only accepts backups with the same code. The admin password you type now is replaced by the old one when you restore.
3. Log in. If the activation screen shows, press "الدخول بوضع محدود". Backup and restore work without an activation.
4. Copy the two backup files into `C:\MizanData\backups` on the new PC.
5. Screen "النسخ الاحتياطي والاستعادة": press "استعِد" on that backup, type استعادة.
6. Open "الترخيص والأجهزة":
   - Green and valid: you are done.
   - Asking for activation: press "نسخ الرقم" and send the new **install number** to your provider. A new PC with a clean install has a new number, and the old code will not match. The provider moves your licence and sends one code. You paste it once.
7. Set the browser windows on other devices again with the new PC's address (section 3).
8. Switch the old PC off for good. Do not run both.

If the old disk is dead and you have no backup copy, the data cannot be recovered. Nobody can rebuild it from a dead disk. Install clean and re-enter your opening balances and items (section 10 can help).

---

## 10. Moving from an old system (data import)

Screen **"نقل البيانات (استيراد وتصدير)"** (group "التركيب والبيانات", admin login). Takes `.xlsx`, `.xls` and `.csv`.

1. In the old program, find "Export", "Save to Excel" or "Print to Excel" in the items screen and the customers screen. Save to the desktop.
2. Open the import tab (استيراد). Choose the file (اختيار ملف إكسل أو CSV).
3. Mizan finds the header row and the columns itself and tells you what kind of file it thinks it is. Fix the guess if it is wrong.
4. Press the check button (فحص الملف). **Nothing is written yet.** Read the warnings: price below cost, zero price, zero cost on opening stock, a customer that looks like a supplier. Warnings do not block. Blocking errors reject only their own rows.
5. Confirm.
6. Order: **items first**, then customers and suppliers. If the items file has a quantity column, the opening stock is entered with the items.
7. Barcodes like `6.24E+12` were ruined by Excel. Set the barcode column to "Text" in your own file and retype them.
8. Wrong import? Press "تراجع عن آخر استيراد" at the top. It works until something from that import is sold or edited.
9. Check three item prices and stocks, two customer debts, then make one test sale.

The export tab gives the same columns as CSV, so you can also use it to see the format. Invoices and vouchers are exported for archive only. They cannot be imported. Sales are made from the till.

---

## 11. Updating to a new version

Take a backup first (section 8) and copy it to USB.

**Way 1, inside Mizan.** Screen "تحديثات المنظومة".

- The box "قناة التحديث" says "مفعَّلة (اتصال بالإنترنت)" or "معطَّلة (فلاشة فقط)". If it is off, your provider has not set the update link: use way 2.
- If on: "افحص وجود تحديث", then "نزّل التحديث", "اعتمِد التحديث", "ركّب التحديث", and when asked "إعادة التشغيل الآن".
- It installs only between 23:00 and 05:00. To install now with the shop closed, tick "ركّب الآن رغم وقت البيع".
- It takes a backup first and rolls back by itself if the database upgrade or the new program fails.
- It needs an active Care (رعاية ميزان) and internet.

**Way 2, a new installer.** This is how updates arrive when the update link is off: the screen itself says "تصل التحديثات كتركيب جديد عبر فلاشة".

1. Download the new `mizan-setup-<version>.exe` and run it on the server PC (right-click, Run as administrator).
2. It recognises the existing install and fills in your shop code. **Keep the same shop code.** A different code would open an empty shop.
3. Keep the "other devices" box the way it was, so other tills stay connected.
4. It stops Mizan, copies the new files, upgrades the database and starts Mizan again. Your password, sales and settings stay.
5. If you see "لم يكتمل التجهيز", Mizan starts the old setup again by itself. Do not delete `C:\MizanData`. Send `C:\MizanData\logs\setup.log` and a photo of the screen to your provider.

---

## 12. Uninstall

1. Take a backup and copy it to USB.
2. Windows Settings, Apps, find **ميزان <version>**, Uninstall.
3. It asks if you want to delete the shop data too. **Choose No** unless you are sure. A second question asks again. Both default to No. Deleting is permanent and nobody can recover the books.
4. Uninstall removes the program, the `MizanDB` service, the `MizanPOS` task and the firewall rule.
5. With No, `C:\MizanData` stays (database, backups, logs). Re-attaching a new install to it is a technician job. To change PC, use section 9.

---

## 13. Troubleshooting

| What you see | What to do |
|---|---|
| **"Refused to connect" / "This site can't be reached"** (ERR_CONNECTION_REFUSED) | The server is not up yet. Wait 1 to 2 minutes after Windows starts, then reload. Still nothing: open Task Scheduler, find `MizanPOS`, press Run. Open `services.msc`, find `MizanDB`, press Start. Log: `C:\MizanData\logs\mizan.log`. From another device: the server PC is off, it is a different router, the "other devices" box was not ticked, the network is set to Public, or the address changed (section 3). |
| **"Windows protected your PC" (blue box)** | More info, then Run anyway. It shows because the installer has no publisher certificate yet. |
| **Receipt has margins, is cut off, or a print box appears** | Make the thermal printer the Windows default. Open Mizan from the desktop icon, not from a normal browser tab. Use 80 mm paper (58 mm rolls come out squeezed or cut). Print the proof page in Settings, "الأجهزة". If you must print from a normal tab: Margins None, Scale 100, untick Headers and footers, paper 80mm. |
| **Forgot the admin password** | Another admin resets it in "إدارة المستخدمين". No second admin: your installer runs, in an Administrator PowerShell with Mizan running: `powershell -ExecutionPolicy Bypass -File "C:\Program Files\Mizan\installer\setup.ps1" -Mode ResetAdmin -ShopCode <code> -AdminPassword "<new password, 8+ characters>"` |
| **Device limit reached** (login says the licensed device count is full) | Log in on a device that works. Open "الترخيص والأجهزة", find the old device in the list "الأجهزة المسجَّلة", press "اسحب الجهاز", confirm "نعم، اسحبه". Or the login screen itself names the oldest device and asks to pull it. Remember: a normal browser tab on the till PC counts as a device, and so does a cleared browser. For more devices, ask your provider for a bigger size or an extra device. |
| **Users limit reached** (the size allows N active users) | Stop a user you no longer need in "إدارة المستخدمين", or ask your provider for a bigger size. |
| **Licence expired / "انتهت مدة الترخيص"** | Send your provider the install number (copy it from the screen). Paste the new code (section 6). Reports and backups work meanwhile. |
| **Banner about the PC date** | The Windows date is far from the last run. Fix the date, time and time zone in Windows. Selling keeps working. |
| **Scanner types odd letters** | Windows typing language must be English while you scan the "US Keyboard" setup code from the scanner manual. |
| **Cash drawer does not open** | See section 2, "Cash drawer". |
| **Backup list empty** | On older versions this happens when you logged in without the shop code. Log out, log in with the code typed. |
| **Restore says the backup belongs to another shop** | The new install has a different shop code from the backup, or you logged in in a different way than when the backup was made. Use the same code (section 9). |
| **Antivirus blocks the installer** | Pause it during install only, then switch it back on. |
| **A tablet will not load Mizan** | Is the server PC on? Same router? Server PC network set to Private? Right address? Try the address on the server PC itself first. |

### What to send to support

1. Screen "حالة المنظومة والدعم" (F12 then H): press "احفظ ملف الدعم وأرسله". It saves a plain text file with no customer names, prices or passwords. Open it in Notepad first if you want to read it.
2. A photo of the screen at the moment of the problem.
3. Your shop code, and what you were doing.
4. If asked: `C:\MizanData\logs\mizan.log`.
