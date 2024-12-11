# mac osx specific-zsh aliases

Here is a list of my most loved bash / zsh aliases.

## Display the Current WIFI Network Information

Apple keeps changing how you pull this info in the shell, Sometimes, you just need to grab an SSID, especially if it’s ridiculously long or packed with weird characters. This command spits out everything you’d ever want to know about your Mac’s current Wi-Fi status, plus all the SSIDs it can see—no more squinting at your network list.

```shell
#Display Current WIFI Information
alias wifi="system_profiler SPAirPortDataType"
```

### Show Lan IP

Using `ip a` or `ip -c a` (depending on the OS) works for checking network details, but it often outputs a lot of information, especially if Docker is installed on the machine. If you're only interested in your local IP address, this script assumes the interface is `en0` and outputs just the current local IP assigned to that interface.

```shell
#Display the current LAN IP address.
alias lanip="ip a show en0 | awk '/inet / {print \$2}' | cut -d/ -f1 | sed $'s/^/\e[35m/'"
```

### Flushing DNS

Flushing the DNS cache on a Mac can be quite a hassle compared to the straightforward process on Windows, where you simply run `ipconfig /flushdns` in an elevated Command Prompt. Here's how you can do it on a Mac with a simple shell alias.

```shell
# Flush DNS cache on macOS.
alias flushdns='sudo dscacheutil -flushcache && sudo killall -HUP mDNSResponder'
```

### Show WAN IP

Forget Going to the Browser to Get Your WAN IP

```shell
# Display the current WAN IP address.
alias wanip="curl -s ifconfig.me | grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' | sed $'s/^/\e[35m/' | sed $'s/$/\e[0m/'"
```

### What Year is It?

A really quick way to show the full date and time. (Format 'Sun, Jun 05, 2022 12:00:00 PM')

```shell
# Display Date and Time - Format 'Sun, Jun 05, 2022 12:00:00 PM'
alias now='echo -e "\033[35m$(date +'"'%a, %b %d, %Y %l:%M:%S %p'"')\033[0m"'
```
