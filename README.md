# IPv4 vs IPv6 for DevOps Engineers

A 60-minute workshop that de-mystifies IPv6 for people who ship infrastructure — not
protocol designers. Read hex without panic, drop the NAT tax, understand why
*routable ≠ reachable*, and build a dual-stack link with your own hands.

> "Don't panic. It's just more numbers."

## What's in the bundle

| File | What it is |
|------|-----------|
| `ipv4-vs-ipv6-workshop.pptx` | The 12-slide talk (~40 min), timed section by section |
| `ipv4-vs-ipv6-lab-workbook.docx` | Self-contained lab + field guide, fill-in exercises, cheat sheet, full answer key |
| `ipv4-vs-ipv6-errata-and-further-reading.docx` | Optional deep dives (IPsec, death of broadcast, history) + RFC index |
| `lab/ipv6-lab.clab.yml` | The containerlab topology you deploy in the lab |
| `lab/srl1.cli`, `lab/srl2.cli` | Complete per-router configs — the lab answer key; optionally pre-loaded to demo the finished state |
| `ipv6-ipv4-timeline.{svg,png}` | Timeline diagram — from RFC 791 to IPv4 exhaustion |

## The lab

Two Nokia SR Linux routers, back-to-back, one **dual-stack** link. You bring the link
up in both address families, add static routes, and connect two "server" subnets —
proving IPv6 routes exactly like IPv4, with no NAT anywhere in the path.

```
  server-a                                    server-b
2001:db8:a::/64                             2001:db8:b::/64
10.1.1.0/24                                 10.2.2.0/24
   (lo0 on srl1)                               (lo0 on srl2)
      │                                           │
   ┌──────┐  e1-1   2001:db8:12::/64   e1-1  ┌──────┐
   │ srl1 │══════════════════════════════════│ srl2 │
   └──────┘         192.168.12.0/30          └──────┘
```

### Prerequisites
- A Linux host with Docker and [containerlab](https://containerlab.dev)
- The SR Linux image: `docker pull ghcr.io/nokia/srlinux:latest`

### Run it

```bash
# deploy
sudo containerlab deploy -t lab/ipv6-lab.clab.yml

# hop onto a router
docker exec -it clab-ipv6-lab-srl1 sr_cli

# tear down
sudo containerlab destroy -t lab/ipv6-lab.clab.yml --cleanup
```

Then follow **Section 3** of the workbook top to bottom. The `✎` marks are blanks to
fill in; Section 6 is the answer key.

## The four takeaways

1. **It's just more numbers.** Two rules tame any address — hex isn't the enemy.
2. **No more NAT tax.** Global addresses + egress-only gateways = privacy, no translation.
3. **Routable ≠ reachable.** Security is the firewall / Security Group, never IP hiding.
4. **Design for dual-stack.** Both families, every rule and listener — cloud and k8s already are.

## Audience

Aspiring DevOps engineers with basic networking knowledge. ~20 min of hands-on lab
inside a 60-min session.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — reuse, remix, and teach
from any of it, including commercially; just give credit. See [LICENSE](LICENSE).

---
Addresses use `2001:db8::/32`, the RFC 3849 documentation prefix — safe for labs and
slides, never routable on the real internet.
