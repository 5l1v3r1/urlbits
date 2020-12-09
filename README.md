# urlbits

![made with go](https://img.shields.io/badge/made%20with-Go-0040ff.svg) ![maintenance](https://img.shields.io/badge/maintained%3F-yes-0040ff.svg) [![open issues](https://img.shields.io/github/issues-raw/drsigned/urlbits.svg?style=flat&color=0040ff)](https://github.com/drsigned/urlbits/issues?q=is:issue+is:open) [![closed issues](https://img.shields.io/github/issues-closed-raw/drsigned/urlbits.svg?style=flat&color=0040ff)](https://github.com/drsigned/urlbits/issues?q=is:issue+is:closed) [![license](https://img.shields.io/badge/License-MIT-gray.svg?colorB=0040FF)](https://github.com/drsigned/urlbits/blob/master/LICENSE) [![twitter](https://img.shields.io/badge/twitter-@drsigned-0040ff.svg)](https://twitter.com/drsigned)

urlbits is a tool to pull out bits from URLS provided on stdin.

## Resources

* [Installation](#installation)
    * [From Binary](#from-binary)
    * [From source](#from-source)
    * [From github](#from-github)
* [Usage](#usage)
* [Credits](#credits)
* [Contribution](#contribution)

## Installation

#### From Binary

You can download the pre-built binary for your platform from this repository's [releases](https://github.com/drsigned/urlbits/releases/) page, extract, then move it to your `$PATH`and you're ready to go.

#### From Source

urlbits requires **go1.14+** to install successfully. Run the following command to get the repo

```bash
$ GO111MODULE=on go get -u -v github.com/drsigned/urlbits/cmd/urlbits
```

#### From Github

```bash
$ git clone https://github.com/drsigned/urlbits.git; cd urlbits/cmd/urlbits/; go build; mv urlbits /usr/local/bin/; urlbits -h
```

## Usage

urlbits works with URLs provided on stdin; they might come from a file like `urls.txt`:

```
$ cat urls.txt

https://sub.example.com/users?id=123&name=Sam
https://sub.example.com/orgs?org=ExCo#about
http://example.net/about#contact
```

You can extract:

* Hostnames from the URLs with the `hostnames` mode:

    ```
    $ cat urls.txt | urlbits hostnames

    sub.example.com
    sub.example.com
    example.net
    ```

* Paths, with the `paths` mode:

    ```
    $ cat urls.txt | urlbits paths

    /users
    /orgs
    /about
    ```

* Query String Keys, with the `keys` mode:

    ```
    $ cat urls.txt | urlbits keys

    id
    name
    org
    ```

* Query String Values, with the `values` mode:

    ```
    $ cat urls.txt | urlbits values

    123
    Sam
    ExCo
    ```

* Query String Key/Value Pairs , with the `keypairs` mode:

    ```
    $ cat urls.txt | urlbits keypairs

    id=123
    name=Sam
    org=ExCo
    ```


* **NOTE:** You can use the `format` mode to specify a custom output format:

    ```
    $ cat urls.txt | urlbits format %d%p

    sub.example.com/users
    sub.example.com/orgs
    example.net/about
    ```

    > For more format directives, checkout the help message `urlbits -h` under `Format Directives`. 
    
    Any characters that don't match a format directive remain untouched:

    ```
    $ cat urls.txt | urlbits -u format "%d (%s)"

    sub.example.com (https)
    example.net (http)
    ```

**Note** that if a URL does not include the data requested, there will be no output for that URL:

```
$ echo http://example.com | urlbits format "%P"

$ echo http://example.com:8080 | urlbits format "%P"
8080
```

To display help message for urlbits use the `-h` flag:

```bash
$ urlbits -h
```

help message:

```text
            _ _     _ _
 _   _ _ __| | |__ (_) |_ ___
| | | | '__| | '_ \| | __/ __|
| |_| | |  | | |_) | | |_\__ \
 \__,_|_|  |_|_.__/|_|\__|___/ v1.0.0

USAGE:
  urlbits [OPTIONS] [MODE] [FORMATSTRING]

GENERAL OPTIONS:
  -u                output unique values
  -v                verbose mode: output URL parse errors

MODE OPTIONS:
  keys              keys from the query string (one per line)
  values            values from the query string (one per line)
  keypairs          `key=value` pairs from the query string (one per line)
  hostnames         the hostname (e.g. sub.example.com)
  paths             the request path (e.g. /users)
  format            specify a custom format (see below)

FORMAT DIRECTIVES:
  %%                a literal percent character
  %s                the request scheme (e.g. https)
  %u                the user info (e.g. user:pass)
  %h                the hostname (e.g. sub.example.com)
  %S                the subdomain (e.g. sub)
  %r                the root of domain (e.g. example)
  %t                the TLD (e.g. com)
  %P                the port (e.g. 8080)
  %p                the path (e.g. /users)
  %q                the raw query string (e.g. a=1&b=2)
  %f                the page fragment (e.g. page-section)
  %@                inserts an @ if user info is specified
  %:                inserts a colon if a port is specified
  %?                inserts a question mark if a query string exists
  %#                inserts a hash if a fragment exists
  %a                authority (alias for %u%@%d%:%P)

EXAMPLES:
  cat urls.txt | urlbits keys
  cat urls.txt | urlbits format %s://%h%p?%q
```

## Credits

All credits to [Tom Hudson](https://github.com/tomnomnom), i took the initial code from his [unfurl](https://github.com/tomnomnom/unfurl).
## Contibution

[Issues](https://github.com/drsigned/urlbits/issues) and [Pull Requests](https://github.com/drsigned/urlbits/pulls) are welcome! 