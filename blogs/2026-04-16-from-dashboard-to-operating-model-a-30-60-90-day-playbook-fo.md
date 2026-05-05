---
title: "From dashboard to operating model: A 30-60-90 day playbook for cloud sustainability in FinOps"
url: "https://www.flexera.com/blog/finops/from-dashboard-to-operating-model-a-30-60-90-day-playbook-for-cloud-sustainability-in-finops/"
date: "Thu, 16 Apr 2026 16:45:25 +0000"
author: "Mark Bradley"
feed_url: "https://www.flexera.com/blog/feed/"
---
<div><img alt="" class="attachment-card-hero size-card-hero wp-post-image" height="480" src="https://www.flexera.com/blog/wp-content/uploads/2026/02/featured-10.jpg" style="margin-bottom: 10px;" width="915" /></div><div contenteditable="false" id="toc" title="Table of Contents"></div>
<h2>Cloud sustainability cuts cost and carbon when it’s built into an operating model</h2>
<p>Sustainability is a compliance capability that&#8217;s become increasingly essential. Reporting requirements have expanded significantly over the past year, and they&#8217;re cascading through supply chains in ways that affect organizations regardless of where they&#8217;re headquartered. But it’s not enough to meet mandates like <a href="https://finance.ec.europa.eu/financial-markets/company-reporting-and-auditing/company-reporting/corporate-sustainability-reporting_en" rel="noopener" target="_blank">CSRD</a>, <a href="https://commission.europa.eu/topics/business-and-industry/doing-business-eu/sustainability-due-diligence-responsible-business/corporate-sustainability-due-diligence_en" rel="noopener" target="_blank">CSDDD</a>, <a href="https://taxation-customs.ec.europa.eu/carbon-border-adjustment-mechanism_en" rel="noopener" target="_blank">CBAM</a> in Europe and California&#8217;s <a href="https://leginfo.legislature.ca.gov/faces/billNavClient.xhtml?bill_id=202320240SB253" rel="noopener" target="_blank">SB 253</a> and <a href="https://leginfo.legislature.ca.gov/faces/billNavClient.xhtml?bill_id=202520260SB261" rel="noopener" target="_blank">SB 261</a>. To provide real value to the business, you need to go beyond dashboards and reporting to the “double win” opportunities, where a single action cuts both spend and emissions.</p>
<p>Dashboards don&#8217;t reduce emissions. Operating models do.</p>
<p>To make the operating model successful, carbon must be measured, allocated, forecasted and governed with the same rigor that FinOps teams already apply to cloud spend. We call it &#8220;treating carbon like cash,&#8221; and the data suggests the industry is moving in this direction. The <em><a href="https://info.flexera.com/CM-REPORT-State-of-the-Cloud">2026 Flexera State of the Cloud Report</a></em> found that for nearly a third of respondents, cost optimization and reducing carbon emissions are now equal priorities.</p>
<p>Most organizations have some form of cloud sustainability visibility today — a report, a quarterly slide, maybe a provider-generated summary that arrives weeks after the fact. The challenge isn&#8217;t seeing the data. It&#8217;s turning that data into a repeatable practice that drives sustainable cloud computing action, assigns ownership and produces measurable results over time. That’s “treating carbon like cash”.</p>
<h3>Treating carbon like cash in FinOps</h3>
<p>The phrase &#8220;treat carbon like cash&#8221; describes a specific operational shift for FinOps sustainability: applying the same financial discipline you already use for cloud spend to your organization&#8217;s carbon footprint.</p>
<p>When carbon is managed like a financial metric, you can:</p>
<ul>
<li><b>Allocate it </b>to the teams, services and business units that generated it — creating the same ownership and accountability structures you use for cost</li>
<li><b>Forecast it </b>using historical trends and confidence bands, so you can plan ahead instead of reacting after the fact</li>
<li><b>Set budgets and guardrails</b> so sustainability targets are enforced with the same governance mechanisms as financial targets</li>
<li><b>Prioritize work by return</b> — comparing optimization opportunities not just by cost savings but by the combined impact on cost, carbon and water</li>
</ul>
<p>This should be an extension of what FinOps teams already do. The cadence is the same (monthly reviews, optimization sprints, variance analysis). The accountability model is the same (ownership by team and account). The allocation mechanics are the same (tags, hierarchies, organizational structures). The only difference is that sustainability signals are now plugged into the same system of work.</p>
<p>And here&#8217;s the key insight: many of the optimization actions that reduce cost also reduce emissions. Rightsizing an oversized VM saves money and reduces energy consumption. Retiring a zombie workload that no one uses eliminates both the spend and the carbon footprint. Shifting a workload to a region with a cleaner energy grid may not change the cost much, but it can materially reduce emissions. When you can see both dimensions side by side, you can target these &#8220;double win&#8221; opportunities systematically instead of stumbling across them by accident.</p>
<p>So how do you make this model and reality in your organization? Whether you’re starting from scratch or looking to mature an existing sustainability practice, we’ve built a concrete path forward using the FinOps Foundation’s Framework with specific actions and checkpoints to gauge your progress and success.</p>
<h2>Why the FinOps Framework works for cloud sustainability</h2>
<p>If you&#8217;re familiar with the <a href="https://www.finops.org/framework/" rel="noopener" target="_blank">FinOps Foundation&#8217;s Framework</a>, the structure of this playbook will feel natural. The Inform → Optimize → Operate cycle is the backbone of cloud financial management: build visibility, take action on what you find, then operationalize reporting and governance so the work sustains itself.</p>
<p>The same structure works for a cloud sustainability strategy with one important requirement. For sustainability metrics to fit into this framework, they need to behave like cost metrics. That means they need to be allocatable (tied to the teams and services that generated them), actionable (connected to recommendations that quantify impact) and repeatable (supported by reporting and governance that hold up month after month).</p>
<p>When sustainability data meets that bar, it stops being a parallel reporting effort and becomes part of the FinOps system of work. That&#8217;s the goal of this playbook.</p>
<h2>Phase 1: Inform your cloud sustainability baseline (30 days)</h2>
<h3>Build a baseline you can defend</h3>
<p>The first phase is about establishing clear, trustworthy visibility into your organization&#8217;s cloud sustainability footprint. You can&#8217;t manage what you can&#8217;t measure, but more importantly, you can&#8217;t manage what you can&#8217;t explain. The goal isn&#8217;t just to produce a number; it&#8217;s to produce a number you can defend, slice by any relevant dimension and use to answer, &#8220;what drove the change?&#8221; when someone asks.</p>
<h3>Actions to take to establish your cloud sustainability baseline</h3>
<ul>
<li><b>Connect your cloud billing data to sustainability metrics.</b> Flexera One Cloud Sustainability uses your existing cloud billing data combined with auditable emissions factors from <a href="https://greenpixie.com/" rel="noopener" target="_blank">Flexera partner Greenpixie</a> to generate carbon (CO₂e), energy and water metrics. This isn&#8217;t a separate data pipeline; it&#8217;s a sustainability layer built on top of the financial data you already trust</li>
<li><b>Validate your allocation inputs.</b> Sustainability data is only as useful as the organizational context around it. Review your accounts, subscriptions, tags, labels and organizational hierarchies to make sure they&#8217;re clean and consistent. The question you need to be able to answer is &#8220;who owns this?&#8221; Ownership is what turns a metric into accountability</li>
<li><b>Explore the carbon dashboards.</b> Track CO₂e alongside cloud cost over time. Drill into the drivers by cloud provider, resource category and region. The goal at this stage is to understand the shape of your carbon footprint. Where is the carbon concentrated? Which services and regions are the biggest contributors? Are there surprises such as workloads that are modest in cost but significant in emissions?</li>
<li><b>Identify your initial hotspots.</b> Look specifically for always-on workloads (especially AI-related), high-density compute in carbon-intensive regions and any services where the sustainability footprint is disproportionate to the spend. These are the areas where Phase 2 optimization will deliver the most impact</li>
<li><b>Set up organizational context for accountability.</b> Add attributes like Azure subscription and resource group and use tag- and hierarchy-driven filtering to align sustainability reporting to the same structures you use for financial reporting. If your cost data is organized by business unit and team, your carbon data should be too</li>
</ul>
<h3>Flexera capabilities that give you cloud sustainability visibility</h3>
<p>Flexera One Cloud Sustainability provides carbon dashboards that connect spend to impact, with the ability to track CO₂e alongside cloud cost and drill into drivers by provider, resource category and region. Organizational context features let you add attributes and use tag- and hierarchy-driven filtering to align sustainability reporting to your existing financial reporting structures. Enterprise-grade access patterns and the ability to extract sustainability datasets for downstream BI workflows (including Power BI) ensure the data is accessible at scale.</p>
<p><img alt="" class="alignnone wp-image-35006 size-full" height="900" src="https://www.flexera.com/blog/wp-content/uploads/2026/04/Carbon-Spend-Screenshot-and-Drilldown.png" width="1600" /></p>
<p><img alt="" class="alignnone wp-image-35007 size-full" height="900" src="https://www.flexera.com/blog/wp-content/uploads/2026/04/Sustainability-Carbon-Emission-Screenshot-Drilldown_Blog.png" width="1600" /></p>
<h3>Checkpoint</h3>
<p>Ask yourself: Can I explain, by team and service, what&#8217;s driving our carbon emissions this month and how that compares to last month? If the answer is yes, you&#8217;re ready for Phase 2.</p>
<h2>Phase 2: Optimize for cost and carbon together (60 days)</h2>
<h3>Move from awareness to action</h3>
<p>Phase 1 gave you the map. Phase 2 is where you start moving. The goal here is to execute a focused optimization sprint using recommendations that quantify both cost and sustainability impact, so you can target the &#8220;double win&#8221; opportunities where a single action reduces both spend and emissions.</p>
<p>This is also where the &#8220;carbon like cash&#8221; concept becomes most concrete. By applying a financial lens to carbon impact, you can compare different optimization opportunities using the same decision language your FinOps team and executive stakeholders already share.</p>
<h3>Actions to take to quantify cost and sustainability impact</h3>
<ul>
<li><b>Review the AI Carbon Optimizer Dashboard.</b> This is where you see carbon impact alongside the cost optimization recommendations your team already trusts — rightsizing, waste reduction, scheduling and retirement. Instead of running cost optimization and sustainability optimization as separate workstreams, you&#8217;re seeing both dimensions of every opportunity in a single view</li>
<li><b>Prioritize using a dual lens: cost savings plus sustainability impact.</b> Not every optimization opportunity delivers the same ratio of cost savings to carbon reduction. Some changes save a lot of money but have minimal sustainability impact. Others are modest in dollar terms but eliminate outsized emissions. The “carbon like cash” approach lets you apply a financial lens to carbon impact so trade-offs are comparable and prioritization is grounded in a decision framework your organization already understands</li>
<li><b>Target &#8220;small spend, big impact&#8221; opportunities. </b>This is one of the most important and most overlooked categories. Traditional cost triage starts with the biggest line items and works down. But some resources sit below the cost threshold that would trigger attention, yet they run 24 hours a day, 7 days a week, and create outsized sustainability and compliance impact. Sustainability-aware recommendations in Flexera One surface these opportunities earlier, so they don&#8217;t stay hidden behind a &#8220;not worth my time&#8221; cost filter</li>
<li><b>Hunt for zombie and ghost systems</b>. Sustainability analysis is remarkably effective at surfacing forgotten infrastructure. Demo environments that were never torn down. Test VMs that outlived their project. Legacy workloads that no one uses but everyone assumes someone else owns. These systems continue to run, update, consume resources and generate emissions. They&#8217;re prime candidates for retirement, and eliminating them delivers immediate savings across cost, carbon and water</li>
<li><b>Consider location-aware optimization.</b> The carbon intensity of electricity isn&#8217;t uniform. It depends on where workloads run and the energy mix powering that region. A workload running in a region powered primarily by renewables has a materially different carbon footprint than the same workload running in a coal-heavy region, even if the cloud cost is identical. Where feasible, consider shifting or redesigning workloads to take advantage of cleaner grid regions. This is an optimization lever that cost-only analysis would never surface</li>
</ul>
<h3>Flexera capabilities that support carbon and cost optimization</h3>
<p>The AI Carbon Optimizer Dashboard shows carbon impact alongside the cost optimization recommendations teams already run. The Carbon Impact Calculation Engine translates cost optimization opportunities into quantified environmental impact including carbon and water, making prioritization concrete and measurable. “Carbon like cash” prioritization applies a financial lens to carbon impact so teams can compare trade-offs using familiar decision mechanics. And the &#8220;small spend, big impact&#8221; detection surfaces resources that cost triage misses but sustainability analysis reveals.</p>
<h3>Checkpoint</h3>
<p>Ask yourself: Can our team compare a rightsizing opportunity against a workload migration — not just by cost savings, but by the combined impact on cost, carbon and water? If the answer is yes, you&#8217;re optimizing with a complete picture.</p>
<h2>Phase 3: Operationalize cloud sustainability in FinOps (90 days)</h2>
<h3>Keep it running</h3>
<p>Phase 1 built the baseline. Phase 2 delivered the first wave of optimization. Phase 3 is where sustainability becomes embedded in your FinOps operating rhythm rather than treated as a one-time sprint.</p>
<p>This is the phase that separates organizations that merely report on sustainability from organizations that manage it. The difference is the same as in FinOps for cost: the value isn&#8217;t in the initial cleanup; it&#8217;s in the ongoing discipline that prevents waste from creeping back and ensures progress compounds over time.</p>
<h3>Actions to take to operationalize cloud sustainability</h3>
<ul>
<li><b>Set up ongoing reporting for both practitioners and executives.</b> FinOps engineers and cloud architects need operational views that are detailed, filterable, connected to the services and accounts they manage. Executive stakeholders need rollup summaries that show trends, progress against targets and the connection between optimization actions and sustainability outcomes. Flexera supports both levels, so you&#8217;re not maintaining two separate reporting systems</li>
<li><b>Introduce forecasting. </b>Backward-looking reports tell you where you&#8217;ve been. Forecasting tells you where you&#8217;re heading and whether you&#8217;ll hit your targets at the current trajectory. Flexera&#8217;s carbon forecasting uses auditable, confidence-banded trend visuals built from historical emissions data. This gives FinOps and sustainability leaders forward-looking signals, not just rearview reporting, and creates early warning when you&#8217;re drifting off track</li>
<li><b>Add sustainability variance review to your monthly FinOps cadence. </b>Just as you explain cost variance every month, you should be able to explain carbon variance. Did emissions increase because of legitimate growth, or because of waste? If they decreased, was it because of intentional optimization or because a team decommissioned a project? Building this habit is what turns sustainability from a metric on a dashboard into a managed dimension of cloud operations</li>
<li><b>Define clear ownership.</b> Align sustainability reporting to the same accountability structures you use for financial reporting. If a team owns their cloud cost, they should own their cloud carbon footprint too. This isn&#8217;t about creating new org structures. It&#8217;s about extending the ownership model you already have</li>
<li><b>Build governance signals.</b> Set targets, whether internal goals or externally aligned commitments, and create the guardrails and reporting cadence to track against them. Define what &#8220;good&#8221; looks like, establish the executive-ready summaries that support internal reporting and external compliance narratives, and make sure the data is auditable and defensible. This is increasingly important for organizations operating in or selling into markets with expanding sustainability disclosure requirements</li>
</ul>
<h3>Flexera capabilities that provide carbon forecasting</h3>
<p>Flexera One Cloud Sustainability provides carbon forecasting with auditable, confidence-banded trend visuals. Reporting is built for both practitioners and executives, with operational detail views and leadership-ready rollups. Enterprise-grade performance supports high-volume analysis at scale. And the Flexera One platform is designed to align with the FinOps operating cadence and ownership models your team already runs.</p>
<h3>Checkpoint</h3>
<p>Ask yourself: If our CFO asked today, &#8220;Are we on track against our sustainability targets and what actions drove the change?&#8221;, could I answer in under five minutes? If the answer is yes, you&#8217;ve operationalized sustainability.</p>
<p><img alt="" class="alignnone wp-image-35005 size-full" height="1266" src="https://www.flexera.com/blog/wp-content/uploads/2026/04/Sustainability-Forecast-View-2-DRILL-DOWN-FOR-BOUNDS-Screenshot-2026-04-15-115934300.png" width="1517" /></p>
<h2>A 30-60-90 day cloud sustainability playbook at a glance</h2>
<table>
<thead>
<tr>
<th>Timeframe</th>
<th>Phase</th>
<th>Objective</th>
<th>Key Question</th>
</tr>
</thead>
<tbody>
<tr>
<td>30 days</td>
<td>Inform</td>
<td>Build a baseline that links spend to carbon, energy and water. Validate allocation inputs and identify hotspots.</td>
<td>Can I explain what&#8217;s driving our emissions by team and service and how it changed?</td>
</tr>
<tr>
<td>60 days</td>
<td>Optimize</td>
<td>Execute a focused sprint using recommendations with both cost and sustainability impact. Prioritize &#8220;double win&#8221; opportunities using Carbon Like Cash.</td>
<td>Can I compare optimization options by combined cost, carbon and water impact?</td>
</tr>
<tr>
<td>90 days</td>
<td>Operate</td>
<td>Operationalize reporting, forecasting, ownership and governance. Embed sustainability into the monthly FinOps cadence.</td>
<td>Can I answer &#8220;are we on track?&#8221; with defensible data in under five minutes?</td>
</tr>
</tbody>
</table>
<p>One important note: this isn&#8217;t a linear, one-time process. After 90 days, you cycle back to Inform — with a better baseline, tighter allocation, cleaner data and sharper questions. Each cycle compounds the value, just as it does in FinOps for cost.</p>
<p>Ready to start your own 30-60-90 Cloud Sustainability program? <a href="https://www.flexera.com/about-us/contact-us">Request a demo</a> to see how Flexera One Cloud Sustainability applies to your environment.</p>
<p style="text-align: center;"><a class="btn" href="https://www.flexera.com/about-us/contact-us">Talk with a cloud sustainability expert</a></p>
