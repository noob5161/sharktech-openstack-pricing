# public cloud：what Sharktech's OpenStack plans really cost ($39–$499/mo), and when they make more sense than AWS or Azure

Most people searching "public cloud" want one of two things: a clear definition, or a cheaper place to actually run their servers. Here's the short version of both.

A public cloud is computing infrastructure — VMs, storage, networking — that a provider owns and rents out over the internet, on demand, in shared pools. You don't buy hardware; you pay for what you use [S1][S4]. If you're at the "where should I actually deploy" stage, Sharktech's public cloud is worth a look, especially if your bill at a hyperscaler has started growing in ways you can't explain. It runs on OpenStack, starts at **$39/month**, spans five data centers in the US and Europe, and includes DDoS protection at the network level. It's also genuinely not for everyone — more on that below.

> **Quick verdict:** For most developers and small teams running VMs, the **Small plan at $39/mo** (4 cores, 8GB RAM, 300GB SSD, 20TB transfer included) is the right starting point — it maps to a typical mid-size cloud server with room to burst. Steady, predictable workloads should compare the Dedicated Cloud model instead. 👉 [Check Sharktech Public Cloud plans and current pricing](https://bit.ly/SharKtech)

## What a public cloud actually is, in 30 seconds

You rent virtualized servers, storage, and networking from a provider's shared infrastructure, accessible over the public internet, and scale up or down without owning anything [S1][S2][S4]. The alternative is a private cloud, where the physical infrastructure is exclusive to one organization. Sharktech sells both, and the distinction matters later in this article: its Public Cloud is a shared OpenStack pool with metered billing, while its Private Cloud is custom-built dedicated hardware with quote-based pricing [S10][S19].

One industry datapoint for context: global spending on public cloud services was projected to hit $679 billion in 2024 and pass $1 trillion by 2027 [S3]. The market isn't short on providers — the real question is which billing model won't ambush you.

## Sharktech public cloud plans: the full price table

Sharktech's Public Cloud has four fixed plans plus a Custom option. Prices below are the monthly base commitment, pulled from the company's own plan and calculator pages; each plan's base covers an included resource pool, and usage beyond it is metered [S11][S12].

| Plan | Included resources | Base price | Hourly equivalent | Order |
| --- | --- | --- | --- | --- |
| Small | 4 cores · 8GB RAM · 300GB SSD · 20,000GB transfer · 1 IPv4 | $39/mo | ~$0.0609/hr | [Order Small](https://bit.ly/SharKtech) |
| Medium | 8 cores · 16GB RAM · 800GB SSD · 20,000GB transfer · 1 IPv4 | $79/mo | ~$0.1289/hr | [Order Medium](https://bit.ly/SharKtech) |
| Large | 32 cores · 64GB RAM · 1,500GB SSD · 20,000GB transfer · 1 IPv4 | $249/mo | ~$0.3989/hr | [Order Large](https://bit.ly/SharKtech) |
| Enterprise | 64 cores · 128GB RAM · 5,000GB SSD · 20,000GB transfer · 1 IPv4 | $499/mo | ~$0.7412/hr | [Order Enterprise](https://bit.ly/SharKtech) |
| Custom | Example config: 32 cores · 64GB RAM · 2,400GB SSD · 8 IPv4 · 40,000GB transfer | from $51.82/mo | ~$0.072/hr | [Request Custom](https://bit.ly/SharKtech) |

The plans share one platform: Linux or Windows VMs, three storage tiers (NVMe, SSD, HDD), private networks, routers, security groups, floating IPs, firewalls, and load balancers. You can upload your own VM images and ISOs through the portal or API [S10][S20]. All plans can be deployed in Los Angeles, Las Vegas, Denver, Chicago, or Amsterdam [S10][S16].

## How billing actually works — the details most overviews skip

The base price covers your included allocation; anything above it is charged at published rates [S10]. That sentence hides three things that genuinely affect your invoice, and they rarely show up in comparison articles.

**First: resource ceilings are not spending caps.** Sharktech states this explicitly: "Resource ceilings are not monetary spending caps" [S10][S16]. Small, Medium, and Large cap how far you can burst above your included pool; Enterprise has no preset ceiling. If you run Enterprise and your workloads spike hard, your bill grows with your usage — there is no automatic monthly maximum. Budget accordingly.

**Second: storage keeps billing when your VM is off.** Storage is metered on a 730-hour estimated month and stays billed for the full period even when the VM is powered down [S12]. Deleting the disks you no longer need is the only way to stop paying for them. Stopping a VM saves you compute charges, but if you snapshot everything and keep the volumes, the storage line on your invoice won't move.

**Third: you buy a resource pool, then build VMs inside it.** Billing is aggregated across the whole pool, not per named VM shape [S16]. The calculator page also notes that "VM details are an estimate and must be configured after ordering" [S12] — so the order flow is: pick a plan, pick a location, then log into the portal and carve the pool into machines. A common trick with this model: one pool can run several small VMs instead of one big one, in any combination [S17].

Overage rates beyond the included pool [S11][S12]:

- CPU: $0.0025 per core/hour
- RAM: $0.0035 per GB/hour
- SSD storage: $0.00006 per GB/hour
- NVMe storage: $0.00009 per GB/hour
- HDD storage: $0.00002 per GB/hour
- Transfer beyond 20,000GB: $0.002/GB; extra IPv4: $1.50/mo
- The published rate sheet lists no inbound traffic charge; egress beyond the included 20TB is what gets metered

If you'd rather sanity-check a specific deployment before committing, the numbers above are exactly what the official cloud cost calculator uses — it estimates a multi-VM build against each plan's base price and shows whether you'd exceed the included pool. 👉 [Open the cloud cost calculator and price your setup](https://bit.ly/SharKtech)

## Public Cloud vs. Dedicated Cloud: which model fits you

Same OpenStack platform, two billing philosophies — and picking the wrong one costs money either way [S10][S20].

|  | Public Cloud | Dedicated Cloud |
| --- | --- | --- |
| Billing | Monthly base + metered usage above included pool | Fixed price for a fixed allocation |
| Growth | Burst at published rates (S/M/L have ceilings) | No automatic overages; upgrade via billing platform |
| Fits | Workloads with changing resource demand | Steady workloads, predictable budgets |

The practical rule: if your usage swings — dev environments that idle at night, staging stacks, seasonal apps — Public Cloud's burst model is cheaper than paying Dedicated Cloud prices for peak capacity all month. If you run the same load 24/7 and hate surprises on invoices, Dedicated Cloud's flat fee does exactly that, with no hourly resource charges at all [S10][S16].

One naming trap to avoid: Dedicated Cloud is still on Sharktech's shared platform, just with fixed allocation. Private Cloud is a different product entirely — exclusive physical hardware, custom-built, quote-based pricing, designed for things like HIPAA-scoped deployments [S19].

## How it compares with AWS, Azure, and Google Cloud

Sharktech's own pitch is cost: the company claims at least 40% savings versus hyperscalers on comparable resources [S11]. That's the vendor's number, so treat it as a claim — but the structural reasons behind it check out on the rate sheet. Transfer beyond the included 20TB bills at $0.002/GB, which is where hyperscalers traditionally produce the most painful line items; a few terabytes of monthly egress elsewhere can quietly exceed this entire plan's base price. DDoS protection is built into the network by default rather than sold as an add-on, which is the company's approach since 2003 [S9].

On the honest side of the ledger: the big three offer vastly more managed services — AWS alone covers managed databases, analytics, AI/ML pipelines, IoT, and serverless in a way a VM-focused OpenStack platform simply doesn't [S8]. If your project needs those, no amount of egress savings should pull you away. A review displayed on Sharktech's site makes the counterpoint from the other direction: an IT manager who migrated from AWS and Azure to Sharktech's Dedicated Cloud described the price as "shockingly reasonable" and said he got more control and flexibility than on the hyperscalers [S9]. HostAdvice's published review of the platform noted "6,000+ random IOPS, sub-millisecond network latency … Sharktech delivers genuine power" [S9].

The OpenStack foundation matters here too. It's an open platform, you can upload your own images and ISOs, and the REST API covers compute, networking, storage, and identity management [S10][S20] — so your VM configurations aren't trapped in one vendor's proprietary format. That's a real exit ramp, which the hyperscalers structurally don't offer.

👉 [See Sharktech's full cloud feature set and order options](https://bit.ly/SharKtech)

## Who this isn't for

Three cases where you should spend your money elsewhere:

- **You need managed serverless, managed Kubernetes, or ML pipelines as services.** Sharktech's platform gives you VMs, storage, and networking on OpenStack [S10]; it doesn't replicate AWSLambda-scale ecosystems [S8].
- **You need regions outside the five listed locations.** Los Angeles, Las Vegas, Denver, Chicago, Amsterdam — if your users are primarily in Asia or South America, latency will suffer and no plan tier fixes geography [S10][S16].
- **Your workload is tiny.** The entry point is $39/mo for a 4-core pool. A simple blog or a single lightweight VPS doesn't need a resource pool; Sharktech's own cheaper VPS line exists for that, starting lower.

## FAQ

**Can I run multiple VMs on one plan?** Yes. Billing aggregates the whole pool, so a Small plan's 4 cores and 8GB can be split across several small machines in any combination, and you can rebalance later [S16][S17].

**Is there hourly billing?** Yes — every plan has a published hourly equivalent (Small runs about $0.0609/hour) [S11], so short-term test environments don't require a full-month commitment. Note that storage still bills on the 730-hour month while volumes exist [S12].

**Can I run Windows?** Yes, both Linux and Windows VMs are supported, alongside custom image and ISO uploads [S10][S20].

**What happens if I exceed my plan?** On Small, Medium, and Large you can burst to that plan's ceiling and pay published rates above the included pool; Enterprise has no preset ceiling, and usage above the pool keeps metering [S10][S16]. Remember the ceiling is a resource limit, not a billing limit.

## Before you click order

Match the plan to your load pattern, not your aspirations. Light-to-medium VMs with variable demand → Small at $39/mo covers the majority of real-world setups, and Medium at $79 is the step up if you already know 8GB of RAM is tight. Flat 24/7 load → compare Dedicated Cloud's fixed allocation first, since you'd be paying for burst capacity you won't use. Big managed-service requirements or users far from North America and Western Europe → the hyperscalers or a regional provider fit better, and no table in this article changes that.

Two practical checks at checkout: pick your service location deliberately — it's chosen when you order the pool — and set a calendar reminder to review your first invoice, since metered usage above the pool only shows up in billing, not in the plan price. When you're ready, the 👉 [Small plan page is the place to start configuring your pool](https://bit.ly/SharKtech).
