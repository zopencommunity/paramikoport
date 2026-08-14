# paramiko

[paramiko](https://www.paramiko.org/) for z/OS: an SSHv2 implementation in pure
Python, covering clients, servers, SFTP and key handling.

## Install

```sh
zopen install paramiko
```

or, into a virtual environment:

```sh
export PIP_EXTRA_INDEX_URL="https://repo.zopen.community/pypi/wheels/simple/"
export PIP_CONSTRAINT="https://repo.zopen.community/pulp/content/constraints/zopen-constraints.txt"
pip install paramiko
```

See [Using Python packages on z/OS](https://zopen.community/#/Guides/PythonPackages)
for why both settings are needed.

## Notes for this platform

paramiko has no C code of its own. Everything platform-specific is in its
dependencies, which is why this port arrived only after
[cryptography](https://github.com/zopencommunity/cryptographyport),
[bcrypt](https://github.com/zopencommunity/bcryptport) and
[pynacl](https://github.com/zopencommunity/pynaclport) were building — and
pynacl in turn needed
[libsodium](https://github.com/zopencommunity/libsodiumport).

Ed25519 keys go through pynacl, so they exercise that whole chain. The port's
tests run a real SSHv2 exchange over a loopback socket with an Ed25519 host key
— key exchange, host key verification, password authentication, and data across
an encrypted channel — rather than only importing the module, since an import
proves nothing about whether the crypto underneath it works here.
