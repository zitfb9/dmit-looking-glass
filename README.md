# DMIT Looking Glass: Test Real Network Performance Before You Buy, Lock In Up To 45% Recurring Discounts

If you've ever bought a VPS based on a slick marketing page only to find out the "premium network" routes your traffic through a congested path at 9 PM peak hours, you already know why a Looking Glass matters. A Looking Glass is the one tool that lets you peek behind the curtain—run ping, traceroute, MTR, and iperf3 tests from a provider's actual server before you spend a cent. And when the provider in question is DMIT, whose entire reputation rests on CN2 GIA and CMIN2 routing into mainland China, the Looking Glass isn't a nice-to-have. It's the deciding factor.

This article walks through how DMIT's Looking Glass works, what the test results actually tell you across its Los Angeles, Hong Kong, and Tokyo locations, and how to pair what you learn with the currently active promo codes so you don't just test the network—you lock in a recurring discount on it.

## What DMIT's Looking Glass Actually Shows You

DMIT runs a public Looking Glass at lg.dmit.sh, and it covers every routing profile the company sells. That matters more than it sounds, because DMIT doesn't just sell "a Los Angeles VPS." It sells three distinctly different networks out of the same data center, and they perform nothing alike.

The Looking Glass lets you select a server—say, Los Angeles (Pro), Los Angeles (Eyeball), or Los Angeles (Tier 1)—and from that node run four classic diagnostics:

- **Ping** — raw round-trip latency from the DMIT server to any IP you enter. This is your baseline. If you're in Shanghai and pinging the LAX Pro node, you want to see something in the 140–150ms range for CN2 GIA. If you're seeing 200ms+, the route isn't doing what you hoped.
- **Traceroute** — hop-by-hop path. This is where you see whether your traffic rides China Telecom's CN2 backbone or gets dumped onto a generic transit path. The hop names tell the story; CN2 GIA routes show `*-g2-cn2.gtmans.*` style hostnames.
- **MTR** — traceroute on steroids, combining path discovery with loss and latency statistics per hop over time. MTR is the one to watch for peak-hour stability. A route that looks clean on a single traceroute can still show 2% packet loss on the China-side hop during evening hours, and MTR exposes that.
- **iperf3** — bandwidth testing directly from the DMIT node to your own server (or to another public iperf3 endpoint). This tells you whether the "10Gbps port" on a LAX Pro.STARTER actually delivers, or whether the upstream is constrained.

On top of those, each Looking Glass node exposes test files in 100MB, 1GB, and 5GB sizes over both IPv4 and IPv6. Downloading one of those from your own connection is the single fastest way to sanity-check real-world throughput before committing to a plan.

## The Three Networks, Seen Through The Looking Glass

This is the part most guides skip. DMIT's three routing profiles aren't marketing labels—they're physically different transit blends, and the Looking Glass makes the difference visible within about thirty seconds of testing.

**Premium (Pro)** combines Tier 1 transit with DMIT's own backbone and China Telecom CN2 GIA. Run an MTR from the LAX Pro node toward a mainland IP and you'll see the path stay on CN2 GIA end-to-end. This is the profile to buy when the user experience inside China is the whole point—lower latency, fewer hops, materially less packet loss at peak. The Looking Glass test IP for LAX Pro is `179.255.100.100`, with IPv6 at `2605:52c0:2:d96:942f:3fff:fedf:1746`.

**Eyeball (EB)** pairs Tier 1 transit with "reasonable effort" China routing via CMIN2 and similar Chinese eyeball ISPs. On the Looking Glass, you'll see CN2 out for Telecom/Unicom and CMIN2 return for all three carriers. It's not the pure premium path, but it's noticeably better than vanilla international transit, and the price reflects that—it sits between Pro and Tier 1.

**Tier 1 (T1)** is straight international routing—RETN out of Hong Kong, standard transit out of LA and Tokyo—optimized for Asia-America and intra-Asia latency with no China-specific work. The Looking Glass will show generic transit hostnames, no CN2 hops. This is the cheapest tier and the right pick when your audience isn't in mainland China.

If you're choosing between these, don't read the spec sheets first. Open the Looking Glass, pick the city closest to your users, run an MTR toward a target IP in your audience's region from each of the three profiles, and compare the loss and latency columns. The "right" network will be obvious inside five minutes.

## Running A Real Test: A Worked Example

Here's what a sensible pre-purchase test looks like, using the Hong Kong Pro node as the example because Hong Kong is where CN2 GIA's latency advantage is most dramatic.

