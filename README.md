# syntaxhighlight

go
```go
package main

import "fmt"
import "time"

func main() {
    go spinner(100 * time.Millisecond)
    const n = 45
    fibN := fib(n) // slow
    fmt.Printf("\rFibonacci(%d) = %d\n", n, fibN)
}

func spinner(delay time.Duration) {
    for {
        for _, r := range `-\|/` {
            fmt.Printf("\r%c", r)
            time.Sleep(delay)
        }
    }
}


func fib(x int) int {
    if x < 2 {
      return x
    }
    return fib(x-1) + fib(x-2)
}
```
og

bash
```bash
#!/usr/bin/env bash

set -o errexit
set -o nounset
set -o pipefail

OLD_IFS=$IFS
IFS=$'\r\n'
interfaces=($(networksetup -listallnetworkservices | tail +2))
IFS=$OLD_IFS
route_if=""
for ((i = 0; i < ${#interfaces[@]}; i++)); do 
  if networksetup -getinfo "${interfaces[$i]}" | \
       awk -F': ' '/Router/ {print $2}'|
       grep -qE '\d+\.\d+\.\d+\.\d+'; then
    route_if="${interfaces[$i]}" 
  fi
done

net_if=${1:-"$route_if"}
if [[ -z "$net_if" ]]; then
  echo "Unable to determine network interface, please provide it as the first argument"
  exit 1
fi

if [ "$EUID" != 0 ]; then
  echo "Please run this script as root (or via sudo)"
  exit 1
fi

if pgrep dnsmasq >/dev/null 2>&1; then 
  if sudo -u "$SUDO_USER" brew services list | grep -qE 'dnsmasq.*stopped'; then
    echo "Found a running dnsmasq process that's not managed by homebrew, please kill it before running this"
    # shellcheck disable=SC2016
    echo 'ex: sudo kill $(pgrep dnsmasq)'
    exit 1
  fi
fi

# Store original nameservers so we can try to restore them later if needed
if [ ! -f "$HOME/.dns_servers.orig" ]; then
  networksetup -getdnsservers "$net_if" > "$HOME/.k8s_dns_servers.orig"
fi

if ! sudo -u "$SUDO_USER" brew list dnsmasq >/dev/null 2>&1; then
  echo "Please install dnsmasq via Homebrew as a non-root user: brew install dnsmasq"
fi

cat <<-EOF > /usr/local/etc/dnsmasq.conf
no-poll
no-hosts
max-cache-ttl=60
server=/svc.admin.us-east-2.aws.k8s/100.66.0.10
server=/svc.stage.us-west-2.aws.k8s/100.65.0.10
server=/svc.prod.us-west-1.aws.k8s/100.64.0.10
server=/admin.us-east-2.aws.k8s/10.4.0.2
server=/stage.us-west-2.aws.k8s/10.33.0.2
server=/prod.us-west-1.aws.k8s/10.1.0.2
server=/us-west-1.postmates.com/10.1.0.2
server=/postmates.prod-us-west-1/10.1.0.2
server=/postmates.stage-us-west-1/10.1.0.2
server=8.8.8.8
EOF

brew services restart dnsmasq

networksetup -setdnsservers "$net_if" 127.0.0.1 8.8.8.8
```
bash
