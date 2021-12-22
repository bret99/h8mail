This repository contains fixed python modules with correct working of Breachdirectory API (original ones have troubles with getting  correct response from Breachdirectory resource).

One should:
1. pip3 install h8mail;
2. substitute classes.py "original" for classes.py from this repository;
3. substitute BREACHDIRECTORY_API_KEY for one's breachdirectory actual API key in line 850 in classes.py (already substituted to correct one from this repository) and uncomment generated h8mail_config.ini line that starts with 'breachdirectory_api =' and insert breachdirectory actual API key;
4. substitute gen_config.py "original" for gen_config.py from this repository;
5. substitute run.py "original" for run.py from this repository.
