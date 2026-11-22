# VPS for PBN: How Many Sites Fit on One Server, How Many IPs You Really Get, and Which Plan to Order

Most PBN hosting mistakes come from one wrong assumption about IP addresses. A VPS is a single machine with one or a few addresses assigned by the provider, so putting eight WordPress sites on it gives you eight sites behind the same address unless you order additional ones. Upgrading to a bigger box doesn't change that. Ordering separate small boxes across separate networks usually does.

What follows is the spec floor a PBN actually needs, how to estimate site count from RAM and traffic instead of domain count, the footprint rules that matter at the IP, ASN, DNS and datacenter level, and what to confirm on the order page before you pay. Ava Hosting's live VPS plan list and prices are linked wherever they become relevant, since those numbers change and the order page is the only place worth trusting.

## Quick pick

> **2–4 light WordPress sites, one country:** 2 vCPU / 4 GB RAM with NVMe storage is enough. Buy the smallest tier that clears 4 GB.
> **5–10 sites, each needing its own IP:** three or four small VPS across different providers and regions beats one large VPS. Same total spend, far less shared infrastructure.
> **PBN plus Scrapebox or GSA SER on the same box:** 8–16 GB RAM. A 1 GB plan will fall over the first time a crawler runs.
>
> 👉 [See Ava Hosting's current VPS plans and RAM tiers](https://my.ava.hosting/aff.php?aff=15562)

## Sizing a PBN VPS by workload, not by domain count

The useful way to shop is to start from what you're hosting and work back to a spec. The table below maps four common setups to the resources they need. Every plan link goes to Ava Hosting's order area, where the current configuration and price for each tier is listed.

| What you're hosting | vCPU / RAM | Disk | IPs required | Where to start |
| --- | --- | --- | --- | --- |
| 1–2 light WordPress sites, little traffic | 1–2 vCPU / 2–4 GB | 20–40 GB NVMe | 1 per site | [Check Ava Hosting's entry VPS tier](https://my.ava.hosting/aff.php?aff=15562) |
| 3–5 sites, some caching and plugins | 2 vCPU / 4 GB | 40–60 GB NVMe | 1 per site, ideally different subnets | [Compare Ava Hosting's mid-range VPS plans](https://my.ava.hosting/aff.php?aff=15562) |
| 6–10 sites, mixed traffic | 4 vCPU / 8 GB, or several smaller boxes | 80 GB+ NVMe | 1 per site across separate networks | [View Ava Hosting's larger VPS configurations](https://my.ava.hosting/aff.php?aff=15562) |
| PBN plus Scrapebox, GSA SER, custom crawlers | 4–8 vCPU / 8–16 GB | 100 GB+ NVMe | Multiple, often rotating | [See Ava Hosting's high-RAM VPS options](https://my.ava.hosting/aff.php?aff=15562) |

Two things about that table are worth spelling out, because they're where most buying decisions go wrong.

**The RAM floor is higher than the entry price suggests.** A survey of 157 VPS providers published in October 2026 found the median plan price at $6.00/month and the cheapest at $0.48/month, with entry plans commonly sitting at 1 GB RAM in the $1.75–$2.95 range. Those plans are legitimately the minimum needed to run a VPS. Guidance for PBN hosting and SEO tooling puts the working figure at 8–16 GB for anything beyond a couple of sites, with 2–4 vCPU and NVMe storage as the baseline. Both numbers are correct; they answer different questions. A 1 GB box will boot, serve a brochure site, and then page to disk the moment WordPress runs a plugin update across five installs.

**The cheapest listed price often excludes the IP you need.** The $0.48/month plan in that same survey bills IPv4 separately, which is increasingly normal as address space runs out. If your PBN plan depends on one address per site, the per-IP charge belongs in your budget from the start, not as a surprise at checkout.

## How many PBN sites actually fit on one VPS?

Resource limits decide this, and they're set by the sites, not by how many domains you own. Ten lightweight sites with static pages and no plugins can demand less from a server than two heavy sites running page builders, form plugins, WooCommerce and a bloated database. That's why sizing by domain count misleads people into either overspending or buying a box that dies at site six.

Six variables set the real ceiling:

- **Available CPU and RAM**, minus whatever the control panel and database consume at idle
- **Disk space and disk type**, since MySQL on spinning or network storage slows every page load
- **Traffic per site**, which matters more than total network traffic because PHP workers are per-request
- **Themes and plugins**, the single biggest uncontrolled variable across a network
- **Caching and web server configuration**, which can cut RAM demand by half or double it
- **Database size and background processes**, including cron jobs and backups

There's a second cost to stacking sites on one machine that doesn't show up in any resource graph. Every site on that VPS shares the same account, the same server, the same provider and often the same network path. A failed disk, a botched kernel update, a suspension over an abuse complaint or a provider-side outage takes all of them offline at once. If those sites feed links to money sites, the whole network goes quiet together. Four small VPS in four providers cost roughly what one mid-range box costs and remove most of that shared failure surface.

## IP, C-class, ASN, DNS and datacenter: where a footprint actually forms

Each site in a PBN should sit on its own IP address, and those addresses should belong to different C-classes and different autonomous systems, so the network doesn't resolve back to a single owner. That's the standard, and it's only half the job: separate IPs on the same provider, the same subnet or the same ASN are still visibly related, and moving from shared hosting to a VPS doesn't erase connections that already exist at the DNS or nameserver level.

A checklist that actually separates sites:

1. **One IP per site, on different subnets.** Ask before ordering whether additional IPv4 addresses are available and on which subnets. Some providers sell extras from a single /24, which defeats the purpose.
2. **Different ASNs.** Two IPs from two ranges owned by the same hosting company still point at one organization. Spread registrations across unrelated providers.
3. **Different nameservers.** Use the provider's NS only where they differ per IP, or third-party DNS with distinct records per site. Identical nameserver pairs across ten sites are a cleaner signal than shared IPs.
4. **Different countries and datacenters.** Sites that all sit in Frankfurt look like one operation regardless of IP math.
5. **Different CMS configurations.** Same theme, same plugin set, same permalink structure and same footer across a network is a content-level footprint that no amount of IP diversification hides.
6. **Clean neighbors.** A dedicated VPS keeps your address away from other tenants' behaviour, which is the one advantage shared hosting can't offer.

None of this is expensive. It's mostly a matter of ordering from more than one place instead of buying the biggest plan on a single invoice.

## VPS vs shared vs cloud for a PBN

Shared hosting puts many accounts on one physical server, pools CPU, RAM and bandwidth, and leaves configuration to the provider. It's the cheapest option and it's fine for a small network of light WordPress sites with limited traffic. The trade-offs are real: you can't change server-level settings, your performance depends on your neighbors, and hundreds of sites sharing one IP means one aggressive tenant can get that IP blacklisted for everyone.

A VPS slices a physical server into isolated environments with their own operating system and allocated resources. You control the software stack, the database, the firewall and the security configuration, and IP allocation is decided by the provider and plan rather than guaranteed per site.

Cloud hosting draws from a resource pool and scales up or down without migrating, and some setups add redundancy across infrastructure. It also brings usage-based billing and more configuration work. For a small PBN with predictable load, that flexibility usually buys nothing you'll use.

The honest summary: shared hosting wins on cost and simplicity for small light networks, a VPS wins when you need control over the stack and cleaner IPs, and cloud only pays off when elasticity is a genuine requirement.

## From order to first live site

1. **Deploy and get root access.** Automated provisioning on a KVM or similar plan takes minutes; you should receive root credentials immediately.
2. **Pick a stack.** Apache, Nginx or LiteSpeed, plus a control panel if you want one (cPanel and Plesk are paid; open-source panels cost nothing but time). Add a firewall and a malware scanner before any site goes live.
3. **Order additional IPs and set custom nameservers.** Do this before installing WordPress so each site is born on its own address, not migrated later.
4. **Install the CMS.** Most PBNs run WordPress. Vary themes and plugin sets deliberately as you deploy.
5. **Issue SSL on every domain.** Free certificates cover this; a PBN with mixed HTTP and HTTPS stands out.
6. **Automate backups and monitoring.** Off-server backups, uptime checks and a resource-usage alert so you find out about a memory spike before your host does.

If you'd rather start with the smallest tier and scale as sites come online, 👉 [open Ava Hosting's VPS order page and check the current plan list](https://my.ava.hosting/aff.php?aff=15562).

## What to confirm on the order page before paying

Prices on hosting order pages change often, and the only version that matters is the one you see at checkout. Four items are worth checking explicitly:

- **The renewal price.** Promotional first-term pricing is common. If the order page shows a discounted initial period and a standard rate afterward, budget the second year at the higher figure.
- **Monthly vs annual billing.** Annual terms usually cut the effective monthly cost, but they also lock in a provider you may want to leave after testing.
- **The price of extra IPv4 addresses.** Ask how many are available per VPS and whether they come from different subnets. This single answer can decide whether a provider works for a PBN at all.
- **The acceptable use policy.** Some hosts restrict SEO link networks or bulk tooling. Read the AUP before you build, not after a suspension notice.

Ava Hosting's plan details, configuration options and current pricing are on its own order pages, which is where those four answers live. 👉 [Check Ava Hosting's VPS configurations and pricing directly](https://my.ava.hosting/aff.php?aff=15562)

## FAQ

**Can I host multiple PBN sites on one VPS?**
Yes, if the server has the resources. Whether it's a good idea depends on how much each site consumes and whether you can get separate IPs for them. Ten light sites on one box is technically fine and strategically weak, because they share an account, a server and a failure point.

**Does each site get its own IP address automatically?**
No. A VPS normally comes with one or a few addresses assigned by the provider, and multiple websites share them unless you configure additional ones. Some providers sell extra IPv4 addresses, some don't, and the subnet they come from matters as much as the count.

**Is shared hosting enough for a small PBN?**
For a handful of lightweight WordPress sites with modest traffic, shared hosting can be cheaper and simpler. It stops working when you need server-level control, when neighbors' behaviour affects your IP reputation, or when you want each site on an address you chose.

**Will moving to a VPS remove my footprint?**
No. Footprints form from related IPs, hosting networks, DNS settings and nameservers. Changing hosting type leaves those connections in place unless you deliberately separate addresses, ASNs, nameservers and locations.

**How much RAM do I need per PBN site?**
There's no fixed per-site figure. A cached, plugin-light WordPress site runs comfortably in 256–512 MB; a heavy one with page builders and WooCommerce can want 1 GB on its own. Start from 4 GB for a small network, 8–16 GB if you're also running crawlers or SEO tools on the same machine.

## Which setup fits your network

Match the order to the network you're actually running:

- **Two or three light sites, one market:** the smallest Ava Hosting VPS tier that clears 4 GB RAM. Paying for 16 GB here is wasted money.
- **Six to ten sites that need distinct IPs:** three or four small VPS from different providers and countries, each on its own subnet. Same budget as one large machine, better separation.
- **PBN plus crawlers and rank-tracking scripts:** a single 8–16 GB box is reasonable, since the tools benefit from shared CPU and you're not relying on it for footprint diversity.
- **A network you plan to grow past twenty sites:** budget for per-IP charges from day one. Address costs, not server costs, will dominate the bill.

Before you order anywhere, get two answers in writing: how many additional IPv4 addresses you can buy and from which subnets, and whether PBN or SEO link-network use is permitted under the acceptable use policy. Those two determine whether a cheap plan is usable or a trap.

👉 [Start with Ava Hosting's VPS plans and pick the RAM tier your site count needs](https://my.ava.hosting/aff.php?aff=15562)
