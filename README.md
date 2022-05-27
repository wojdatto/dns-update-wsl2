# `dns-update-wsl2`

Perl script that automatically updates WSL2 `resolv.conf` file based on
DNS servers extracted from `ipconfig.exe /all` output. Cloudflare
`1.1.1.1` DNS is always added as the last server.

The script might be useful when your DNS servers often change from any
reason, and you cannot count on `resolv.conf` being auto-generated by
WSL2 (because it can be wrong when using corporate network etc.).

E.g. you might be working from home via the VPN, and then from the
office with different DNS servers in these two situations.

Before using the script, you have to turn off the auto-generation of
`resolv.conf` file. It can be done by placing the following in
`etc/wsl.conf` in your WSL distro:

```plaintext
[network]
generateResolvConf = false
```

See more information about that in [Microsoft
Docs](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#network-settings).

## Limitations

Script is able to only detect IPv4 DNS servers.

## How to use?

Obtain `dns-update-wsl2` script in any preferred way, and just run it.
The generated file will be presented to you with the choice to abort or
proceed. With choosing the latter option, you will be prompted for your
password, because `sudo` is required to move the file to `/etc`.

You might want to place the script on your `PATH` to have an easier
access to it.

Example output:

```plaintext
$ ./dns-update-wsl2
Temporary resolv.conf generated:

# Auto-generated from ipconfig.exe
# on Mon Apr 11 20:13:13 2022
nameserver 123.456.789.37
nameserver 9.8.7.6
nameserver 1.1.1.1

Update 'etc/resolv.conf'? (y/N): y
Using sudo to update '/etc/resolv.conf'...
Successfully updated '/etc/resolv.conf'!
```

## `sudo`

Running the script as `root` or with `sudo` will fail because for some
reason `ipconfig.exe` doesn't return any output in such case. This
script prompts for sudo later on its own, and thus should be called with
the standard user privileges.

Running the script with `root` privileges will cause it to exit at the
very beginning.

## License

> Copyright 2022 Tomasz Wojdat
>
> Licensed under the Apache License, Version 2.0 (the "License"); you
> may not use this file except in compliance with the License. You may
> obtain a copy of the License at
>
>     http://www.apache.org/licenses/LICENSE-2.0
>
> Unless required by applicable law or agreed to in writing, software
> distributed under the License is distributed on an "AS IS" BASIS,
> WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
> implied. See the License for the specific language governing
> permissions and limitations under the License.
