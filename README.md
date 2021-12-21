It's a fork from https://github.com/khast3x/h8mail with correct setttings of Breachdirectory API (original has troubles to get correct response from one's API provided by not correct code).
One should substitute BREACHDIRECTORY_API_KEY for one's breachdirectory API key in lines 834 and 855 in classes.py.

## Features

Email pattern matching (reg exp), useful for reading from other tool outputs

Pass URLs to directly find and target emails in pages

Loosey patterns for local searchs ("john.smith", "evilcorp")

Painless install. Available through `pip`, only requires `requests`

Bulk file-reading for targeting

Output to CSV file or JSON

Compatible with the "Breach Compilation" torrent scripts

Search cleartext and compressed .gz files locally using multiprocessing

Compatible with "Collection#1"

Get related emails

Chase related emails by adding them to the ongoing search


Supports premium lookup services for advanced users

Custom query premium APIs. Supports username, hash, ip, domain and password and more

Regroup breach results for all targets and methods

Includes option to hide passwords for demonstrations

Delicious colors

---

### `cd h8mail && python3 utils/run.py`

-----


####  APIs
| Service | Functions | Status |
|-|-|-|
| [HaveIBeenPwned(v3)](https://haveibeenpwned.com/) | Number of email breaches | :white_check_mark: :key: |
| [HaveIBeenPwned Pastes(v3)](https://haveibeenpwned.com/Pastes) | URLs of text files mentioning targets | :white_check_mark: :key: |
| [Hunter.io](https://hunter.io/) - Public | Number of related emails | :white_check_mark: |
| [Hunter.io](https://hunter.io/) - Service (free tier) | Cleartext related emails, Chasing | :white_check_mark: :key: |
| [Snusbase](https://snusbase.com/) - Service | Cleartext passwords, hashs and salts, usernames, IPs - Fast :zap: | :white_check_mark: :key: |
| [Leak-Lookup](https://leak-lookup.com/) - Public | Number of search-able breach results | :white_check_mark: (:key:) |
| [Leak-Lookup](https://leak-lookup.com/) - Service | Cleartext passwords, hashs and salts, usernames, IPs, domain | :white_check_mark: :key: |
| [Emailrep.io](https://emailrep.io/) - Service (free) | Last seen in breaches, social media profiles | :white_check_mark: :key: |
| [scylla.so](https://scylla.so/) - Service (free) | Cleartext passwords, hashs and salts, usernames, IPs, domain | :construction: |
| [Dehashed.com](https://dehashed.com/) - Service | Cleartext passwords, hashs and salts, usernames, IPs, domain | :white_check_mark: :key: |
| [IntelX.io](https://intelx.io/signup) - Service (free trial) | Cleartext passwords, hashs and salts, usernames, IPs, domain, Bitcoin Wallets, IBAN | :white_check_mark: :key: |
| :new: [Breachdirectory.tk](https://breachdirectory.tk) - Service (free) | Cleartext passwords, hashs and salts, usernames, domain | :white_check_mark: :key: |

API key required*  




-----

## Usage

```bash
usage: h8mail [-h] [-t USER_TARGETS [USER_TARGETS ...]]
              [-u USER_URLS [USER_URLS ...]] [-q USER_QUERY] [--loose]
              [-c CONFIG_FILE [CONFIG_FILE ...]] [-o OUTPUT_FILE]
              [-j OUTPUT_JSON] [-bc BC_PATH] [-sk]
              [-k CLI_APIKEYS [CLI_APIKEYS ...]]
              [-lb LOCAL_BREACH_SRC [LOCAL_BREACH_SRC ...]]
              [-gz LOCAL_GZIP_SRC [LOCAL_GZIP_SRC ...]] [-sf]
              [-ch [CHASE_LIMIT]] [--power-chase] [--hide] [--debug]
              [--gen-config]

Email information and password lookup tool

optional arguments:
  -h, --help            show this help message and exit
  -t USER_TARGETS [USER_TARGETS ...], --targets USER_TARGETS [USER_TARGETS ...]
                        Either string inputs or files. Supports email pattern
                        matching from input or file, filepath globing and
                        multiple arguments
  -u USER_URLS [USER_URLS ...], --url USER_URLS [USER_URLS ...]
                        Either string inputs or files. Supports URL pattern
                        matching from input or file, filepath globing and
                        multiple arguments. Parse URLs page for emails.
                        Requires http:// or https:// in URL.
  -q USER_QUERY, --custom-query USER_QUERY
                        Perform a custom query. Supports username, password,
                        ip, hash, domain. Performs an implicit "loose" search
                        when searching locally
  --loose               Allow loose search by disabling email pattern
                        recognition. Use spaces as pattern seperators
  -c CONFIG_FILE [CONFIG_FILE ...], --config CONFIG_FILE [CONFIG_FILE ...]
                        Configuration file for API keys. Accepts keys from
                        Snusbase, WeLeakInfo, Leak-Lookup, HaveIBeenPwned,
                        Emailrep, Dehashed and hunterio
  -o OUTPUT_FILE, --output OUTPUT_FILE
                        File to write CSV output
  -j OUTPUT_JSON, --json OUTPUT_JSON
                        File to write JSON output
  -bc BC_PATH, --breachcomp BC_PATH
                        Path to the breachcompilation torrent folder. Uses the
                        query.sh script included in the torrent
  -sk, --skip-defaults  Skips Scylla and HunterIO check. Ideal for local scans
  -k CLI_APIKEYS [CLI_APIKEYS ...], --apikey CLI_APIKEYS [CLI_APIKEYS ...]
                        Pass config options. Supported format: "K=V,K=V"
  -lb LOCAL_BREACH_SRC [LOCAL_BREACH_SRC ...], --local-breach LOCAL_BREACH_SRC [LOCAL_BREACH_SRC ...]
                        Local cleartext breaches to scan for targets. Uses
                        multiprocesses, one separate process per file, on
                        separate worker pool by arguments. Supports file or
                        folder as input, and filepath globing
  -gz LOCAL_GZIP_SRC [LOCAL_GZIP_SRC ...], --gzip LOCAL_GZIP_SRC [LOCAL_GZIP_SRC ...]
                        Local tar.gz (gzip) compressed breaches to scans for
                        targets. Uses multiprocesses, one separate process per
                        file. Supports file or folder as input, and filepath
                        globing. Looks for 'gz' in filename
  -sf, --single-file    If breach contains big cleartext or tar.gz files, set
                        this flag to view the progress bar. Disables
                        concurrent file searching for stability
  -ch [CHASE_LIMIT], --chase [CHASE_LIMIT]
                        Add related emails from hunter.io to ongoing target
                        list. Define number of emails per target to chase.
                        Requires hunter.io private API key if used without
                        power-chase
  --power-chase         Add related emails from ALL API services to ongoing
                        target list. Use with --chase
  --hide                Only shows the first 4 characters of found passwords
                        to output. Ideal for demonstrations
  --debug               Print request debug information
  --gen-config, -g      Generates a configuration file template in the current
                        working directory & exits. Will overwrite existing
                        h8mail_config.ini file

```

-----

## Usage examples

###### Query for a single target

```bash
$ h8mail -t target@example.com
```

###### Query for list of targets, indicate config file for API keys, output to `pwned_targets.csv`
```bash
$ h8mail -t targets.txt -c config.ini -o pwned_targets.csv
```

###### Query a list of targets against local copy of the Breach Compilation, pass API key for [Snusbase](https://snusbase.com/) from the command line
```bash
$ h8mail -t targets.txt -bc ../Downloads/BreachCompilation/ -k "snusbase_token=$snusbase_token"
```

###### Query without making API calls against local copy of the Breach Compilation
```bash
$ h8mail -t targets.txt -bc ../Downloads/BreachCompilation/ -sk
```

###### Search every .gz file for targets found in targets.txt locally, skip default checks

```bash
$ h8mail -t targets.txt -gz /tmp/Collection1/ -sk
```

###### Check a cleartext dump for target. Add the next 10 related emails to targets to check. Read keys from CLI

```bash
$ h8mail -t admin@evilcorp.com -lb /tmp/4k_Combo.txt -ch 10 -k "hunterio=ABCDE123"
```
###### Query username. Read keys from CLI

```bash
$ h8mail -t JSmith89 -q username -k "dehashed_email=user@email.com" "dehashed_key=ABCDE123"
```

###### Query IP. Chase all related targets. Read keys from CLI


```bash
$ h8mail -t 42.202.0.42 -q ip -c h8mail_config_priv.ini -ch 2 --power-chase
```

###### Fetch URL content (CLI + file). Target all found emails


```bash
$ h8mail -u "https://pastebin.com/raw/kQ6WNKqY" "list_of_urls.txt"
```


-----
