# VHostScan
A virtual host scanner that can pivot over hosts, detect catch-all scenarios, aliases and dynamic default pages.

[![Python 3.2|3.6](https://img.shields.io/badge/python-3.2|3.6-green.svg)](https://www.python.org/) [![License](https://img.shields.io/badge/license-GPL3-_red.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html) [![Twitter](https://img.shields.io/badge/twitter-@____timk-blue.svg)](https://twitter.com/__timk) [![Twitter](https://img.shields.io/badge/twitter-@codingo__-blue.svg)](https://twitter.com/codingo_)

## Key Benefits

* Quickly highlight unqiue content in catch-all scenarios
* Locate the outliers in catch-all scenarios where results have dynamic content on the page (such as the time)
* Identify aliases by tweaking the unique depth of matches
* Wordlist supports standard words and a variable to input a base hostname (for e.g. dev.%s from the wordlist would be run as dev.BASE_HOST)
* Work over HTTP and HTTPS
* Ability to set the real port of the webserver to use in headers when pivoting through ssh/nc
* Add simple response headers to bypass some WAF products

## Product Comparisons

![VHOSTScan Feature Map](https://github.com/codingo/codingo.github.io/blob/master/assets/featureMap.PNG)

# Usage

| Argument        | Description |
| ------------- |:-------------|
| -h, --help | Display help message and exit |
| -t TARGET_HOSTS | Set the target host. |
| -b BASE_HOST   | Set host to be used during substitution in wordlist (default to TARGET).|
| -w WORDLIST | Set the wordlist to use for generated commands. Ex /usr/share/wordlist.txt |
| -p PORT  | Set the port to use (default 80). |
| -r REAL_PORT | The real port of the webserver to use in headers when not 80 (see RFC2616 14.23), useful when pivoting through ssh/nc etc (default to PORT). |
| --ignore-http-codes IGNORE_HTTP_CODES | Comma separated list of http codes to ignore with virtual host scans (default 404). |
| --ignore-content-length IGNORE_CONTENT_LENGTH | Ignore content lengths of specificed amount. |
| --unique-depth UNIQUE_DEPTH | Show likely matches of page content that is found x times (default 1). |
| --ssl | If set then connections will be made over HTTPS instead of HTTP. |

## Usage Examples
### Quick Example
The most straightforward example runs the default wordlist against example.com using the default of port 80:

```bash
$ VHostScan.py -t example.com
```
### Port forwarding
Say you have an SSH port forward listening on port 4444 fowarding traffic to port 80 on example.com's development machine. You could use the following to make VHostScan connect through your SSH tunnel via localhost:4444 but format the header requests to suit connecting straight to port 80:

```bash
$ VHostScan.py -t localhost -b example.com -p 4444 -r 80
```

### STDIN
If you want to pipe information into VHostScan you can use the ```-``` flag:
```bash
$ cat vhostname | VHostScan.py -t localhost -
```
### STDIN and WordList
You can still specify a wordlist to use along with stdin. In these cases wordlist information will be appended to stdin. For example:
```bash
$ cat vhostname | VhostScan.py -t localhost -w ./wordlists/wordlist.txt -
```