1. Open 👉 [DMIT Looking Glass](https://bit.ly/DMIt) and select `Hong Kong (Pro)`. The test IP for that node is `191.222.216.216` on IPv4.
2. Run a **ping** to a mainland China target (your own IP, or a known endpoint). Expect ~30–40ms to Guangzhou on CN2 GIA. If you see that, the route is doing its job.
3. Run **MTR** to the same target with ~100 cycle count. Look at the China-side hops—the loss column should stay at 0.0% across all hops. Any sustained loss on the last two hops means the route is congested, not premium.
4. Run **iperf3** in reverse mode from your own server to the Looking Glass's iperf3 endpoint. The HKG Pro node is on a 1Gbps port, so you should see close to that on a clean path.
5. Download the **1GB test file** over IPv4 from the Looking Glass page using `curl -o /dev/null -w "%{speed_download}\n" <url>` from your own server. Real-world MB/s is the number that matters for actual workloads.

Do the same for the LAX Pro and TYO Pro nodes if you're undecided on location. The whole exercise takes maybe fifteen minutes and removes essentially all the guesswork from the purchase.

## DMIT Plan Pricing, By Location And Profile

Below is the current plan lineup pulled from DMIT's pricing page, grouped by location and routing profile. Prices are standard (pre-coupon); the promo codes in the next section stack on top of these.

### Los Angeles

| Plan | CPU | RAM | Storage | Bandwidth | Traffic | Price (monthly) | Price (annual) | Order |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| LAX Pro.TINY | 1 vCore | 2GB | 20GB SSD | 1Gbps | 1TB/mo | $9.90 | $88.88 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=167) |
| LAX Pro.POCKET | 2 vCores | 2GB | 40GB SSD | 4Gbps | 1.5TB/mo | $14.90 | $159.98 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=168) |
| LAX Pro.STARTER | 2 vCores | 2GB | 80GB SSD | 10Gbps | 3TB/mo | $29.90 | $322.99 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=169) |
| LAX EB.TINY | 1 vCore | 2GB | 20GB SSD | 2Gbps | 1.2TB/mo | $6.90 | $74.88 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=183) |
| LAX EB.POCKET | 1 vCore | 2GB | 40GB SSD | 4Gbps | 2TB/mo | $12.90 | $139.90 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=184) |
| LAX EB.STARTER | 2 vCores | 2GB | 40GB SSD | 4Gbps | 2.4TB/mo | $16.90 | $181.90 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=185) |
| LAX EB.MEDIUM | 2 vCores | 4GB | 80GB SSD | 8Gbps | 4.5TB/mo | $29.90 | $322.99 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=186) |
| LAX T1.STARTER | 1 vCore | 2GB | 40GB SSD | burst | 4TB/mo | $12.90 | — | [Order Now](https://bit.ly/DMIt) |
| LAX T1.MICRO | 4 vCores | 4GB | 80GB SSD | burst | 16TB/mo | $32.90 | — | [Order Now](https://bit.ly/DMIt) |

### Hong Kong

| Plan | CPU | RAM | Storage | Bandwidth | Traffic | Price (monthly) | Price (annual) | Order |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| HKG Pro.STARTER | 1 vCore | 2GB | 40GB SSD | 300Mbps | 500GB/mo | — | ~$298 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=152) |
| HKG EB.TINY | 1 vCore | 1GB | 20GB SSD | 1Gbps | 1TB/mo | $25.90 | $310.80 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=188) |
| HKG EB.STARTERv2 | 1 vCore | 2GB | 40GB SSD | 2Gbps | 2TB/mo | $59.90 | — | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=189) |
| HKG T1.WEE | 1 vCore | 0.5GB | 10GB SSD | 10Gbps | 800GB/mo | $3.07 | $36.90 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=195) |
| HKG T1.TINY | 1 vCore | 1GB | 20GB SSD | 10Gbps | 1TB/mo | $6.14 | $73.80 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=196) |

### Tokyo

