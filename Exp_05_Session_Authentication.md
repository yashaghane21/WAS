################################################################
# EXP 05 - Attacking Session & Authentication in Web Apps
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Demonstrate common weaknesses in web app authentication and
session management, and how improper handling leads to unauthorized access.

TOPIC / CONCEPTS:
- Authentication = verifying WHO the user is (username/password, MFA, tokens).
- Authorization = what an authenticated user is ALLOWED to access.
    Auth  = "Who are you?"    Authz = "What can you access?"
- Session management = after login the app issues a SESSION ID (stored in a
  cookie, e.g. Set-Cookie: PHPSESSID=...) so you don't re-login each request.
- Common auth/session weaknesses:
    Weak passwords, weak auth controls, session fixation, session hijacking,
    improper session invalidation (session valid after logout),
    insecure cookies (missing Secure / HttpOnly / SameSite flags).

----------------------------------------------------------------
SETUP (do this directly - works standalone)
----------------------------------------------------------------
# 1. Start Docker + Mutillidae
sudo systemctl start docker
sudo docker run -d --name mutillidae -p 8081:80 citizenstig/nowasp
# if "name already in use":  sudo docker start mutillidae
sudo docker ps
# target -> http://127.0.0.1:8081

# 2. Build the DB + set security level
#    Open http://127.0.0.1:8081
#    If "database offline" -> click "setup/reset the DB" -> wait for "Database has been built"
#    Click "Toggle Security" until it shows: Security Level 0 (Hosed)
# If DB setup errors:
sudo docker restart mutillidae   # wait 15s, reload, click setup/reset the DB again

# 3. Launch Burp
burpsuite        # Temporary Project -> default
# Proxy -> Intercept -> Open Browser

----------------------------------------------------------------
STEPS
----------------------------------------------------------------
 1. Register a test user:
    Login/Register -> "Please register here"
    Username: testuser   Password: testpass  -> Create
 2. Intercept ON in Burp
 3. Log in as testuser -> login request freezes in Burp
 4. In the request show: POST method, URL, username= and password= params,
    headers, cookies. Forward it.
 5. Proxy -> HTTP history -> login response -> find:  Set-Cookie: PHPSESSID=...
    Check for Secure / HttpOnly / SameSite flags (missing = weakness)
    (Also view in Firefox: F12 -> Storage -> Cookies)
 6. Logout, press browser Back to an account page:
    if it still shows = weak session invalidation (secure app forces re-login)
 7. Try a wrong password -> should be rejected

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is authentication?
A: Verifying the identity of a user (e.g. username/password).
Q: What is session management in a web app?
A: Maintaining a logged-in user's state using a session ID stored in a cookie.
Q: Difference between authentication and authorization?
A: Authentication = who you are; authorization = what you're allowed to access.
