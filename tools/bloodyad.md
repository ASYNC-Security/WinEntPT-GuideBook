# bloodyad

Src: [https://github.com/CravateRouge/bloodyAD](https://github.com/CravateRouge/bloodyAD)

## Installation

Do note that `bloodyAD` uses Impacket, it's advised to use `pipx` to install it as it will handle virtual environments for you.

```bash
pipx install git+https://github.com/CravateRouge/bloodyAD --force
```

## Usage

Although `bloodyAD` uses Impacket, they do not use the Impacket-style syntax of `<domain>/<username>:<password>@<fqdn>`. Instead, they use the more common `-u` and `-p` flags.

```bash
bloodyAD --host '<fqdn>' -u '<username>' -p '<password>' VERB
bloodyAD --host 'dc.lab.local' -u 'jess' -p 'P@ssw0rd' get writable
```

If you do not specify a domain with `-d`, it will default to the domain returned by the `--host`. This is particularly important when doing cross-forest authentication.

```bash
bloodyAD --host 'dc.lab.local' -u 'test' -p 'P@ssw0rd' -d 'corp.local' get writable
```

Unlike Impacket, when doing Kerberos authentication, you do need to specify a username but no password. Similarly, they use the `KRB5CCNAME` environment variable to specify the Kerberos cache.

```bash
bloodyAD --host 'dc.lab.local' -u 'jess' -k get writable
```

If a password is specified, the script will request a TGT using the specified credentials (similar to `getTGT.py`), and then use that TGT to authenticate to the target host.

```bash
bloodyAD --host 'dc.lab.local' -u 'jess' -p 'P@ssw0rd' -k get writable
```