# EXP 00 - ONE-TIME SETUP (do first, every exam)

Targets: Juice Shop (Exp 1-4) | Mutillidae (Exp 5-10)
Burp proxy: 127.0.0.1:8080
Golden rules:
  - Page won't load? -> turn Burp Intercept OFF
  - Mutillidae weird? -> re-run "setup/reset the DB" + Security Level 0

================================================================
START THE TARGET APPS
================================================================

# Check if images already installed (if listed -> no internet needed)
sudo docker images

# Start Docker
sudo systemctl start docker

# Start Juice Shop (Exp 1-4)   [auto-downloads if missing + online]
sudo docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop

# Start Mutillidae (Exp 5-10)  [auto-downloads if missing + online]
sudo docker run -d --name mutillidae -p 8081:80 citizenstig/nowasp

# If "name already in use" -> just start existing ones:
sudo docker start juice-shop mutillidae

# Confirm running
sudo docker ps

# Juice Shop   -> http://127.0.0.1:3000
# Mutillidae   -> http://127.0.0.1:8081

================================================================
MUTILLIDAE DB SETUP (needed Exp 5-10)
================================================================
 1. Open http://127.0.0.1:8081
 2. If "database offline" error -> click "setup/reset the DB" link
 3. Wait for "Database has been built"
 4. Click "Toggle Security" until it shows: Security Level 0 (Hosed)

# If DB setup refuses / errors:
sudo docker restart mutillidae
# wait 15s, reload page, click "setup/reset the DB" again

================================================================
BURP + BROWSER
================================================================
 1. Terminal: burpsuite  -> Temporary Project -> default settings
 2. Proxy -> Intercept -> "Open Browser"  (pre-wired, no config)
 3. Intercept OFF by default. Turn ON only when capturing a request.
 (Firefox manual proxy if needed: 127.0.0.1 : 8080, also use for HTTPS)

================================================================
QUICK TROUBLESHOOTING
================================================================
Page won't load          -> Burp Intercept OFF
Mutillidae DB error      -> click "setup/reset the DB"; or: sudo docker restart mutillidae
Security not vulnerable  -> Toggle Security to 0
Image won't download     -> needs internet; check: sudo docker images
sqlmap finds no data     -> DB not built; set up DB, re-run --dbs, use printed name
CSRF has no effect        -> field names must match the captured request
Menu label differs       -> pick closest matching category
