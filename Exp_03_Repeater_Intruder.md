################################################################
# EXP 03 - Burp Repeater and Intruder
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Use Burp Repeater and Burp Intruder to analyze and test HTTP
requests by modifying them and observing responses.

TOPIC / CONCEPTS:
- Repeater = capture a request, modify it, and resend it repeatedly by hand.
  Good for: testing single parameters, modifying headers, comparing responses,
  manual investigation, input-validation testing.
  Flow: Capture -> Send to Repeater -> Modify -> Send -> Analyze response.
- Intruder = AUTOMATE sending many variations of a request with different
  payload values. Good for: parameter fuzzing, input-validation testing,
  controlled auth testing.
  Flow: Capture -> Send to Intruder -> Select positions (§markers§)
        -> Configure payloads -> Start attack -> Analyze responses.
- Compare responses by: status code, response length, content, time.
- Difference: Repeater = manual/single; Intruder = automated/bulk.

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
# Proxy -> Intercept -> Open Browser

----------------------------------------------------------------
REPEATER
----------------------------------------------------------------
 1. Open Juice Shop login page, Intercept ON
 2. Enter any email/password, click Login -> request freezes
 3. Right-click the request -> Send to Repeater -> Repeater tab
 4. Change a value (e.g. email) -> click Send -> read Response
    (status code, length, body). Change again, Send again -> compare.

----------------------------------------------------------------
INTRUDER
----------------------------------------------------------------
 5. Back in Proxy, right-click same login request -> Send to Intruder
 6. Positions tab -> click "Clear §", highlight the password value -> "Add §"
 7. Payloads tab -> add values:
test1
admin
password
123456
 8. Click "Start attack"
 9. Compare Status code + Length columns across payloads
    (different length/status = different behavior)

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is Burp Repeater?
A: A tool to manually modify and resend a single HTTP request.
Q: What is Burp Intruder?
A: A tool to automate sending many request variations with different payloads.
Q: Main difference between Repeater and Intruder?
A: Repeater is manual/single-request; Intruder is automated/bulk testing.
