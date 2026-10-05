################################################################
# EXP 02 - Crawling and Scanning using Burp Suite
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Use Burp Suite to crawl a web app, map its resources/attack surface,
then scan it for security issues.

TOPIC / CONCEPTS:
- Crawling = systematically discovering the app's pages, links, directories,
  forms, parameters, requests, and endpoints (maps the ATTACK SURFACE).
- Scanning = analyzing those discovered components for vulnerabilities
  (input params, headers, auth, session mgmt, common web vulns).
- Site map (Target tab) = the tree of everything Burp discovered.
- Findings are graded by severity: High / Medium / Low / Informational.
- Each finding shows: affected URL, request, response, description,
  severity, evidence, remediation.
- Difference: crawling FINDS content; scanning TESTS it for weaknesses.

----------------------------------------------------------------
SETUP (do this directly - works standalone)
----------------------------------------------------------------
# Start Docker + Juice Shop target
sudo systemctl start docker
sudo docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop
# if "name already in use":  sudo docker start juice-shop
sudo docker ps
# target -> http://127.0.0.1:3000

# Launch Burp
burpsuite        # Temporary Project -> default settings
# Proxy -> Intercept -> Open Browser   (keep Intercept OFF)

----------------------------------------------------------------
STEPS
----------------------------------------------------------------
 1. In the Burp browser, browse Juice Shop fully: http://127.0.0.1:3000
    (open products, Login page, Account, different sections)
 2. Burp -> Target -> Site map (left panel)
    = the CRAWL result: discovered URLs / folders / parameters. Show it.
 3. Right-click host http://127.0.0.1:3000 -> Scan
 4. Open Dashboard / Issues -> review findings by severity
    (High / Medium / Low / Informational)
 5. Click one finding -> show: Affected URL, Request, Response,
    Description, Severity, Remediation

NOTE: Burp Community has no full active scanner. If "Scan" is greyed out,
show the Site map + the Issues/Dashboard panel and explain it. (Accepted.)

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is crawling in web app security?
A: Discovering the app's pages, links, and parameters by following its content.
Q: What is scanning in Burp Suite?
A: Analyzing discovered components to identify vulnerabilities.
Q: Difference between crawling and scanning?
A: Crawling finds the content; scanning tests that content for weaknesses.
