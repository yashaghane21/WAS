################################################################
# EXP 01 - Install Burp Suite in Kali Linux / Intercept traffic
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Install Burp Suite in Kali Linux and use it as an intercepting
proxy to analyze HTTP/HTTPS traffic between a browser and a web app.

TOPIC / CONCEPTS:
- Burp Suite = integrated web app security testing platform (by PortSwigger).
- Intercepting proxy = sits BETWEEN browser and server:
      Browser -> Burp Suite -> Web Server
  lets you capture, view, modify, and replay requests/responses.
- Major Burp components:
    Proxy    - intercept/forward/modify HTTP traffic
    Repeater - resend a single request repeatedly
    Intruder - automate sending many request variations
    Scanner  - automated vulnerability scanning (edition-dependent)
    Decoder  - encode/decode data
    Comparer - compare two requests/responses
- An HTTP request has: method, URL/path, HTTP version, Host, User-Agent,
  cookies, parameters, headers.
- An HTTP response has: status code, headers, cookies, HTML body.

----------------------------------------------------------------
SETUP (do this directly - works standalone)
----------------------------------------------------------------
# 1. Start Docker + the Juice Shop target
sudo systemctl start docker
sudo docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop
# if "name already in use":
sudo docker start juice-shop
# confirm:
sudo docker ps
# target -> http://127.0.0.1:3000

# 2. Install + launch Burp (usually already installed)
sudo apt update
sudo apt install burpsuite
burpsuite

----------------------------------------------------------------
STEPS
----------------------------------------------------------------
 1. Burp starts -> choose "Temporary Project" -> Next -> Start with default settings
 2. Go to Proxy -> Intercept -> click "Open Browser"
 3. Click "Intercept is off" so it turns ON
 4. In the browser go to:  http://127.0.0.1:3000
 5. Request freezes in Intercept tab -> show: method (GET), Host,
    User-Agent, headers
 6. Click Forward a few times -> page loads
 7. Go to Proxy -> HTTP history -> click a request -> open Response tab
    (status 200, headers, HTML)
 8. Turn Intercept OFF

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is Burp Suite?
A: A toolkit for testing web application security, acting as an intercepting proxy.
Q: What is an intercepting proxy?
A: A component between client and server that lets you view/modify requests.
Q: Purpose of the Proxy module?
A: To capture, forward, and modify HTTP traffic between browser and server.
