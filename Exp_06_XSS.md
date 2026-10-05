################################################################
# EXP 06 - Detect & Exploit Reflected, Stored, DOM-Based XSS
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Detect and demonstrate (in a controlled lab) Reflected, Stored, and
DOM-Based Cross-Site Scripting, and understand impact + prevention.

TOPIC / CONCEPTS:
- XSS = a web vuln where untrusted input is interpreted as executable
  JavaScript in a user's browser (happens when input is placed into a page
  without proper output encoding).
- REFLECTED XSS: input is immediately returned in the response.
  Flow: Attacker input -> request -> response -> victim browser -> script runs.
  Usually via a URL parameter or form field.
- STORED XSS: input is saved by the app (e.g. database) and runs whenever
  anyone views the affected page. Broader impact (many users).
  Flow: Input -> storage -> response -> browser.
- DOM-BASED XSS: client-side JavaScript takes attacker-controlled data
  (URL param/fragment) and writes it into a dangerous DOM sink.
  Dangerous sinks: innerHTML, document.write(), eval().
  Flow: Input -> client-side JS -> DOM -> browser.
- Prevention: input validation, context-aware OUTPUT ENCODING, avoid unsafe
  sinks/eval, Content Security Policy (CSP), use secure frameworks.

----------------------------------------------------------------
SETUP (do this directly - works standalone)
----------------------------------------------------------------
sudo systemctl start docker
sudo docker run -d --name mutillidae -p 8081:80 citizenstig/nowasp
# if "name already in use":  sudo docker start mutillidae
sudo docker ps
# Open http://127.0.0.1:8081
#  - if "database offline" -> click "setup/reset the DB"
#  - Toggle Security until: Security Level 0 (Hosed)
# If DB errors:  sudo docker restart mutillidae  (wait 15s, reset DB again)

----------------------------------------------------------------
REFLECTED XSS
----------------------------------------------------------------
Menu: OWASP 2017 -> A7 Cross Site Scripting (XSS) -> Reflected (First Order) -> DNS Lookup
In the Hostname/IP box paste:

<script>alert('XSS')</script>

Click Lookup -> alert box pops = reflected XSS.

----------------------------------------------------------------
STORED XSS
----------------------------------------------------------------
Menu: A7 XSS -> Persistent (Second Order) -> Add to your blog
In the blog entry box paste:

<script>alert('Stored XSS')</script>

Save -> revisit/refresh View Blogs -> alert pops every load = stored XSS.

----------------------------------------------------------------
DOM-BASED XSS
----------------------------------------------------------------
Menu: A7 XSS -> DOM-Based -> HTML5 Web Storage   (or "Via input (GET/POST)")
In the input field paste:

<script>alert(1)</script>

Submit -> alert pops = DOM XSS.

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is Cross-Site Scripting (XSS)?
A: A vuln where untrusted input runs as script in a victim's browser.
Q: Difference between Reflected, Stored, and DOM-Based XSS?
A: Reflected = echoed back immediately; Stored = saved and runs for all viewers;
   DOM = unsafe client-side JS writes input into the page.
Q: Main difference between Stored and Reflected XSS?
A: Stored persists and affects many users; reflected needs a crafted link/request.