| Plan | CPU | RAM | Storage | Bandwidth | Traffic | Price (monthly) | Price (annual) | Order |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TYO Pro.TINY | 1 vCore | 1GB | 20GB SSD | 1Gbps | 500GB/mo | $21.90 | $262.80 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=172) |
| TYO Pro.STARTER | 1 vCore | 2GB | 40GB SSD | 1Gbps | 1TB/mo | $39.90 | $478.80 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=173) |
| TYO EB.TINY | 1 vCore | 1GB | 20GB SSD | 1Gbps | 1TB/mo | $25.90 | $310.80 | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=190) |
| TYO T1.TINY | 1 vCore | 1GB | 20GB SSD | 10Gbps | 1TB/mo | ~$7.00 | — | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=200) |
| TYO T1.STARTER | 1 vCore | 2GB | 40GB SSD | 10Gbps | 2TB/mo | ~$14.00 | — | [Order Now](https://www.dmit.io/aff.php?aff=13832&pid=201) |

## Active Promo Codes That Pair With What The Looking Glass Told You

Once you've used the Looking Glass to settle on a network profile, the next move is to apply the right coupon at checkout. DMIT's discounts are recurring—they apply on every renewal, not just the first invoice—which is a meaningfully bigger deal than a one-time promo. A 20% recurring discount on a $139.90/year plan saves you roughly $28 every year, indefinitely, for as long as you stay on the plan.

The codes confirmed working as of mid-2026:

- **`LAX-EB-LAUNCH-NON-MONTHLY-RECURRING-20OFF`** — 20% recurring off on Los Angeles Eyeball plans (TINY and above), quarterly or annual billing. This is the standout value: CMIN2 routing gives solid China optimization at a price well below CN2 GIA Premium, and 20% off recurring makes the math compelling. If the LAX EB Looking Glass MTR looked clean to your audience, this is the code to use. 👉 [Apply it on a LAX EB plan](https://www.dmit.io/aff.php?aff=13832&pid=183)
- **`HKG-T1-ANNUALLY-45OFF-RECUR`** — 45% recurring off plus upgraded specs (more vCPU, double disk, ~50% more RAM) on Hong Kong Tier 1 annual plans. Hong Kong Tier 1 uses RETN international routing, not CN2—so confirm on the Looking Glass that RETN is fine for your audience before committing. At $36.90/year base with 45% off, you're looking at roughly $20/year for a Hong Kong VPS with a 10Gbps port. 👉 [Grab the HKG T1 deal](https://www.dmit.io/aff.php?aff=13832&pid=195)
- **`2025-TYO-T1-HI-GSL-NON-MONTHLY-30OFF`** — 30% recurring off on Tokyo Tier 1, quarterly or annual. Tokyo Tier 1 gives you 10Gbps bandwidth and decent Asia-America routing without China-specific optimization. Good fit for mobile-user infrastructure or a Japan regional presence. There's also a monthly variant, `2025-TYO-T1-HI-GSL-MONTHLY-10OFF`, if you want to test first. 👉 [Try Tokyo Tier 1](https://www.dmit.io/aff.php?aff=13832&pid=200)
- **`SJC-Unmetered-Annually-30OFF`** — 30% off San Jose unmetered bandwidth plans, annual billing. San Jose is DMIT's fourth location, built for high-volume transfer (media streaming, large file distribution, backups) rather than China-optimized routing. 👉 [Check San Jose unmetered](https://bit.ly/DMIt)
- **`7L8O3PQTHNXCFS2TXPLP`** — 5% off, general purpose, non-monthly billing. A fallback for plans that don't have a dedicated coupon (Hong Kong Eyeball, Tokyo Premium, etc.). Small, but better than nothing.
- **`SPRO-20OFF`** — 20% off SPro orders, per third-party coupon aggregators. Worth trying at checkout if your plan is eligible.

A few traps worth naming: almost every DMIT code requires quarterly or annual billing, monthly plans are typically excluded; codes don't stack—one per transaction; and code names are descriptive (`LAX-EB` = Los Angeles Eyeball, `HKG-T1` = Hong Kong Tier 1), so trying a code on the wrong series silently fails. DMIT also caps promotional inventory and doesn't oversell, so when a coupon-eligible plan shows in stock, it's genuinely available—waiting for a "better" sale usually means missing the slot.

## So What Should You Actually Buy?

The Looking Glass exists so you don't have to take anyone's word for it, but if you want a starting recommendation based on what most users are testing and buying right now:

- **Best China connectivity, money no object** — LAX Pro.STARTER or HKG Pro.STARTER. The Looking Glass MTR will show you why: clean CN2 GIA end-to-end, low loss at peak. Use the general `7L8O3PQTHNXCFS2TXPLP` for 5% off since the Premium line doesn't have a dedicated recurring code at the moment. 👉 [Start with LAX Pro](https://www.dmit.io/aff.php?aff=13832&pid=169)
- **Best value with real China routing** — LAX EB.STARTER with `LAX-EB-LAUNCH-NON-MONTHLY-RECURRING-20OFF`. CMIN2 routing at 20% off recurring, indefinitely. This is the combo most people should land on. 👉 [Get the LAX EB discount](https://www.dmit.io/aff.php?aff=13832&pid=185)
- **Cheapest Hong Kong presence** — HKG T1.WEE with `HKG-T1-ANNUALLY-45OFF-RECUR`. ~$20/year after discount for a real Hong Kong VPS with 10Gbps. Confirm on the Looking Glass that RETN routing works for your audience first. 👉 [Lock in HKG T1](https://www.dmit.io/aff.php?aff=13832&pid=195)
- **Japan presence on a budget** — TYO T1.TINY with `2025-TYO-T1-HI-GSL-NON-MONTHLY-30OFF`. 30% off recurring plus 10Gbps bandwidth. 👉 [Try Tokyo Tier 1](https://www.dmit.io/aff.php?aff=13832&pid=200)

The workflow that actually works: open the 👉 [DMIT Looking Glass](https://bit.ly/DMIt), run MTR from each candidate node toward your audience's region, pick the profile whose loss and latency numbers look right, then apply the matching coupon at checkout on a quarterly or annual billing cycle. Fifteen minutes of testing saves you a year of paying for the wrong network.
