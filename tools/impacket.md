# impacket

Src: [https://github.com/fortra/impacket](https://github.com/fortra/impacket)

Impacket is by far one of the most widely used libraries by other tools. However, most of these tools package their own version of Impacket within isolated virtual environments, so compatibility issues are rarely a concern.

When people mention using "Impacket", they are likely referring to the Impacket [Examples](https://github.com/fortra/impacket/tree/master/examples) directory, which contains a number of useful scripts for various tasks: such as
* [smbclient.py](https://github.com/fortra/impacket/blob/master/examples/smbclient.py)
* [psexec.py](https://github.com/fortra/impacket/blob/master/examples/psexec.py)
* [secretsdump.py](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)

You can view versioning information at the printed banner on every script. For example, running `secretsdump.py` will show the version of Impacket being used - which in this case corresponds to [this](https://github.com/fortra/impacket/commit/27bebb1347569fa810e432326266acf17560f274) commit.

```bash
secretsdump.py                                                    
Impacket v0.13.0.dev0+20250422.104055.27bebb13 - Copyright Fortra, LLC and its affiliated companies 

usage: secretsdump.py [-h] [-ts] [-debug] [-system SYSTEM] [-bootkey BOOTKEY] [-security SECURITY] [-sam SAM] [-ntds NTDS] [-resumefile RESUMEFILE] [-skip-sam] [-skip-security] [-outputfile OUTPUTFILE] [-use-vss] [-rodcNo RODCNO]
                      [-rodcKey RODCKEY] [-use-keylist] [-exec-method [{smbexec,wmiexec,mmcexec}]] [-use-remoteSSMethod] [-remoteSS-remote-volume REMOTESS_REMOTE_VOLUME] [-remoteSS-local-path REMOTESS_LOCAL_PATH]
                      [-just-dc-user USERNAME] [-ldapfilter LDAPFILTER] [-just-dc] [-just-dc-ntlm] [-skip-user SKIP_USER] [-pwd-last-set] [-user-status] [-history] [-hashes LMHASH:NTHASH] [-no-pass] [-k] [-aesKey hex key]
                      [-keytab KEYTAB] [-dc-ip ip address] [-target-ip ip address]
                      target
```

# Installation

It's recommended to install from source, via `pipx` as the versions shipped with pentesting distros (such as kali) are often dated to the latest stable release and `pipx` will handle virtual environments for you.

```
pipx install git+https://github.com/fortra/impacket --force
```

# Syntax

Impacket syntax is pretty standard, and most tools that use the Impacket library also use the same syntax.

## Cleartext Credentials

```bash
psexec.py '<domain>'/'<username>':'<password>'@<fqdn>
psexec.py 'lab.local'/'jess':'P@ssw0rd'@'dc.lab.local'
```

This also extends for cross-forest authentication.

```
psexec.py 'corp.local'/'test':'P@ssw0rd'@'dc.lab.local'
```

## NTLM Hashes

You can also use NTLM hashes instead of cleartext credentials, do note that you can simply specify an empty LM hash.

```bash
psexec.py '<domain>'/'<username>'@<fqdn> -hashes ':<nthash>'
psexec.py 'lab.local'/'jess'@'dc.lab.local' -hashes ':e19ccf75ee54e06b06a5907af13cef42'
```

## Kerberos Authentication

The script will grab the Kerberos cache currently set in the `KRB5CCNAME` environment variable, the most common way to obtain this is via `getTGT.py`. You can specify Kerberos authentication using `-k`, the `-no-pass` flag is also useful to shut up the script for interactive password prompts.

```bash
getTGT.py '<domain>'/'<username>':'<password>'
getTGT.py 'lab.local'/'jess':'P@ssw0rd'

psexec.py -k -no-pass dc.lab.local
```

Do note that most of the time, you _do not_ need to explicitly specify the domain and username as the script will refer to those in the Kerberos ticket.