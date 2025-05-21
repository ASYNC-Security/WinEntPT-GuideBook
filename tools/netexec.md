# netexec

Src: [https://github.com/Pennyw0rth/NetExec](https://github.com/Pennyw0rth/NetExec)

NetExec is probably the tool that I use the most when testing Windows environments, it's a swiss army knife for Windows enumeration and exploitation.

## Installation

NetExec does use Impacket, as well as a ton of other dependencies. It's advised to use `pipx` to install it as it will handle virtual environments for you.

```bash
pipx install git+https://github.com/Pennyw0rth/NetExec --force
```

## Usage

Way too many options to list here, but the most common ones are:

```bash
nxc <protocol> <host> -u '<username>' -p '<password>' VERB
```

Enumerating SMB, with cleartext credentials:

```bash
nxc smb <host> -u '<username>' -p '<password>' --shares
nxc smb dc.lab.local -u 'jess' -p 'P@ssw0rd' --shares
```

Enumerating SMB, with an NTLM hash:

```bash
nxc smb <host> -u '<username>' -H '<nthash>' --shares
nxc smb dc.lab.local -u 'jess' -H 'e19ccf75ee54e06b06a5907af13cef42' --shares
```

Enumerating SMB, with Kerberos authentication:

```bash
getTGT.py 'lab.local'/'jess':'P@ssw0rd'
export KRB5CCNAME='jess.ccache'

nxc smb <host> --use-kcache --shares
nxc smb dc.lab.local --use-kcache --shares
```
