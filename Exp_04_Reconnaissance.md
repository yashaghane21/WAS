################################################################
# EXP 04 - Reconnaissance & Information Gathering
################################################################

----------------------------------------------------------------
WHAT THIS EXPERIMENT COVERS (say this if asked)
----------------------------------------------------------------
AIM: Perform reconnaissance against an authorized target to collect
publicly available info, DNS records, domain info, technologies, and
network services (the first phase of a security assessment).

TOPIC / CONCEPTS:
- Reconnaissance = gathering info about a target before detailed testing;
  helps understand the attack surface.
- PASSIVE recon = collect info WITHOUT directly interacting with the target
  (WHOIS, public DNS, search engines). Minimal footprint.
- ACTIVE recon = directly interact with the target (port scanning, service
  enumeration, direct DNS queries, tech fingerprinting). Generates traffic,
  so needs authorization.
- DNS record types:
    A     = IPv4 address      AAAA  = IPv6 address
    MX    = mail server       NS    = name server
    TXT   = text info         CNAME = canonical name
- Tools: whois (domain reg), nslookup/dig/host (DNS), whatweb (web tech),
  nmap (open ports + services).

----------------------------------------------------------------
SETUP (do this directly - works standalone)
----------------------------------------------------------------
# Tools (whois, dig, nmap, whatweb) are preinstalled in Kali.
# whois/dig/nslookup need INTERNET (use example.com).
# For local/offline, start Juice Shop and target 127.0.0.1:

sudo systemctl start docker
sudo docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop
# if "name already in use":  sudo docker start juice-shop
sudo docker ps
# local target -> http://127.0.0.1:3000

----------------------------------------------------------------
COMMANDS (Terminal)
----------------------------------------------------------------
# WHOIS - domain registration info (needs internet)
whois example.com

# DNS lookups (need internet)
nslookup example.com
dig example.com
dig example.com A
dig example.com NS
dig example.com MX
host example.com

# Web technology fingerprint (works offline on local app)
whatweb http://127.0.0.1:3000

# Port scan local target (works offline)
nmap 127.0.0.1
nmap -sV 127.0.0.1

# List app hidden routes (matches PDF)
curl -s http://127.0.0.1:3000/main.js | grep -Eo '/rest/[A-Za-z0-9_./?-]+' | sort -u | head -20

# Organize output (IPs, ports, tech) = your target profile.

----------------------------------------------------------------
VIVA
----------------------------------------------------------------
Q: What is reconnaissance in cybersecurity?
A: The initial phase of collecting info about a target before testing.
Q: Difference between passive and active recon?
A: Passive = no direct contact with target; active = directly interacts with it.
Q: Purpose of the whois command?
A: To obtain public domain registration details (registrar, name servers, etc.).
