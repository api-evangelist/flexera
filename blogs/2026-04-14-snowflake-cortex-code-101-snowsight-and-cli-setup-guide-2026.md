---
title: "Snowflake Cortex Code 101: Snowsight and CLI setup guide (2026)"
url: "https://www.flexera.com/blog/finops/snowflake-cortex-code/"
date: "Tue, 14 Apr 2026 12:30:09 +0000"
author: "Pramit Marattha"
feed_url: "https://www.flexera.com/blog/feed/"
---
<div><img alt="" class="attachment-card-hero size-card-hero wp-post-image" height="478" src="https://www.flexera.com/blog/wp-content/uploads/2025/09/blog-header-10-915x478.jpg" style="margin-bottom: 10px;" width="915" /></div><div id="toc" title="Table of Contents"></div>
<p><a href="https://github.com/e2b-dev/awesome-ai-agents"><span>AI coding agents</span></a><span> are now available everywhere. </span><a href="https://github.com/copilot"><span>GitHub Copilot</span></a><span>, </span><a href="https://cursor.com/"><span>Cursor</span></a><span>, </span><a href="https://openai.com/codex/"><span>OpenAI&#8217;s Codex</span></a><span> and </span><a href="https://aws.amazon.com/q/developer/"><span>Amazon Q Developer</span></a><span> are just a few examples. These tools are good at writing and completing code, handling application logic and performing various coding-related tasks. But they have trouble working in the </span><a href="https://www.flexera.com/blog/finops/snowflake-vs-databricks/#what-is-snowflake"><span>Snowflake environment</span></a><span>. Third-party tools often don&#8217;t integrate well with Snowflake&#8217;s security and metadata services. </span></p>
<p><span>To solve this problem, Snowflake introduced its own coding agent, </span><a href="https://www.flexera.com/blog/finops/snowflake-build-2025/#%F0%9F%91%89-cortex-code-ai-assistant-preview"><b><span>Snowflake Cortex Code</span></b></a><span>. Announced at </span><a href="https://www.flexera.com/blog/finops/snowflake-build-2025/#tldr"><span>Snowflake BUILD</span></a><span>, Cortex Code knows the Snowflake environment well. It knows well about your databases, schemas, active warehouses, semantic models and governance rules. This knowledge allows it to assist with SQL, Python and dbt projects without making errors like using incorrect table names or violating access controls. </span><span> </span></p>
<p><span>Snowflake Cortex Code is available through two interfaces. You can use Snowflake Cortex Code in </span><a href="https://www.flexera.com/blog/finops/snowflake-snowsight-guide/"><span>Snowsight</span></a><span>, which is currently in Open Preview and is free. The </span><a href="https://docs.snowflake.com/en/release-notes/2026/other/2026-02-02-cortex-code-cli"><span>Cortex Code command line interface (CLI)</span></a><span> is also available in GA on macOS and Linux. It integrates easily with </span><a href="https://code.visualstudio.com/"><span>VS Code</span></a><span>, Cursor and any terminal-capable editor.</span><span> </span></p>
<p><span>In this article, we will cover what Snowflake Cortex Code is, how it works, how it is different from other Snowflake AI features and exactly how to enable and use it in Snowflake Snowsight and via the CLI.</span></p>
<h2><span>What is Snowflake Cortex Code?</span><span> </span></h2>
<p><span>Snowflake Cortex Code is Snowflake&#8217;s own AI coding agent. It is a fully managed, Snowflake native assistant that translates natural language prompts into executable SQL, Python, dbt workflows, Streamlit apps and various other administrative actions. It does not just suggest code fragments. It plans multi-step tasks, executes them against your actual Snowflake environment and uses your account-level context (schemas, roles, semantic models, metadata) to make sure the output is accurate and relevant to your specific setup.</span></p>
<p><span>Snowflake Cortex Code is a part of Snowflake’s new Cortex AI suite, which also includes:</span></p>
<ul>
<li><a href="https://www.flexera.com/blog/finops/snowflake-intelligence/"><span>Snowflake Intelligence</span><span> </span></a></li>
</ul>
<ul>
<li><a href="https://www.flexera.com/blog/finops/snowflake-cortex-analyst/"><span>Snowflake Cortex Analyst</span></a><span> </span></li>
</ul>
<ul>
<li><a href="https://www.flexera.com/blog/finops/snowflake-cortex-search/"><span>Snowflake Cortex Search</span></a><span> </span></li>
</ul>
<ul>
<li><a href="https://www.flexera.com/blog/finops/snowflake-cortex/"><span>Snowflake Cortex AI Function</span></a><span> </span></li>
</ul>
<ul>
<li><a href="https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents"><span>Snowflake Cortex Agents</span></a></li>
</ul>
<p><span>Snowflake Cortex Code is the developer-facing component of the suite. </span></p>
<p><span>Snowflake Cortex Code is available to all commercial Snowflake accounts with </span><a href="https://docs.snowflake.com/en/user-guide/snowflake-cortex/cross-region-inference"><span>cross-region inference</span></a><span> enabled. Government cloud, virtual private Snowflake (VPS) and Sovereign cloud deployments are not supported now. If your account previously opted out of (or disabled) </span><a href="https://www.flexera.com/blog/finops/snowflake-copilot/"><span>Snowflake Copilot (legacy)</span></a><span>, Snowflake Cortex Code will also be disabled by default. You will need to contact your Snowflake account team to re-enable it.</span><span> </span></p>
<p><span>Snowflake Cortex Code is accessible via two channels:</span></p>
<ul>
<li><b><span>In-platform (Snowflake Snowsight/Snowflake Workspaces)</span></b><b><span> → </span></b><span>embedded in the Snowsight UI</span><span> </span></li>
</ul>
<ul>
<li><b><span>Local CLI (Terminal/IDE)</span></b><b><span> → </span></b><span>runs in your local terminal; works inside VS Code, Cursor, or any shell</span><span> </span></li>
</ul>
<div class="wp-caption aligncenter" id="attachment_34118" style="width: 1010px;"><img alt="Snowflake Cortex Code (Source: Snowflake) " class="wp-image-34118 size-full" height="428" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-1.png" width="1000" /><p class="wp-caption-text" id="caption-attachment-34118">Figure 1: Snowflake Cortex Code (Source: Snowflake)</p></div>
<p><span>Snowflake Cortex Code is a Snowflake native tool. It operates on Snowflake&#8217;s managed Cortex infrastructure, eliminating the need for a public third-part AI agents API. This allows it to access your account metadata and object details. When running queries or code, Cortex Code functions within your current security framework. It sends commands to your Snowflake warehouses or serverless compute. Due to this, everything is secure since Cortex Code can only access what you already have permission to see and use.</span><span> </span></p>
<h2><span>Architecture and how Snowflake Cortex Code accesses context</span><span> </span></h2>
<p><span>Snowflake Cortex Code can seem mysterious but is actually pretty straightforward. Once you understand how various parts interact, you will be able to use it more effectively.</span></p>
<p><span>Snowflake Cortex Code architecture is straightforward. Here is a detailed look at what goes on inside the Snowflake Cortex code.</span><span> </span></p>
<h3><span>The runtime environment</span><span> </span></h3>
<p><span>Snowflake Cortex Code runs inside Snowflake Cortex, Snowflake&#8217;s fully managed AI infrastructure. It does not hand off your queries or data to third-party model infrastructure outside your Snowflake environment. All models execute under Snowflake’s security and governance umbrella.</span><span> </span></p>
<p><span>When you submit a prompt, Snowflake Cortex Code takes action in a few ways:</span><span> </span></p>
<ul>
<li><span>It pulls in metadata from your account and specific object-level context, like schemas and tables</span><span> </span></li>
</ul>
<ul>
<li><span>It reads your current </span><a href="https://www.flexera.com/blog/finops/snowflake-snowsight-guide/#what-is-snowsight-from-snowflake"><span>Snowflake workspace</span></a><span> context; the SQL file or Snowflake notebook you have open becomes the implicit background context</span><span> </span></li>
</ul>
<ul>
<li><span>If needed, it will execute SQL or Python commands on your authorized warehouses and objects</span><span> </span></li>
</ul>
<p><span>The agent follows a simple workflow: it figures out what you mean, creates a plan, gathers the right tools and context, takes action and summarizes the results. The agent remembers all this context during the session, so if you ask a follow-up question, you do not have to repeat yourself.</span><span> </span></p>
<p><span>Something to clarify: Snowflake Cortex Code gets its “understanding” of your data from metadata. It does not scan the actual rows, but instead looks at table definitions, column names, tags, lineage, usage stats and semantic model specs. When it needs to get to the row-level data to answer a question, it sends a query to your warehouse, gets the results back and uses them in its response. The agent never sends your actual data to outside model providers. Inputs and outputs are classified based on </span><a href="https://www.snowflake.com/en/legal/compliance/snowflake-ai-trust-and-safety/"><span>Snowflake&#8217;s AI Terms and Acceptable Use Policy</span></a><span>.</span><span> </span></p>
<h3><span>The security model</span></h3>
<p><span>Snowflake Cortex Code really sets itself apart here. Most AI coding tools do not care about your data governance rules. They just spit out code without considering your setup. Cortex Code is different, it works within your secure perimeter, respecting all the safeguards and security measures you&#8217;ve put up.</span><span> </span></p>
<ul>
<li><b><span>Full Snowflake RBAC enforcement</span></b><span> — Snowflake Cortex Code can only access and act on what your active role permits. If your role can&#8217;t read a table, Cortex Code cannot read it either. </span><span> </span></li>
</ul>
<ul>
<li><b><span>Column-level masking policies respected</span></b><span> — Sensitive columns that are masked for your role remain masked. It won&#8217;t accidentally expose sensitive columns to the LLM in unmasked form.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Row access policies respected</span></b><span> — Filtered rows stay filtered. If a row access policy filters rows from your session, those rows are invisible to Snowflake Cortex Code too.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Object scope limitation</span></b><span> — The agent operates only within the scope of objects accessible to the current session role. It can&#8217;t reach beyond the objects your privileges cover.</span><span> </span></li>
</ul>
<ul>
<li><b><span>OS-level sandboxing (CLI)</span></b><span> — Snowflake Cortex Code CLI operates within sandboxed boundaries and includes a three-tier approval system with automatic risk assessment, which categorizes actions before they&#8217;re executed.</span><span> </span></li>
</ul>
<ul>
<li><b><span>No data leakage to external providers</span></b><span> — Your data stays in Snowflake. Model inference is Snowflake-managed. Your data stays in your environment.</span><span> </span></li>
</ul>
<p><span>The security model is only as good as your Snowflake RBAC configuration. If your team has been lazy about role scoping, Snowflake Cortex Code will have broad access because your role does. Tighten your </span><a href="https://www.flexera.com/blog/finops/snowflake-roles-access-control/"><span>access control</span></a><span> before you expand AI tooling across your organization.</span><span> </span></p>
<h3><span>Where code actually executes</span><span> </span></h3>
<p><span>It&#8217;s important to be clear here. The phrase “AI runs your code” might sound scary. Here&#8217;s what&#8217;s actually going on with Snowflake Cortex Code:</span><span> </span></p>
<ul>
<li><span>Generated SQL runs on your virtual warehouses (or serverless compute, depending on object type). And do not worry, the billing and compute rules are the same as if you had written the query from scratch.</span><span> </span></li>
</ul>
<ul>
<li><span>CLI conversations run locally. Session IDs and conversation metadata may be stored locally.</span><span> </span></li>
</ul>
<ul>
<li><span>Cortex inference (the model calls) runs on Snowflake&#8217;s Cortex AI infrastructure.</span><span> </span></li>
</ul>
<blockquote>
<p><b><span>TL; DR</span></b><span>: Snowflake Cortex Code is a context-aware Snowflake agent. It plans a sequence of actions (queries, code edits, calls to Snowflake AI functions) and then executes them within your account. All inference happens on Snowflake’s Cortex nodes, and all queries run on your Snowflake compute, so your data never leaves Snowflake.</span><span> </span></p>
</blockquote>
<h2><span>Four core pillars of Snowflake Cortex Code</span><span> </span></h2>
<p><span>Snowflake Cortex Code is built around four core principles:</span><span> </span></p>
<h3><span>1) Intelligence (knows Snowflake)</span><span> </span></h3>
<p><span>Snowflake Cortex Code is a built-in expert on Snowflake. Cortex Code uses agentic orchestration. It interprets intent, creates a plan, selects tools and executes steps autonomously. It doesn&#8217;t just respond to a single prompt; it maintains context across a session and handles multi-step workflows. On top of that, it also understands Snowflake’s features and terminology (databases, schemas, tables, data types, views, semantic models). It learns about your environment (schema, tags, pipelines) so it can write valid, efficient code.</span><span> </span></p>
<h3><span>2) Relevance (context-aware)</span><span> </span></h3>
<p><span>Context awareness is built in. Snowflake Cortex Code knows which file you have opened, which role you are using, what objects exist in your account and what your semantic models say. It never treats each request in isolation. Responses are grounded in your environment, not generic Snowflake knowledge alone. </span><span> </span></p>
<h3><span>3) Integration (works with your stack)</span><span> </span></h3>
<p><span>Snowflake Cortex Code fits into your existing workflow and tools. It handles </span><a href="https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake"><span>dbt projects</span></a><span>, </span><a href="https://www.flexera.com/blog/finops/streamlit-in-snowflake/"><span>Streamlit apps</span></a><span>, </span><a href="https://www.flexera.com/blog/finops/snowflake-notebooks/"><span>SQL/Python notebooks</span></a><span>, etc. It knows how to use your code files directly (via the CLI) or route to the right UI surface. It can orchestrate multi-step pipelines (generate data transformations, tests and docs for a dbt project), work with version control (via bash/git in CLI) and even pull in external tools.</span><span> </span></p>
<h3><span>4) Governance (secure by design)</span><span> </span></h3>
<p><span>From day one, the Snowflake Cortex Code was built with enterprise governance in mind. Everything Cortex Code does is governed by the same controls as everything else in your account. It uses the Snowflake RBAC model, so it only sees/acts on what your role permits. All outputs stick to row-level security and masking policies. Every action (even what it writes to your DB) happens under your account’s privileges. There is no governance bypass, no shadow execution and no access to data your role cannot touch. For regulated environments, that means Snowflake Cortex Code is safe to try on real data.</span><span> </span></p>
<p><span>Next up, we will dive into the main core features of Snowflake Cortex Code.</span><span> </span></p>
<h2><span>Power of Snowflake Cortex Code—features and use cases</span><span> </span></h2>
<p><span>Snowflake Cortex Code offers a bunch of AI-powered capabilities for data workflows. The most notable ones are:</span><span> </span></p>
<ul>
<li><b><span>Natural language ⇒ SQL/Python</span></b><span> — Simply describe what you want to do. You just have to describe what you want in plain natural language, and Snowflake Cortex Code writes the query or script. The difference from a generic assistant is that it can reference your actual tables by name, apply your schema conventions and generate queries that are accurate for your data model; not a generic approximation.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Context awareness</span></b><span> — The assistant leverages your environment context. In Snowflake workspaces, it knows which file or cell you are editing. In CLI, you can </span><span>@-inject code</span><span> files or </span><span>#-inject</span><span> tables. Because it “sees” your Snowflake catalog and Snowflake workspace content, it can write more accurate code and answer questions grounded in your actual data.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Code refactoring and explanation</span></b><span> — Snowflake Cortex Code can not only generate code but also improve/fix it. You can ask it to optimize a slow query, fix bugs, or add new logic. In Snowflake Snowsight, it can auto-format SQL or add missing JOINs. You can highlight existing SQL and ask for quick actions (formatting, explain, or “Fix” on error results). It can even comment your code or convert between SQL and other file types. In short, it’s a conversational IDE assistant.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Automatic error fixing</span></b><span> — If a query fails, Snowflake Snowsight results grid shows a &#8220;Fix&#8221; button. Clicking this will ask Snowflake Cortex Code to diagnose the error and suggest a correction.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Diff Review (Snowsight only)</span></b><span> — When Cortex Code suggests code changes in Snowflake workspaces, it shows them in a side-by-side Diff View. Insertions and deletions are highlighted, so you can audit exactly what the agent wants to do before you accept. The Diff View works for any code edits Cortex Code applies, making it safe to iterate on suggestions.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Multi-step orchestration</span></b><span> — This is where the “agentic” label becomes true. Snowflake Cortex Code plans and executes multi-step tasks intelligently. You might ask it to build a Streamlit dashboard on a dataset. It will generate code, create new files, fetch sample data and even assemble the layout. Each step is orchestrated by the agent, so you do not have to manually carry snippets between tools. It can also chain actions, such as creating a new Snowflake notebook, adding code cells, plotting charts and explaining results, all without interrupting your workflow.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Local file and tool access (CLI)</span></b><span> — Snowflake Cortex Code CLI can read and write local project files and run command-line tools. This makes it practical for managing dbt projects, generating or modifying Streamlit apps, running git operations and orchestrating end-to-end workflows that span your repo and your Snowflake account. The Coretex Code CLI also lets you run shell commands with </span><span>!</span><span>  directly inside a Cortex chat; the output feeds back into the agent for analysis. This makes it easy to integrate your normal dev tools.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Agent customization (AGENTS.md)</span></b><span> — You can create an </span><span>AGENTS.md</span><span> file in your project root with custom instructions or guidelines. Snowflake Cortex Code will automatically include the contents of AGENTS.md in every conversation context.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Custom Agent Skills ($ syntax)</span></b><span> — Snowflake Cortex Code CLI supports Agent Skills, which are like plugins for common tasks. Skills are invoked with a </span><span>$</span><span> or via the </span><span>/skill</span><span> command. This lets you build repeatable workflows that other team members can use without knowing the internals.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Web search integration (optional)</span></b><span> — You can enable a web search feature, so Snowflake Cortex Code can include relevant web results in its reasoning. This is especially useful when you want it to reference current Snowflake documentation, external datasets, or industry context alongside your internal data. Web search is off by default and must be explicitly enabled (see the setup guide below).</span><span> </span></li>
</ul>
<ul>
<li><b><span>Semantic model (Snowflake Cortex Analyst) support</span></b><span> — If you are using Snowflake Cortex Analyst with semantic views or YAML-based semantic models, Snowflake Cortex Code can validate the model, identify ambiguities or inconsistencies and help you debug why a natural language question isn&#8217;t returning the right SQL. This is useful for teams managing complex semantic layers. A 500-line semantic view definition is extremely hard to audit manually. Snowflake Cortex Code can analyze the whole thing, generate an entity-relationship diagram, flag conflicting metric definitions and suggest clarifications.</span><span> </span></li>
</ul>
<ul>
<li><b><span>dbt project automation</span></b><span> — There is a powerful agentic workflow for dbt on Snowflake. Snowflake Cortex Code can automatically scaffold staging models from raw data, build full dbt DAGs, add data tests and generate documentation.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Streamlit app generation</span></b><span> — You can easily describe a dashboard you want, and Snowflake Cortex Code will generate a Streamlit app against your specified data source, including filters, charts and interactive elements. Combine this with Snowpark Container Services for deployment within Snowflake.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Model Context Protocol support</span></b><span> — Snowflake Cortex Code CLI supports the model context protocol (MCP), which means you can configure external MCP servers to extend Cortex Code with additional tools and data sources beyond its built-in Snowflake skills.</span><span> </span></li>
</ul>
<ul>
<li><b><span>Plan Mode (CLI)</span></b><span> — In the Snowflake Cortex Code CLI, you can toggle /plan mode. In this mode, the agent pauses before executing any action and presents a “plan” of what it intends to do. It will ask you to confirm each step, which is great for complex or risky tasks, so you can sanity-check everything before it changes your system.</span><span> </span></li>
</ul>
<h2><span>Pricing and cost structure of Snowflake Cortex Code</span><span> </span></h2>
<p><span>Snowflake Cortex Code is priced on a pay-as-you-go basis. In practice, that means using Cortex Code CLI will consume Snowflake credits per token of text processed, while Snowflake Snowsight (web UI) version is free during preview. In short, we only pay for what the AI does, not a flat fee. The key thing to know is that each question you ask or code you generate uses tokens, and Snowflake bills credits per million input/output tokens. </span><b><span>Snowflake Cortex Code in Snowflake Snowsight (the in-browser agent) is currently free</span></b><span>, but once it is generally available, it will also be billed similarly.</span><span> </span></p>
<p><span>Aside from token charges, Snowflake Cortex Code still triggers regular Snowflake compute. Any metadata or SQL operations the agent runs (like </span><a href="https://docs.snowflake.com/en/sql-reference/sql/desc-table"><span>DESCRIBE TABLE</span></a><span>, querying </span><a href="https://docs.snowflake.com/en/sql-reference/account-usage"><span>SNOWFLAKE.ACCOUNT_USAGE</span></a><span>). consume your normal warehouse and Cloud Services credits. In fact, Snowflake notes that Cortex Code “charges standard Cloud Services compute costs for accessing metadata” – basically, your existing query costs. But, as of early 2026, Snowflake is not charging extra fees for the Snowflake Cortex Code feature itself. The only “new” charges on your invoice come from the LLM calls (the token usage). In other words, you will see Cortex Code usage under the AI_SERVICES category on your bill (along with any warehouse credits spent on its queries).</span><span> </span></p>
<p><span>Snowflake publishes the exact rates in its Service Consumption Table. Snowflake Cortex Code currently uses Anthropic’s Claude models, each with its own credit-per-token rate. The table of prices (credits per 1 million tokens) includes:</span><span> </span></p>
<pre><b><span>Table 1</span></b><span>: Credit Consumption Rates for Snowflake AI Features / Snowflake Cortex Code  (Per 1M Tokens)</span><span> </span></pre>
<table>
<tbody>
<tr>
<td><b><span>Model</span></b><span> </span></td>
<td><b><span>Input (Credits/1M tokens)</span></b><span> </span></td>
<td><b><span>Output (Credits/1M tokens)</span></b><span> </span></td>
<td><b><span>Cache Write</span></b><span> </span></td>
<td><b><span>Cache Read</span></b><span> </span></td>
</tr>
<tr>
<td><span>claude-4-sonnet</span><span> </span></td>
<td><span>1.50</span><span> </span></td>
<td><span>7.50</span><span> </span></td>
<td><span>1.88</span><span> </span></td>
<td><span>0.15</span><span> </span></td>
</tr>
<tr>
<td><span>claude-opus-4-5</span><span> </span></td>
<td><span>2.75</span><span> </span></td>
<td><span>13.75</span><span> </span></td>
<td><span>3.44</span><span> </span></td>
<td><span>0.28</span><span> </span></td>
</tr>
<tr>
<td><span>claude-opus-4-6</span><span> </span></td>
<td><span>2.75</span><span> </span></td>
<td><span>13.75</span><span> </span></td>
<td><span>3.44</span><span> </span></td>
<td><span>0.28</span><span> </span></td>
</tr>
<tr>
<td><span>claude-sonnet-4-5</span><span> </span></td>
<td><span>1.65</span><span> </span></td>
<td><span>8.25</span><span> </span></td>
<td><span>2.07</span><span> </span></td>
<td><span>0.17</span><span> </span></td>
</tr>
</tbody>
</table>
<p><span> </span><span>So, let’s say you send 1 million input tokens to the Claude-4-sonnet model; it costs 1.50 credits, and receiving 1 million output tokens costs 7.50 credits. A token is roughly a few characters of text, so typical prompts/responses are a small fraction of a million tokens. Writing to or reading from the agent’s long-term cache (memory) also uses credits (as shown above). Model choice matters: the Opus models are more expensive per token than Sonnet. If one Snowflake credit is ~$3, then 1 million output tokens with Claude-Opus 4.5 would cost about ~$40. A few thousand tokens (a typical short query and answer) would be only a few cents.</span><span> </span></p>
<p><span>Keep in mind the total Cortex Code cost is (</span><b><span>AI token credits + normal compute credits</span></b><span>). In practice, you should track your AI_SERVICES credit usage in Snowflake’s billing console – this will capture the Cortex Code token fees. Your regular warehouse and Cloud Services usage for any queries the agent runs will show up under the normal compute categories.</span><span> </span></p>
<blockquote><p><b><span>TL; DR</span></b><span>: Snowflake Cortex Code CLI is metered by token consumption, and Snowflake Snowsight use is free for now.</span></p></blockquote>
<h2><span>Snowflake Intelligence vs Snowflake Cortex Code</span><span> </span></h2>
<p><span>Snowflake Intelligence</span><span> and Snowflake Cortex Code are frequently confused because both involve natural language, and both live inside the Cortex ecosystem. They serve different users and solve different problems. Here are the key differences between them.</span></p>
<pre><b><span>Table 2</span></b><span>: Difference between Snowflake Intelligence vs Snowflake Cortex Code</span><span> </span></pre>
<table style="width: 90.2496%;">
<tbody>
<tr>
<td style="width: 38.4874%;">
<p style="text-align: center;"><b><span>Snowflake Intelligence</span></b><span> </span></p>
</td>
<td style="width: 23.9072%;">
<p style="text-align: center;"><b><span>🔮</span></b><span> </span></p>
</td>
<td style="width: 37.6894%;">
<p style="text-align: center;"><b><span>Snowflake Cortex Code</span></b><span> </span></p>
</td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>Conversational BI agent for business analysts. Ask complex data questions and get answers with charts or explanations. Supports rich queries over structured and unstructured data in one chat</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Primary use</span><span> </span></td>
<td style="width: 37.6894%;"><span>AI-assisted development workflow – helps data engineers and analysts author code, explore data and manage accounts. Generates, modifies and explains SQL/Python code in Snowflake, and can automate admin tasks</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>Business analysts, non-technical users, executives</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Primary user</span><span> </span></td>
<td style="width: 37.6894%;"><span>Data engineers, analytics engineers, data scientists, DBAs</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>Insights, answers, charts, recommendations</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Output type</span><span> </span></td>
<td style="width: 37.6894%;"><span>SQL, Python, dbt models, Streamlit apps, agent code</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>Snowflake Snowsight conversational UI</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Primary interface</span><span> </span></td>
<td style="width: 37.6894%;"><span>Snowflake Snowsight panel + local CLI</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>Natural language Q&amp;A over governed data</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Interaction mode</span><span> </span></td>
<td style="width: 37.6894%;"><span>Agentic coding, code generation, refactoring and debugging</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>Minimal—answers and visualizations</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Code output</span><span> </span></td>
<td style="width: 37.6894%;"><span>SQL, Python, dbt models, Streamlit apps, YAML semantic models</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>No</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Local file access</span><span> </span></td>
<td style="width: 37.6894%;"><span>Yes (via Snowflake Cortex Code CLI)</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>No</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Code Diff View</span><span> </span></td>
<td style="width: 37.6894%;"><span>Yes (Snowsight)</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>No</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>dbt/pipeline automation</span><span> </span></td>
<td style="width: 37.6894%;"><span>Yes</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>RBAC + semantic model</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Governance scope</span><span> </span></td>
<td style="width: 37.6894%;"><span>Full RBAC + masking + row access + catalog</span><span> </span></td>
</tr>
<tr>
<td style="width: 38.4874%;"><span>Uses Snowflake’s consumption model. There is no separate “seat” or add-on fee – you pay for AI service usage by the credit. Behind the scenes, each intelligence request incurs Cortex Analyst or Cortex Search credits</span><span> </span></td>
<td style="width: 23.9072%; text-align: center;"><span>Pricing/licensing</span><span> </span></td>
<td style="width: 37.6894%;"><span>Snowflake Snowsight (UI) version is free for now (no charge until announced otherwise). Cortex Code CLI (local) is charged per AI token consumption. You also pay normal Snowflake compute/storage fees.</span><span> </span></td>
</tr>
</tbody>
</table>
<h2><span>Snowflake Cortex Code vs Snowflake Copilot (legacy Copilot)</span><span> </span></h2>
<p><a href="https://www.flexera.com/blog/finops/snowflake-copilot/"><span>Snowflake Copilot</span></a><span> was the first AI assistant in Snowflake Snowsight (a basic chat panel for SQL help). It is now deprecated and replaced by the Snowflake Cortex Code. Here are the differences:</span><span> </span></p>
<pre><b><span>Table 3</span></b><span>: Difference between Snowflake Cortex Code vs Snowflake Copilot (legacy Copilot)</span><span> 
</span></pre>
<table style="width: 90.2496%;">
<tbody>
<tr>
<td style="width: 39.1597%;">
<p style="text-align: center;"><b><span>Snowflake Cortex Code</span></b><span> </span></p>
</td>
<td style="width: 20.4202%;">
<p style="text-align: center;"><b><span>🔮</span></b><span> </span></p>
</td>
<td style="width: 40.4202%;">
<p style="text-align: center;"><b><span>Snowflake Copilot</span></b><span> </span></p>
</td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>AI-assisted development workflow. It helps data engineers and analysts to write code, explore data and manage accounts. Generates, modifies and explains SQL/Python code in Snowflake, and can automate admin tasks</span><span> </span></td>
<td style="width: 20.4202%;">
<p style="text-align: center;"><span>Primary use</span><span> </span></p>
</td>
<td style="width: 40.4202%;"><span>Earlier SQL assistant in Snowflake Snowsight – provided basic SQL help and docs lookup. Focused on guiding new users with simple query suggestions and UI tips</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Built into the Snowflake developer experience. Snowflake Snowsight Workspaces (web) shows a Cortex Code icon in code/Snowflake notebook files, and there’s also a Cortex Code CLI for local dev environments. It plugs into VS Code or terminals, connecting to your Snowflake account</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Integration/interface</span><span> </span></td>
<td style="width: 40.4202%;"><span>Embedded in Snowflake Snowsight: you use the Ask Snowflake Copilot panel in Snowflake worksheets and Snowflake notebooks. It was a sidebar assistant for SQL and basic help</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Supports data engineering and ML workflows end-to-end. Use it to write or refactor queries, build dbt transformations, spin up notebooks, debug pipelines and even handle admin queries</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Workflows/tasks</span><span> </span></td>
<td style="width: 40.4202%;"><span>Covered basic data analysis workflows: natural-language to SQL, insights from data and simple UI help. It could scan available tables and suggest SQL or plot snippets, but did not write complex notebooks or handle rich documents</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Active (CLI: GA; Snowflake Snowsight: Open Preview)</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Status</span><span> </span></td>
<td style="width: 40.4202%;"><span>Deprecated</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Full CLI with local file access</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>CLI support</span><span> </span></td>
<td style="width: 40.4202%;"><span>None</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Yes</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>dbt support</span><span> </span></td>
<td style="width: 40.4202%;"><span>No</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Yes (CLI)</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>MCP integration</span><span> </span></td>
<td style="width: 40.4202%;"><span>No</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Yes (CLI)</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Plan mode</span><span> </span></td>
<td style="width: 40.4202%;"><span>No</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Yes (Snowsight)</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Diff View</span><span> </span></td>
<td style="width: 40.4202%;"><span>No</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Agent Skills system</span><span> </span></td>
<td style="width: 40.4202%;"><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Yes (requires admin enablement)</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Web search</span><span> </span></td>
<td style="width: 40.4202%;"><span>No</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Advanced code generation and editing – can translate NL prompts into SQL/Python code, insert or remove lines (with a Diff View for review) and explain existing scripts. It also integrates with Git, bash, or dbt commands in the CLI. Customizable via an AGENTS.md file. Everything runs under Snowflake security (no secret leakage)</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Main key features</span></p>
<p><span> </span></p>
<p><span> </span></td>
<td style="width: 40.4202%;"><span>Basic SQL suggestions and answers – it answered simple how-to questions about your data, generated or improved single SQL queries and explained Snowflake features. It was strictly confined to your accessible tables and had minimal context beyond that. Its interaction was more limited than Cortex Code or Intelligence</span><span> </span></td>
</tr>
<tr>
<td style="width: 39.1597%;"><span>Snowflake Snowsight (UI) version is free for now (no charge until announced otherwise). The Cortex Code CLI (local) is charged per AI token consumption (see Snowflake’s Service Consumption Table). You also pay normal Snowflake compute/storage fees.</span><span> </span></td>
<td style="width: 20.4202%; text-align: center;"><span>Pricing/licensing</span><span> </span></td>
<td style="width: 40.4202%;"><span>Snowflake Copilot was free to use for all eligible Snowflake customers. It ran on Snowflake’s managed Cortex AI service behind the scenes, but users saw no extra billing line.</span><span> </span></td>
</tr>
</tbody>
</table>
<blockquote><p><b style="font-size: 1.25rem;"><span>TL; DR</span></b><span style="font-size: 1.25rem;">: Snowflake Copilot was an early, simpler chat helper for SQL. Snowflake Cortex Code is its powerful replacement: a full coding agent that runs in Snowflake workspaces and the CLI. (Note: if an account had Snowflake Copilot disabled, Cortex Code will also be disabled for that account</span><span style="font-size: 1.25rem;"> </span></p></blockquote>
<h2><span>Step-by-step guide to set up and configure Snowflake Cortex Code (via Snowsight)</span><span> </span></h2>
<p><span>Let us dive right into the main part of the article. Here, we will set up and configure Snowflake Cortex Code from scratch directly within the Snowflake Snowsight web interface.</span><span> </span></p>
<h3><span>Prerequisites and setups:</span><span> </span></h3>
<p><span>Here is what you will need to have ready before getting started.</span><span> </span></p>
<ul>
<li><span>A Commercial Snowflake account (non-Government, non-Sovereign, non-VPS) with Cortex AI enabled.</span><span> </span></li>
</ul>
<ul>
<li><span>A Snowflake user with access to Snowflake Snowsight (the web UI).</span><span> </span></li>
</ul>
<ul>
<li><a href="https://docs.snowflake.com/en/user-guide/security-access-control-considerations#using-the-accountadmin-role"><span>ACCOUNTADMIN role</span></a><span> access for enabling cross-region inference and web search</span><span> </span></li>
</ul>
<ul>
<li><a href="https://docs.snowflake.com/en/user-guide/snowflake-cortex/cross-region-inference"><span>Cross-region inference enabled</span></a><span> on your account (Snowflake Cortex Code won&#8217;t function without this)</span><span> </span></li>
</ul>
<ul>
<li><span>Two specific database roles granted to your user&#8217;s role:</span><span> </span>
<ul>
<li><span>SNOWFLAKE.CORTEX_USER</span><span> </span></li>
<li><span>SNOWFLAKE.COPILOT_USER</span><span> </span></li>
</ul>
</li>
</ul>
<blockquote><p><span>⚠️ Important (Legacy Copilot Accounts – If your account had Copilot (legacy) disabled, Snowflake will also disable Cortex Code on that account, so ask your admin to re-enable it if needed.</span><span> </span></p></blockquote>
<h3><b><span>Step 1</span></b><span>—Log in to Snowflake Snowsight</span><span> </span></h3>
<p><span>Open a browser and navigate to your Snowflake account URL (</span><span>https://&lt;&#8230;..&gt;.snowflakecomputing.com</span><span>) and log in with your Snowflake credentials. Make sure you are using an account in the commercial tier (not a government or VPS deployment).</span><span> </span></p>
<h3><b><span>Step 2</span></b><span>—Enable cross-region inference (ACCOUNTADMIN required)</span><span> </span></h3>
<p><span>Cross-region inference is a Snowflake account-level parameter that permits the Cortex AI service to route inference requests to whichever Snowflake cloud region hosts the requested model, rather than restricting requests to your account&#8217;s home region. This is a hard requirement for Snowflake Cortex Code.</span><span> </span></p>
<p><span>If you haven’t already done this as part of the prerequisites, run this SQL as ACCOUNTADMIN to turn on cross-region AI model access:</span><span> </span></p>
<pre class="codebox"><span>ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'ANY_REGION';</span><span> </span><span> </span></pre>
<div class="wp-caption aligncenter" id="attachment_34119" style="width: 435px;"><img alt="Executing the ALTER ACCOUNT command to enable global cross-region inference routing—Snowflake Cortex Code " class="wp-image-34119 size-full" height="211" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-2.png" width="425" /><p class="wp-caption-text" id="caption-attachment-34119">Figure 2: Executing the ALTER ACCOUNT command to enable global cross-region inference routing—Snowflake Cortex Code</p></div>
<p><span>As you can see, this single command lets Snowflake route Cortex Code requests to any available cloud region (ANY_REGION) so you get access to top models like Claude Sonnet or Opus. You can also use a specific region, like:</span><span> </span></p>
<ul>
<li><span>AWS_US (AWS US regions. It is recommended for Claude Sonnet 4.5 and above)</span><span> </span></li>
</ul>
<ul>
<li><span>AWS_EU (AWS EU regions)</span><span> </span></li>
</ul>
<ul>
<li><span>AWS_APJ (AWS Asia-Pacific/Japan regions)</span><span> </span></li>
</ul>
<p><span>Replace ANY_REGION with the identifier for the target region if needed. Once this is run, Snowflake will handle model routing automatically. No need to restart anything; the change is almost immediate.</span></p>
<h3><b><span>Step 3</span></b><span>—Grant the required database role to your user role (if not done)</span><span> </span></h3>
<p><span>Snowflake Cortex Code access in Snowflake Snowsight is governed by two Snowflake-managed database roles. These roles are defined within the built-in SNOWFLAKE database and must be granted to the Snowflake role that your user will be operating under. The two required roles are:</span><span> </span></p>
<ul>
<li><span>SNOWFLAKE.CORTEX_USER</span><span> </span></li>
</ul>
<ul>
<li><span>SNOWFLAKE.COPILOT_USER</span><span> </span></li>
</ul>
<p><span>The recommended approach is to create a dedicated role for Cortex Code access, grant the required database roles to it and then assign that role to the relevant users. Perform the following steps as ACCOUNTADMIN or SECURITYADMIN:</span><span> </span></p>
<p><span>Create a new role to serve as the Cortex Code access role.</span><span> </span></p>
<pre class="codebox"><span>CREATE ROLE IF NOT EXISTS cortex_code_role;</span></pre>
<div class="wp-caption aligncenter" id="attachment_34120" style="width: 364px;"><img alt="Creating a new custom role specifically for Snowflake Cortex Code usage " class="wp-image-34120 size-full" height="166" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-3.png" width="354" /><p class="wp-caption-text" id="caption-attachment-34120">Figure 3: Creating a new custom role specifically for Snowflake Cortex Code usage</p></div>
<p><span>Once the role is created, now grant it. Grant both Snowflake-managed database roles to cortex_code_role. The </span><a href="https://docs.snowflake.com/en/sql-reference/sql/grant-database-role"><span>GRANT DATABASE ROLE syntax</span></a><span> (not GRANT ROLE) is required because </span><span>SNOWFLAKE.CORTEX_USER</span><span> and </span><span>SNOWFLAKE.COPILOT_USER</span><span> are database-level roles defined within the SNOWFLAKE shared database, not an account-level roles:</span><span> </span></p>
<pre class="codebox"><span>GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER   TO ROLE cortex_code_role;</span><span> </span>

<span>GRANT DATABASE ROLE SNOWFLAKE.COPILOT_USER  TO ROLE cortex_code_role;</span><span> </span><span> </span></pre>
<div class="wp-caption aligncenter" id="attachment_34121" style="width: 497px;"><img alt="Granting the SNOWFLAKE system database roles to the custom developer role—Snowflake Cortex Code " class="wp-image-34121 size-full" height="178" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-4.png" width="487" /><p class="wp-caption-text" id="caption-attachment-34121">Figure 4: Granting the SNOWFLAKE system database roles to the custom developer role—Snowflake Cortex Code</p></div>
<p><span>Assign the </span><span>cortex_code_role</span><span> to the Snowflake user who will be using Snowflake Cortex Code. Replace </span><span>&lt;username&gt;</span><span> with the actual Snowflake login name:</span><span> </span></p>
<pre class="codebox">G<span>RANT ROLE cortex_code_role TO USER &lt;username&gt;;</span><span> </span></pre>
<div class="wp-caption aligncenter" id="attachment_34122" style="width: 421px;"><img alt="Assigning the fully configured cortex_code_role to the target user account—Snowflake Cortex Code " class="wp-image-34122 size-full" height="154" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-5.png" width="411" /><p class="wp-caption-text" id="caption-attachment-34122">Figure 5: Assigning the fully configured cortex_code_role to the target user account—Snowflake Cortex Code</p></div>
<p><span>Now switch to the role that you just created:</span><span> </span></p>
<pre class="codebox"><span>USE ROLE cortex_code_role;</span></pre>
<div class="wp-caption aligncenter" id="attachment_34123" style="width: 296px;"><img alt="Switching the active session context to the newly created cortex_code_role—Snowflake Cortex Code " class="wp-image-34123 size-full" height="169" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-6.png" width="286" /><p class="wp-caption-text" id="caption-attachment-34123">Figure 6: Switching the active session context to the newly created cortex_code_role—Snowflake Cortex Code</p></div>
<p><span>After running these, verify your session’s current role includes those roles (for example, via </span><span>SHOW ROLES;</span><span>).</span><span> </span></p>
<h3><b><span>Step 4</span></b><span>—Enable web search (ACCOUNTADMIN)</span><span> </span></h3>
<p><span>Snowflake Cortex Code can optionally use real-time web search as a tool during inference, which significantly improves the quality of responses for questions involving recently released features, third-party library documentation, or external data context not available in your Snowflake environment. Web search is an account-level toggle that must be enabled by an ACCOUNTADMIN. It does not require any per-user configuration once enabled.</span><span> </span></p>
<p><span>To enable web search:</span><span> </span></p>
<ul>
<li><span>Log in to Snowflake Snowsight with a user whose active role is ACCOUNTADMIN</span><span> </span></li>
</ul>
<ul>
<li><span>In the left navigation pane, select </span><b><span>Admin </span></b><span>&gt; </span><b><span>AI &amp; ML</span></b><span> &gt; </span><b><span>Agents </span></b><span>&gt; </span><b><span>Settings</span></b><span> </span></li>
</ul>
<div class="wp-caption aligncenter" id="attachment_34124" style="width: 376px;"><img alt="Navigating to the AI &amp; ML settings menu within Snowflake Snowsight Administration panel—Snowflake Cortex Code " class="wp-image-34124 size-full" height="428" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-7.png" width="366" /><p class="wp-caption-text" id="caption-attachment-34124">Figure 7: Navigating to the AI &amp; ML settings menu within Snowflake Snowsight Administration panel—Snowflake Cortex Code</p></div>
<ul>
<li><span>On the Settings page, locate the Web Search toggle and set it to Enabled</span><span> </span></li>
</ul>
<div class="wp-caption aligncenter" id="attachment_34125" style="width: 878px;"><img alt="Enabling the Web Search capability in the Cortex settings—Snowflake Cortex Code " class="wp-image-34125 size-full" height="163" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-8.png" width="868" /><p class="wp-caption-text" id="caption-attachment-34125">Figure 8: Enabling the Web Search capability in the Cortex settings—Snowflake Cortex Code</p></div>
<p><span>Once web search is enabled at the account level, all users with the Cortex Code access roles will benefit from it automatically.</span><span> </span></p>
<h3><b><span>Step 5</span></b><span>—Open a Snowflake workspace and access the Snowflake Cortex Code panel</span><span> </span></h3>
<p><span>In the Snowflake Snowsight left navigation pane, select </span><b><span>Projects </span></b><span>&gt; </span><b><span>Workspaces</span></b><span>. Open an existing Snowflake workspace that contains one or more SQL files or create a new one with a new SQL worksheet.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34126" style="width: 1463px;"><img alt="Locating the Snowflake Cortex Code floating action button in the bottom-right of the worksheet " class="wp-image-34126 size-full" height="967" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-9.png" width="1453" /><p class="wp-caption-text" id="caption-attachment-34126">Figure 9: Locating the Snowflake Cortex Code floating action button in the bottom-right of the worksheet</p></div>
<p><span>Once inside the Snowflake workspace, look for the Snowflake Cortex Code icon (a star </span><span>✦</span><span> icon) located in the lower-right corner of Snowflake workspace editor area.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34127" style="width: 847px;"><img alt="Snowflake Cortex Code side panel expanding next to the SQL editor " class="wp-image-34127 size-full" height="946" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-10.png" width="837" /><p class="wp-caption-text" id="caption-attachment-34127">Figure 10: Snowflake Cortex Code side panel expanding next to the SQL editor</p></div>
<p><span>Click the star </span><span>✦ </span><span>icon. Snowflake Cortex Code panel will slide open on the right side of the Snowflake workspace, like a chat sidebar. It contains a message history area at the top and a text input box at the bottom.</span><span> </span></p>
<p><span>If the icon is not visible, confirm that the COPILOT_USER and CORTEX_USER roles are properly granted and that cross-region inference is enabled.</span><span> </span></p>
<h3><b><span>Step 6</span></b><span>—Submit your prompt</span><span> </span></h3>
<p><span>Now that you have your Snowflake Cortex Code panel open, start interacting with the AI assistant. Click inside the message input box at the bottom of the panel and type a natural-language prompt. Press Enter or click the send icon to submit. For best results, include fully qualified object names (</span><span>database.schema.table</span><span>) and sufficient context about what you want to achieve.</span><span> </span></p>
<p><span>The following two examples show the range of tasks Snowflake Cortex Code can handle.</span><span> </span></p>
<p><b><span>Example prompt 1</span></b><span> – What databases do I have access to?</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34128" style="width: 761px;"><img alt="Submitting a natural language question to the Cortex Code agent—Snowflake Cortex Code " class="wp-image-34128 size-full" height="584" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-11.png" width="751" /><p class="wp-caption-text" id="caption-attachment-34128">Figure 11: Submitting a natural language question to the Cortex Code agent—Snowflake Cortex Code</p></div>
<p><b><span>Example prompt 2</span></b><span> – Explain all the code and logic in the current SQL file I have open</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34129" style="width: 614px;"><img alt="Asking Snowflake Cortex Code to analyze and explain the SQL code currently visible in the editor " class="wp-image-34129 size-full" height="809" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-12.png" width="604" /><p class="wp-caption-text" id="caption-attachment-34129">Figure 12: Asking Snowflake Cortex Code to analyze and explain the SQL code currently visible in the editor</p></div>
<p><span>Snowflake Cortex Code follows a multi-stage reasoning process to respond to each prompt: it parses the user&#8217;s intent, plans the sequence of actions needed, retrieves relevant context from the open file and your Snowflake session environment and synthesises a response, all within a few seconds.</span><span> </span></p>
<h3><b><span>Step 7</span></b><span>—Review AI-Suggested changes</span><span> </span></h3>
<p><span>If your prompt involves modifying or generating SQL code, Snowflake Cortex Code may provide you a side-by-side Diff View within the panel (or inline in the editor), clearly distinguishing proposed additions from removals using standard diff colouring:</span><span> </span></p>
<ul>
<li><b><span>Insertions (new lines)</span></b><span> are highlighted in </span><span>green</span><span> </span></li>
</ul>
<ul>
<li><b><span>Deletions (removed lines)</span></b><span> are highlighted in </span><span>red</span><span> </span></li>
</ul>
<ul>
<li><b><span>Unchanged context lines</span></b><span> are shown without highlighting for reference</span><span> </span></li>
</ul>
<p><span>Carefully review this Diff View. The UI will usually show a checkbox or “Apply” button to accept the changes. Check out the differences to make sure the logic and syntax are correct. </span><span> </span></p>
<blockquote><p><b><span>Note</span></b><span>: Snowflake Cortex Code will not change your code without confirmation.</span><span> </span></p></blockquote>
<p><span>Don&#8217;t skip this step, especially for anything touching production workloads.</span><span> </span></p>
<h3><b><span>Step 8</span></b><span>—Execute SQL generated by Snowflake Cortex Code</span><span> </span></h3>
<p><span>Once you are happy with the changes, apply them. The updated SQL will now be in your worksheet. You can then run it as normal in Snowflake Snowsight (for example, by clicking the “Run” button or pressing Shift+Enter). Snowflake Cortex Code generates the SQL for you, but you still execute it in your Snowflake environment. If your prompt asks for a new query (rather than modifying an existing one), simply copy the generated SQL from the panel into a worksheet and run it.</span><span> </span></p>
<h3><b><span>Step 9</span></b><span>—Use follow-up prompts to refine and iterate</span><span> </span></h3>
<p><span>Snowflake Cortex Code maintains context across the session. The conversation does not end after one query. Cortex Code remembers context in your Snowflake workspace. You can follow up with questions, request modifications, add conditions, or ask it to explain what the generated SQL does. Each turn builds on the previous one. You do not have to start over every time.</span><span> </span></p>
<p><span>Use this iterative flow to improve code, convert between SQL and Python (for Snowflake notebooks), or even have Cortex Code perform administrative insights (like showing credit usage or lineage) all within Snowflake Snowsight.</span><span> </span></p>
<p><span>At this point, Snowflake Cortex Code is fully enabled and configured within your Snowsight environment. You have enabled cross-region inference, granted the necessary roles and know how to open the assistant panel. From here, you can enjoy an interactive coding assistant that can boost productivity and reduce manual effort.</span><span> </span></p>
<h2><span>Step-by-step guide to set up and configure Snowflake Cortex Code (via Snowflake Cortex Code CLI)</span><span> </span></h2>
<p><span>For power users or those who prefer a local development environment, Snowflake offers the Cortex Code CLI. It runs in your terminal and can interact with Snowflake directly. Here are the steps to install and configure it:</span><span> </span></p>
<h3><span>Prerequisites and setups:</span><span> </span></h3>
<p><span>Here is what you will need to have ready before getting started.</span><span> </span></p>
<ul>
<li><span>Commercial Snowflake account with cross-region inference enabled (see Step 2 above)</span><span> </span></li>
</ul>
<ul>
<li><span>Snowflake user with </span><span>SNOWFLAKE.CORTEX_USER</span><span> database role granted (see Step 3 above)</span><span> </span></li>
</ul>
<ul>
<li><span>A supported operating system: </span><a href="https://support.apple.com/en-us/116943"><span>macOS on Apple Silicon</span></a><span> or Intel x86_64, Linux on Intel x86_64, or Windows with </span><a href="https://learn.microsoft.com/en-us/windows/wsl/install"><span>Windows Subsystem for Linux 2, aka WSL2</span></a><span>. Native Windows (CMD or PowerShell) is not currently supported</span><span> </span></li>
</ul>
<ul>
<li><span>A local terminal running one of the supported shells: </span><a href="https://en.wikipedia.org/wiki/Bash_(Unix_shell)"><span>bash</span></a><span>, </span><a href="https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH"><span>zsh</span></a><span>, or </span><a href="https://fishshell.com/"><span>fish</span></a><span> </span></li>
</ul>
<ul>
<li><span>Network access to your Snowflake account over HTTPS (port 443). If your organization uses a proxy or private link configuration, ensure that outbound connections to your Snowflake account URL are permitted</span><span> </span></li>
</ul>
<ul>
<li><a href="https://www.flexera.com/blog/finops/snowflake-snowsql-cli/"><span>Snowflake CLI (snow)</span></a><span> is optional but recommended. If you have it installed and configured, the Snowflake Cortex Code CLI can reuse the connection profiles stored in </span><span>~/.snowflake/connections.toml</span><span>, which eliminates the need to re-enter account details. </span><b><span>Note</span></b><span>: Snowflake Cortex Code CLI and the Snowflake CLI (snow) share the same connection configuration file (~/.snowflake/connections.toml). Any connection that works with the Snowflake CLI will automatically be available for selection in the Cortex Code CLI setup wizard.</span><span> </span></li>
</ul>
<p><span>Now, let us jump into the step-by-step guide to configure Snowflake Cortex Code via CLI:</span><span> </span></p>
<h3><b><span>Step 1</span></b><span>—Install Snowflake Cortex Code CLI</span><span> </span></h3>
<p><span>Open your terminal and execute the official Snowflake install script using the following command:</span><span> </span></p>
<pre class="codebox"><span>curl -LsS https://ai.snowflake.com/static/cc-scripts/install.sh | sh</span><span> </span></pre>
<div class="wp-caption aligncenter" id="attachment_34130" style="width: 1772px;"><img alt="Executing the curl command to download and pipe the installation script to the shell—Snowflake Cortex Code CLI " class="wp-image-34130 size-full" height="488" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-13.png" width="1762" /><p class="wp-caption-text" id="caption-attachment-34130">Figure 13: Executing the curl command to download and pipe the installation script to the shell—Snowflake Cortex Code CLI</p></div>
<p><span>This command uses curl to download the installation shell script from Snowflake&#8217;s servers and pipes it directly to sh for execution. </span><span> </span></p>
<ul>
<li><span>-L</span><span> – Follows HTTP redirects automatically</span><span> </span></li>
</ul>
<ul>
<li><span>-s</span><span> – Operates in silent mode, suppressing the progress meter</span><span> </span></li>
</ul>
<ul>
<li><span>-S</span><span> – Re-enables error messages even in silent mode (so failures are visible)</span><span> </span></li>
</ul>
<p><span>The script detects your operating system and CPU architecture, downloads the appropriate cortex binary and installs it to </span><span>~/.local/bin/cortex</span><span> by default.</span><span> </span></p>
<p><span>After installation completes, verify that the binary is accessible from your shell by running:</span><span> </span></p>
<pre class="codebox">cortex --version</pre>
<div class="wp-caption aligncenter" id="attachment_34131" style="width: 858px;"><img alt="Verifying Snowflake Cortex Code CLI installation " class="wp-image-34131 size-full" height="156" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-14.png" width="848" /><p class="wp-caption-text" id="caption-attachment-34131">Figure 14: Verifying Snowflake Cortex Code CLI installation</p></div>
<p><span>If it throws an error, in that case, simply restart a new terminal session (so the profile changes are sourced)</span><span> </span></p>
<h3><b><span>Step 2</span></b><span>—Run the interactive setup wizard</span><span> </span></h3>
<p><span>Launch the Cortex Code CLI by simply typing Cortex and pressing Enter. On first launch, the CLI detects that no connection has been configured and automatically starts an interactive setup wizard that guides you through the configuration process. </span><span> </span></p>
<p><span>The wizard first asks you to select a text display style for the Cortex Code CLI interface. The available options control how code, markdown and AI responses are rendered in your terminal. Use the arrow keys to highlight your preferred option and press Enter to confirm.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34132" style="width: 2058px;"><img alt="Selecting the text display style within the Cortex interactive setup wizard—Snowflake Cortex Code CLI " class="wp-image-34132 size-full" height="623" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-15.png" width="2048" /><p class="wp-caption-text" id="caption-attachment-34132">Figure 15: Selecting the text display style within the Cortex interactive setup wizard—Snowflake Cortex Code CLI</p></div>
<p><span>Next, the wizard displays a security and data-handling notice. This notice explains how the Cortex Code CLI transmits prompts and query context to Snowflake&#8217;s Cortex AI service. Review the notice thoroughly before proceeding. To continue, confirm your acceptance when prompted.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34133" style="width: 1558px;"><img alt="Reviewing and accepting the security and data privacy usage terms—Snowflake Cortex Code CLI " class="wp-image-34133 size-full" height="882" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-16.png" width="1548" /><p class="wp-caption-text" id="caption-attachment-34133">Figure 16: Reviewing and accepting the security and data privacy usage terms—Snowflake Cortex Code CLI</p></div>
<h3><b><span>Step 3</span></b><span>—(Optional) Configure Snowflake connection manually</span><span> </span></h3>
<p><span>At this stage, the wizard prompts you to configure a Snowflake connection. You have two options available:</span><span> </span></p>
<p><b><span>Option A – Use an existing connection</span></b><span> </span></p>
<p><span>If you already have the Snowflake CLI installed and have previously configured connections in </span><span>~/.snowflake/connections.toml</span><span>, the wizard will automatically detect and display those named connection profiles. Use the arrow keys to navigate the list, highlight the desired connection and press </span><b><span>Enter </span></b><span>to select it. The wizard will then attempt to establish a test connection using the selected profile.</span><span> </span></p>
<p><b><span>Option B – Create a new connection manually</span></b><span> </span></p>
<p><span>If there are not any connections available or you want to set up a dedicated connection for Cortex Code, select “More options” from the bottom of the list. The wizard will then prompt you for your Snowflake account details step by step.</span><span> </span></p>
<p><span>To locate your account identifier, log in to app.snowflake.com, click your avatar in the bottom-left corner of the navigation pane, select Account and then View Account Details. Your account identifier follows the format </span><span>&lt;orgname&gt;-&lt;accountname&gt;</span><span> (myorg-myaccount). This is distinct from your account URL, which takes the form </span><span>https://myorg-myaccount.snowflakecomputing.com</span><span>.</span><span> </span></p>
<p><span>Either format is accepted by the Cortex Code CLI.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34134" style="width: 962px;"><img alt="Entering the Snowflake Account Identifier into the Snowflake Cortex Code CLI prompt " class="wp-image-34134 size-full" height="966" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-17.png" width="952" /><p class="wp-caption-text" id="caption-attachment-34134">Figure 17: Entering the Snowflake Account Identifier into the Snowflake Cortex Code CLI prompt</p></div>
<p><span>And next, enter the login name associated with your Snowflake user. This is the username you use to authenticate to Snowsight or via the Snowflake CLI.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34135" style="width: 962px;"><img alt="Entering the Snowflake username associated with the Cortex role—Snowflake Cortex Code CLI " class="wp-image-34135 size-full" height="640" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-18.png" width="952" /><p class="wp-caption-text" id="caption-attachment-34135">Figure 18: Entering the Snowflake username associated with the Cortex role—Snowflake Cortex Code CLI</p></div>
<p><span>The CLI supports multiple authentication mechanisms. For this guide, we will use a </span><a href="https://docs.snowflake.com/en/user-guide/programmatic-access-tokens"><span>programmatic access token (PAT)</span></a><span>, which is the most secure and convenient option for automated and CLI-based workflows. Select the third option from the authentication method list and press Enter.</span><span> </span></p>
<blockquote><p><b><span>🔐 Security Note</span></b><span>:  PATs are self-contained, time-limited bearer tokens issued by Snowflake. Unlike passwords, PATs can be scoped to a specific role and expire after a configurable number of days, reducing the impact of credential compromise.</span><span> </span></p></blockquote>
<div class="wp-caption aligncenter" id="attachment_34136" style="width: 928px;"><img alt="Selecting 'Programmatic Access Token' from the list of authentication mechanisms—Snowflake Cortex Code CLI " class="wp-image-34136 size-full" height="806" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-19.png" width="918" /><p class="wp-caption-text" id="caption-attachment-34136">Figure 19: Selecting &#8216;Programmatic Access Token&#8217; from the list of authentication mechanisms—Snowflake Cortex Code CLI</p></div>
<p><span>To generate a PAT, perform the following steps in the Snowflake Snowsight web interface:</span><span> </span></p>
<ul>
<li><span>In Snowflake Snowsight, open the navigation menu and select </span><b><span>Governance &amp; Security &gt; Users &amp; Roles</span></b><span> </span></li>
</ul>
<ul>
<li><span>Locate and click the user account for which you want to generate the token</span><span> </span></li>
</ul>
<ul>
<li><span>Scroll down to the </span><b><span>Programmatic Access Tokens</span></b><span> section and click Generate New Token</span><span> </span></li>
</ul>
<div class="wp-caption aligncenter" id="attachment_34137" style="width: 1215px;"><img alt="Navigating to the User Profile security settings to initiate token generation—Snowflake Cortex Code CLI " class="wp-image-34137 size-full" height="740" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-20.png" width="1205" /><p class="wp-caption-text" id="caption-attachment-34137">Figure 20: Navigating to the User Profile security settings to initiate token generation—Snowflake Cortex Code CLI</p></div>
<p><span>In the token generation dialog, complete the following fields:</span><span> </span></p>
<ul>
<li><b><span>Name</span></b><span>: A short, descriptive identifier for the token (CortexCodeCLI)</span><span> </span></li>
</ul>
<ul>
<li><b><span>Comment</span></b><span>: An optional free-text description of the token&#8217;s intended use</span><span> </span></li>
</ul>
<ul>
<li><b><span>Expiration</span></b><span>: The number of days until the token expires. Snowflake recommends 30 – 90 days for developer tokens</span><span> </span></li>
</ul>
<ul>
<li><b><span>Privilege Evaluation</span></b><span>: Select One Specific Role to restrict the token&#8217;s operations to a designated role. Selecting </span><i><span>Any of My Roles</span></i><span> causes Snowflake to evaluate privileges against all roles available to the user, which is less restrictive</span><span> </span></li>
</ul>
<div class="wp-caption aligncenter" id="attachment_34138" style="width: 470px;"><img alt="Configuring the PAT scope, role constraints and expiration policy in the UI—Snowflake Cortex Code CLI " class="wp-image-34138 size-full" height="458" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-21.png" width="460" /><p class="wp-caption-text" id="caption-attachment-34138">Figure 21: Configuring the PAT scope, role constraints and expiration policy in the UI—Snowflake Cortex Code CLI</p></div>
<p><span>Click </span><b><span>Generate</span></b><span>. Snowflake Snowsight will display the generated token exactly once. Copy it immediately; it cannot be retrieved again after you close the dialog.</span><span> </span></p>
<p><span>Return to your terminal. At the Enter your Programmatic Access Token: prompt, paste the token you copied and press </span><b><span>Enter</span></b><span>.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34140" style="width: 2058px;"><img alt="Pasting the generated secret token into the terminal's hidden input field—Snowflake Cortex Code CLI " class="wp-image-34140 size-full" height="545" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-22.png" width="2048" /><p class="wp-caption-text" id="caption-attachment-34140">Figure 22: Pasting the generated secret token into the terminal&#8217;s hidden input field—Snowflake Cortex Code CLI</p></div>
<p><span>The wizard will now prompt for additional optional parameters. These set default values for your Snowflake session. You may press Enter at any prompt to accept the shown default.</span><span> </span></p>
<p><b><span>Default Role</span></b><span> – The Snowflake role the Cortex Code CLI assumes for each session. If left blank, the wizard defaults to ACCOUNTADMIN. For least-privilege configurations, specify a role with only the permissions required for Cortex AI queries (like a role with the </span><span>SNOWFLAKE.CORTEX_USER</span><span> database role granted).</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34141" style="width: 942px;"><img alt="Setting the default active role for the session—Snowflake Cortex Code CLI " class="wp-image-34141 size-full" height="656" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-23.png" width="932" /><p class="wp-caption-text" id="caption-attachment-34141">Figure 23: Setting the default active role for the session—Snowflake Cortex Code CLI</p></div>
<p><b><span>Default Warehouse</span></b><span> – The virtual warehouse used to execute queries. If left blank, default to </span><span>COMPUTE_WH</span><span>. Make sure the specified warehouse is accessible to the configured role and is sized appropriately for your workload.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34142" style="width: 942px;"><img alt="Defining the default virtual warehouse for compute operations—Snowflake Cortex Code CLI " class="wp-image-34142 size-full" height="656" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-24.png" width="932" /><p class="wp-caption-text" id="caption-attachment-34142">Figure 24: Defining the default virtual warehouse for compute operations—Snowflake Cortex Code CLI</p></div>
<p><b><span>Default Database</span></b><span> – The database that serves as the default context for SQL queries. This is optional and can be left blank if you intend to always qualify object names with a fully-qualified </span><span>database.schema.object</span><span> reference.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34143" style="width: 942px;"><img alt="Specifying the default target database context—Snowflake Cortex Code CLI " class="wp-image-34143 size-full" height="656" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-25.png" width="932" /><p class="wp-caption-text" id="caption-attachment-34143">Figure 25: Specifying the default target database context—Snowflake Cortex Code CLI</p></div>
<p><b><span>Default Schema</span></b><span> – The schema used as the default context within the configured database. Also, optional.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34144" style="width: 942px;"><img alt="Specifying the default target schema context—Snowflake Cortex Code CLI " class="wp-image-34144 size-full" height="656" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-26.png" width="932" /><p class="wp-caption-text" id="caption-attachment-34144">Figure 26: Specifying the default target schema context—Snowflake Cortex Code CLI</p></div>
<p><b><span>Connection Name</span></b><span> – A unique, human-readable name for this connection profile. This name is used as the profile key in </span><span>~/.snowflake/connections.toml</span><span> and when selecting connections in future CLI sessions.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34145" style="width: 942px;"><img alt="Naming the connection profile to be saved in connections.toml—Snowflake Cortex Code CLI " class="wp-image-34145 size-full" height="656" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-27.png" width="932" /><p class="wp-caption-text" id="caption-attachment-34145">Figure 27: Naming the connection profile to be saved in connections.toml—Snowflake Cortex Code CLI</p></div>
<p><span>After the final prompt, the wizard validates the connection by authenticating against Snowflake using the supplied credentials. A successful validation confirms that the credentials, account identifier and network path are all correct.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34146" style="width: 942px;"><img alt="Viewing the confirmation summary of the newly created connection profile " class="wp-image-34146 size-full" height="852" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-28.png" width="932" /><p class="wp-caption-text" id="caption-attachment-34146">Figure 28: Viewing the confirmation summary of the newly created connection profile</p></div>
<p><span>If this is your first time using Snowflake Cortex Code, you will be prompted to activate a trial subscription before the Cortex Code CLI becomes fully operational.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34147" style="width: 928px;"><img alt="Terminal displaying a Cortex Code trial activation prompt—Snowflake Cortex Code CLI " class="wp-image-34147 size-full" height="946" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-29.png" width="918" /><p class="wp-caption-text" id="caption-attachment-34147">Figure 29: Terminal displaying a Cortex Code trial activation prompt—Snowflake Cortex Code CLI</p></div>
<blockquote><p><b><span>💡 Subscription details</span></b><span> – Snowflake Cortex Code is a paid subscription feature priced at $20 USD per user per month. The trial period begins upon activation. Billing starts the day after the trial period ends. A valid credit card must be on file to activate the trial, but it will not be charged during the trial. You may cancel at any time during or after the trial period without incurring charges beyond the current billing cycle.</span><span> </span></p></blockquote>
<p><span>Follow these steps to activate the trial:</span><span> </span></p>
<p><span>Check the inbox of the email address associated with your Snowflake account for an activation email from Snowflake (sender: </span><span>no-reply@snowflake.com</span><span> or similar).</span><span> </span></p>
<p><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34149" style="width: 1610px;"><img alt="Signing up for Snowflake Cortex Code CLI " class="wp-image-34149 size-full" height="833" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-30.png" width="1600" /><p class="wp-caption-text" id="caption-attachment-34149">Figure 30: Signing up for Snowflake Cortex Code CLI</p></div>
<div class="wp-caption aligncenter" id="attachment_34148" style="width: 634px;"><img alt="Clicking the activation link to confirm the seat license—Snowflake Cortex Code CLI " class="wp-image-34148 size-full" height="400" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-31.png" width="624" /><p class="wp-caption-text" id="caption-attachment-34148">Figure 31: Clicking the activation link to confirm the seat license—Snowflake Cortex Code CLI</p></div>
<p><span>Click the activation link in the email. Your browser will open a Snowflake account setup or subscription activation page. Follow the on-screen prompts to complete account verification.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34150" style="width: 484px;"><img alt="Signing up for Snowflake Cortex Code CLI " class="wp-image-34150 size-full" height="681" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-32.png" width="474" /><p class="wp-caption-text" id="caption-attachment-34150">Figure 32:  Signing up for Snowflake Cortex Code CLI</p></div>
<p><span>After that, enter valid credit card information. This is required to activate the trial and guarantees continuity of service after the trial period, but no charge is applied until the trial ends.</span><span> </span></p>
<div class="wp-caption aligncenter" id="attachment_34151" style="width: 790px;"><img alt="Entering credit card details to activate Snowflake Cortex Code CLI trial " class="wp-image-34151 size-full" height="557" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-33.png" width="780" /><p class="wp-caption-text" id="caption-attachment-34151">Figure 33: Entering credit card details to activate Snowflake Cortex Code CLI trial</p></div>
<p><span>After completing the activation flow, return to the terminal. The CLI will be waiting for you to press Enter to synchronize the new subscription status with your configured connection. Press </span><b><span>Enter </span></b><span>to proceed.</span><span> </span></p>
<h3><b><span>Step 4</span></b><span>—Validate connection</span><span> </span></h3>
<p><span>After completing setup, verify that the Cortex Code CLI can successfully communicate with Snowflake. Run the following command to launch the interactive CLI shell:</span><span> </span></p>
<pre class="codebox">cortex</pre>
<div class="wp-caption aligncenter" id="attachment_34152" style="width: 1394px;"><img alt="Running a connection test to ensure network reachability and authentication—Snowflake Cortex Code CLI " class="wp-image-34152 size-full" height="255" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-34.png" width="1384" /><p class="wp-caption-text" id="caption-attachment-34152">Figure 34: Running a connection test to ensure network reachability and authentication—Snowflake Cortex Code CLI</p></div>
<p><span>If the connection is successful, the Cortex Code CLI will display a welcome message and present an interactive prompt (typically &gt; Try “What can you help me with?”). You can also issue a one-off test prompt directly from the shell:</span><span> </span></p>
<pre class="codebox"><span>cortex -p "&lt;your_prompt_message&gt;"</span></pre>
<p><span>The CLI should execute the query against your Snowflake account and return the result inline. This confirms end-to-end connectivity: authentication, network routing, warehouse activation and query execution.</span><span> </span></p>
<blockquote><p><b><span>⚠️ Troubleshooting tips</span></b><span> – If the command hangs or returns an authentication error, verify the following: </span><span> </span></p>
<ul>
<li><span>Your account identifier is correct and does not include the .snowflakecomputing.com suffix (use the identifier format, not the full URL, unless the full URL was explicitly accepted during setup)</span><span> </span></li>
</ul>
<ul>
<li><span> Your PAT has not expired</span><span> </span></li>
</ul>
<ul>
<li><span>The role specified in your connection profile has the SNOWFLAKE.CORTEX_USER database role granted</span><span> </span></li>
<li><span>And no firewall or proxy is blocking outbound HTTPS traffic to Snowflake</span><span> </span></li>
</ul>
</blockquote>
<h3><b><span>Step 5</span></b><span>—Select or change your model</span><span> </span></h3>
<p><span>Snowflake Cortex Code CLI delegates inference to Snowflake Cortex AI, which provides access to a curated set of large language models (including </span><a href="https://platform.claude.com/docs/en/about-claude/models/overview"><span>Anthropic&#8217;s Claude model family</span></a><span>). Cortex Code CLI operates in auto mode by default, which instructs Snowflake to select the highest-capability model that is available in your account&#8217;s home region (or across regions, if cross-region inference is enabled).</span><span> </span></p>
<p><span>To explicitly specify a model, use the -m flag at startup:</span><span> </span></p>
<pre class="codebox"><span>cortex -m claude-sonnet-4-5</span></pre>
<p><span>To switch models within an active interactive session, use the /model command:</span><span> </span></p>
<pre class="codebox">/model claude-opus-4-5</pre>
<p><span>The recommended high-quality model is </span><b><span>Claude Sonnet 4.5</span></b><span>, or </span><b><span>Opus 4.5</span></b><span> if available. </span><span> </span></p>
<blockquote><p><span>🔮 Model Availability – Model availability in Snowflake Cortex is region-dependent. If you specify a model that is not hosted in your account&#8217;s home region and cross-region inference is disabled, the Snowflake Cortex Code CLI will return a model availability error. Enabling cross-region inference (setting it to ANY_REGION in the Cortex AI settings) allows requests to be routed to the region that hosts the requested model, resolving most availability issues.</span><span> </span></p></blockquote>
<h3><b><span>Step 6</span></b><span>—(Optional) Enable web search (ACCOUNTADMIN required)</span><span> </span></h3>
<p><span>Snowflake Cortex Code can augment its responses with real-time web search results when answering questions that require up-to-date external information. This feature must be enabled at the account level by an ACCOUNTADMIN in Snowflake Snowsight before the CLI can use it.</span><span> </span></p>
<p><span>To enable web search:</span><span> </span></p>
<ul>
<li><span>Log in to Snowflake Snowsight and navigate to</span><b><span> Admin &gt; AI &amp; ML &gt; Agents &gt; Settings</span></b><span>.</span><span> </span></li>
</ul>
<ul>
<li><span>Locate the </span><b><span>Web Search</span></b><span> toggle and enable it.</span><span> </span></li>
</ul>
<div class="wp-caption aligncenter" id="attachment_34125" style="width: 878px;"><img alt="Toggling the Web Search capability in the Snowflake Cortex administration panel—Snowflake Cortex Code CLI " class="wp-image-34125 size-full" height="163" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-cortex-code-8.png" width="868" /><p class="wp-caption-text" id="caption-attachment-34125">Figure 35: Toggling the Web Search capability in the Snowflake Cortex administration panel—Snowflake Cortex Code CLI</p></div>
<p><span>Once enabled at the account level, Snowflake Cortex Code CLI will automatically detect and use this setting in subsequent sessions. You do not need any additional CLI-level configuration.</span><span> </span></p>
<h3><b><span>Step 7</span></b><span>—Submit your first CLI prompt</span><span> </span></h3>
<p><span>Now try asking Snowflake Cortex Code CLI a question or task. Start the interactive shell by running </span><span>Cortex</span><span> with no arguments or use a one-off prompt.</span><span> </span></p>
<p><span>At the &gt; prompt, type a natural-language instruction or question.</span><span> </span></p>
<p><span>Snowflake Cortex Code will interpret your instruction, generate the appropriate SQL, execute it against your Snowflake account and return both the generated query and the result set inline in the terminal. You can also run a single non-interactive prompt without entering the shell:</span><span> </span></p>
<pre class="codebox"><span>cortex -p "&lt;your_prompt_message&gt;"</span><span> </span></pre>
<p><span>The </span><span>-p</span><span> flag (short for </span><span>&#8211;prompt</span><span>) accepts a quoted natural-language string, sends it to the Cortex AI backend and prints the response before exiting.</span><span> </span></p>
<h3><b><span>Step 8</span></b><span>—Reference tables directly with # syntax</span><span> </span></h3>
<p><span>A powerful feature of the Snowflake Cortex Code CLI is its ability to reference Snowflake database objects, such as tables, views and stages, directly in your prompts. To do this, you use the # prefix. When Snowflake Cortex Code CLI sees a token starting with #, it treats the rest as a reference to a Snowflake object. It then resolves this reference against the Information Schema, gets the object&#8217;s DDL definition and some sample data and injects this context to the prompt it sends to the AI model.</span><span> </span></p>
<pre class="codebox"><span>&gt; Describe the schema of #PRODUCTS_DB.PUBLIC.ORDERS and suggest composite indexes for typical analytical queries.</span><span> </span></pre>
<p><span>This guarantees the model’s response is grounded in the actual column names, data types and row-level patterns of your table, rather than generating generic SQL that may not match your schema. The syntax supports any fully-qualified three-part object name:</span><span> </span></p>
<pre class="codebox"><span>&gt; Join #ANALYTICS_DB.MARTS.FCT_SALES with #ANALYTICS_DB.MARTS.DIM_CUSTOMER and generate a monthly revenue cohort analysis</span><span> </span></pre>
<p><span>You can reference multiple objects in a single prompt. Snowflake Cortex Code CLI will resolve each # reference independently and aggregate their schemas into a combined context block before sending the request to the model.</span><span> </span></p>
<h3><b><span>Step 9</span></b><span>—Invoke custom Agent Skills with $ syntax</span><span> </span></h3>
<p><span>Snowflake Cortex Code CLI supports a concept called</span><b><span> Agent Skills. </span></b><span>It is a predefined prompt template that encapsulates domain-specific instructions, constraints and context for common development tasks. Agent Skills are invoked using the </span><span>$</span><span> prefix followed by the skill name.</span><span> </span></p>
<p><span>For example, to invoke the built-in dbt engineering skill:</span><span> </span></p>
<pre class="codebox"><span>&gt; $dbt-engineering Create a new mart model for customer lifetime value</span><span> </span></pre>
<p><span>Or </span><span>$developing-with-streamlit</span><span> to get Streamlit app guidance. You can type </span><span>$</span><span> alone and press Tab or Up/Down to browse available skills.</span><span> </span></p>
<p><span>To browse available skills, type $ followed by Tab, or use the following introspection command:</span><span> </span></p>
<pre class="codebox">/skill</pre>
<p><span>Custom skills are defined as Markdown files (</span><span>.md</span><span>) placed in a designated </span><span>.cortex/skills/</span><span> directory within your project. A skill file uses structured sections (delimited by Markdown headings) to define the skill&#8217;s name, description, behavioral constraints and example invocations. Once defined, the custom skill is immediately available via the </span><span>$</span><span> prefix from any CLI session launched within that project directory.</span><span> </span></p>
<h3><b><span>Step 10</span></b><span>—Configure an </span><span>AGENTS.md</span><span> file for project-level customization</span><span> </span></h3>
<p><span>For project-specific customization, Snowflake Cortex Code CLI recognizes an </span><span>AGENTS.md</span><span> file placed at the root of your project directory. This file is an open-format Markdown document that provides persistent, session-wide instructions to the Cortex AI agent. Every time you launch the Cortex Code CLI from within that project directory, the contents within the AGENTS.md are automatically injected into the system context of each conversation, giving the model project-specific knowledge about your data architecture, naming conventions, coding standards and operational constraints.</span><span> </span></p>
<p><span>Create the file at the root of your project:</span><span> </span></p>
<pre class="codebox"><span># Project Context</span> 

<span>## Database</span><span> </span>

<span>Target database: PRODUCTS_DB, schema: PUBLIC</span><span> </span>

<span> </span>

<span>## Naming conventions</span><span> </span>

<span>- Fact tables: fct_*</span><span> </span>

<span>- Dimension tables: dim_*</span><span> </span>

<span>- Staging tables: stg_*</span><span> </span>

<span> </span>

<span>## Coding Standards</span><span> </span>

<span>- Always qualify object references with database.schema.object</span><span> </span>

<span>- Use parameterised date filters; never hardcode date literals</span><span> </span>

<span>- Add a header comment block to all new SQL files with:</span><span> </span>

<span>    -- Author, creation date, description, and downstream dependencies</span><span> </span>

<span>- Prefer CTEs over nested subqueries for readability</span><span> </span>

<span> </span>

<span>## Constraints</span><span> </span>

<span>- Do NOT modify tables in RAW_DB directly; use staging layers only</span><span> </span>

<span>- All new models must include a dbt YAML schema file with column tests</span><span> </span>

<span>- Incremental models must use the unique_key strategy with a surrogate key</span><span> </span></pre>
<p><span>The </span><span>AGENTS.md</span><span> file is particularly valuable in team settings: when all developers on a project use Snowflake Cortex Code CLI from the same repository root, they all benefit from the same shared project context.</span><span> </span></p>
<h3><b><span>Step 11</span></b><span>—(Optional) Disable automatic updates</span><span> </span></h3>
<p><span>Snowflake Cortex Code CLI updates automatically by default. In production environments or locked-down machines, you may want to disable this. Edit (or create) the file </span><span>~/.snowflake/cortex/settings.json</span><span> and set &#8220;autoUpdate&#8221;: false.</span><span> </span></p>
<pre class="codebox"><span>{</span><span> </span>

<span>  "compactMode": false,</span><span> </span>

<span>  "autoUpdate": false,</span><span> </span>

<span>  "theme": "dark"</span><span> </span>

<span>...</span><span> </span>

<span>}</span><span> </span></pre>
<p><span>Setting  </span><span>&#8220;autoUpdate&#8221;: false</span><span>, Snowflake Cortex Code CLI will stop checking for updates. Save the file, and you’ll skip the automatic update prompts. You can still run the Cortex update manually whenever you want to upgrade.</span></p>
<blockquote><p><span>📁 File Location &#8211;  The settings file path is ~/.snowflake/cortex/settings.json. If this file does not exist, you can create it manually. The directory ~/.snowflake/cortex/ is created automatically when the CLI is first run.</span><span> </span></p></blockquote>
<p><span>Now, with these steps done, Snowflake Cortex Code CLI is ready. You can now interact with Snowflake using natural language right in your terminal. The CLI will execute SQL on your behalf, show query results and even interact with your local files (Git, Bash commands) as needed.</span><span> </span></p>
<p><span>For further details, see the </span><b><span>Snowflake Cortex Code documentation, Cortex Code CLI documentation </span></b><span>and the </span><b><span>Snowflake CLI integration guide </span></b><span>for advanced configuration options.</span><span> </span></p>
<h2><span>Limitations of Snowflake Cortex Code—what you need to know</span><span> </span></h2>
<p><span>Snowflake Cortex Code is extremely powerful, but it has some limitations to consider:</span><span> </span></p>
<h3><span>1) Regional availability</span><span> </span></h3>
<p><span>Snowflake Cortex Code is only available in Commercial Snowflake accounts with cross-region inference enabled. It is not supported in the Government cloud, the Sovereign cloud, or certain isolated deployments.</span><span> </span></p>
<h3><span>2) Legacy Snowflake Copilot opt-out blocks Snowflake Cortex Code</span><span> </span></h3>
<p><span>If anyone on your account team historically disabled Snowflake Copilot, Cortex Code will be disabled too. You cannot enable it yourself from the Snowflake Snowsight UI; you need to contact your Snowflake account team to resolve it.</span><span> </span></p>
<h3><span>3) No persistent memory by default </span><span> </span></h3>
<p><span>Context is maintained within a session, but not across sessions. Each new session starts fresh. If you have project-level context, you want Snowflake Cortex Code to always know, put it in an AGENTS.md file. </span><span> </span></p>
<h3><span>4) Role-based access limits apply—always</span><span> </span></h3>
<p><span>The agent can only access data that your current role permits. It will not bypass any security policies. If a table is hidden or has masking, the agent will not reveal it.</span><span> </span></p>
<h3><span>5) Diff view and context limited to Snowflake Snowsight</span><span> </span></h3>
<p><span>The visual Diff View, workspace context awareness and some admin-facing features only exist in the Snowflake Snowsight interface. The CLI is more powerful for local development but doesn&#8217;t replicate everything from the UI.</span><span> </span></p>
<h3><span>6) Web search is not enabled by default</span><span> </span></h3>
<p><span>The web search feature must be explicitly enabled with ACCOUNTADMIN access. </span><span> </span></p>
<h3><span>7) MCP server configuration complexity</span><span> </span></h3>
<p><span>MCP integration is powerful but not trivial to set up. You will need to configure MCP server connections manually, understand the tool schema and handle authentication for each external service. It&#8217;s not a one-click integration.</span><span> </span></p>
<h3><span>8) Windows native support doesn&#8217;t exist</span><span> </span></h3>
<p><span>macOS and Linux are supported natively. Windows users have to go through the WSL2 setup, which might add setup complexity.</span><span> </span></p>
<p><span>Be aware of these limitations. Snowflake Cortex Code still handles most common data-engineering and analytics tasks reliably.</span><span> </span></p>
<h2><span>Snowflake Cortex Code best practices—tips for effective use</span><span> </span></h2>
<p><span>Here are some tips and tricks to help you achieve excellent results and maintain safety while working with Snowflake Cortex Code:</span><span> </span></p>
<h3><span>1) Be specific and detailed in your prompts </span><span> </span></h3>
<p><span>Vague prompts produce vague results. Be very thorough and detailed. “Write a revenue query” is less effective than “Write a query against ANALYTICS_DB.SALES_MART.ORDERS that calculates weekly revenue by region, filtered to 2025, using the REGION column and ORDER_DATE field”. </span><span> </span></p>
<p><span>Give the agent context it can work with.</span><span> </span></p>
<h3><span>2) Use fully qualified object names in prompts</span><span> </span></h3>
<p><span>Reference tables as DATABASE.SCHEMA.TABLE_NAME whenever possible, rather than just the table name. On large accounts with many databases, ambiguity causes the agent to pick the wrong object.</span><span> </span></p>
<h3><span>3) Use # table references in the CLI</span><span> </span></h3>
<p><span>In the CLI, use # before a table name to auto-load its schema and sample rows. This guarantees the model knows your table’s structure without you describing it.</span><span> </span></p>
<h3><span>4) Always use Diff View before accepting changes (Snowflake Snowsight)</span><span> </span></h3>
<p><span>Do not skip the review step. Always check for suggested code changes in the Diff View (Snowsight) or plan output (CLI). Do not blindly accept large edits without inspection.</span><span> </span></p>
<h3><span>5) Create AGENTS.md for project-specific workflows</span><span> </span></h3>
<p><span>Put any recurring guidelines, style rules, or context in an AGENTS.md file so the agent always sees them.</span><span> </span></p>
<h3><span>6) Build Agent Skills for repeated workflows</span><span> </span></h3>
<p><span>Anything you regularly explain in sessions, such as data quality checks, audit column requirements, or tagging validation, should be in a custom Agent Skill. This will save you from having to re-explain the context every time.</span><span> </span></p>
<h3><span>7) Use Plan Mode for complex or write operations</span><span> </span></h3>
<p><span>Any operation that touches the schema, modifies data, or has consequences you can&#8217;t easily undo should go through plan mode. Confirm each step explicitly rather than letting the agent run to completion unattended.</span><span> </span></p>
<h3><span>8) Secure your connections.toml </span><span> </span></h3>
<p><span>The ~/.snowflake/connections.toml file contains your Snowflake connection details. Set appropriate file permissions (chmod 600 ~/.snowflake/connections.toml).</span><span> </span></p>
<p><span>Don&#8217;t commit it to version control.</span><span> </span></p>
<h3><span>9) Use programmatic access tokens (PATs) for CI/CD</span><span> </span></h3>
<p><span>If you&#8217;re integrating Snowflake Cortex Code CLI into automated pipelines or GitHub Actions, use PATs rather than passwords or SSO-based auth. PATs are scoped, revocable and designed for machine-to-machine use.</span><span> </span></p>
<h3><span>10) Pair Snowflake Cortex Code CLI with VS Code or Cursor</span><span> </span></h3>
<p><span>Open VS Code, start your terminal inside it and run Cortex from there. Files Cortex Code generates land in your Snowflake workspace where you can immediately review, edit and commit them. You&#8217;re not jumping between a terminal and an IDE.</span><span> </span></p>
<h3><span>11) Start with read-only tasks before write operations. </span><span> </span></h3>
<p><span>Especially at first, ask the agent to generate queries or show plans before running any data-modifying commands. Once you trust it, you can move on to DDL/DML changes.</span><span> </span></p>
<h3><span>12) Monitor model selection and switch when needed</span><span> </span></h3>
<p><span>The default model (auto) should pick a good large language models (LLMs), but sometimes a specific model is better for your region. If answers seem off, try switching models with /model (CLI) or the Snowsight model selector.</span><span> </span></p>
<h3><span>13) Enable cross-region inference—always </span><span> </span></h3>
<p><span>To get the best models (Claude Sonnet/Opus), make sure CORTEX_ENABLED_CROSS_REGION is set. Without it, you’ll be limited to older models in your region.</span><span> </span></p>
<p><span>Follow these practices to make the most of Snowflake Cortex Code’s features and reduce risks.</span><span> </span></p>
<h2><span>Conclusion</span><span> </span></h2>
<p><span>And that is a wrap! Snowflake Cortex Code is a substantive leap forward from generic AI coding assistants, specifically because it does not pretend your data platform doesn&#8217;t exist. It knows your catalog, honors your governance controls and understands your semantic models. Seamlessly, it operates within the security perimeter your data team&#8217;s trust. Yet the Snowflake Cortex Code isn’t a magic wand that solves everything automatically. You will still need to peruse its outputs and direct it with thoughtful prompts. However, it can slice through tedious development tasks, helping you prototype in a flash. In mere seconds, install the CLI or toggle the Snowflake Snowsight switch to unveil fresh suggestions. Try it on a small task to see how it integrates into your workflow. Always double-check the SQL and Python it generates, especially for critical data. Utilize the Diff View and plan modes to maintain control. With care, Snowflake Cortex Code could be your new right-hand companion in the Snowflake toolkit.</span><span> </span></p>
<p><span>In this article, we have covered:</span><span> </span></p>
<ul>
<li><span>What is Snowflake Cortex Code?</span><span> </span></li>
</ul>
<ul>
<li><span>Architecture &amp; How Snowflake Cortex Code accesses context</span><span> </span></li>
</ul>
<ul>
<li><span>Four core pillars of Snowflake Cortex Code</span><span> </span></li>
</ul>
<ul>
<li><span>Power of Snowflake Cortex Code—Features and use cases</span><span> </span></li>
</ul>
<ul>
<li><span>Pricing and cost breakdown of Snowflake Cortex Code</span><span> </span></li>
</ul>
<ul>
<li><span>Snowflake Intelligence vs Snowflake Cortex Code</span><span> </span></li>
</ul>
<ul>
<li><span>Snowflake Cortex Code vs Snowflake Copilot (legacy Copilot)</span><span> </span></li>
</ul>
<ul>
<li><span>Step-by-step guide to set up and configure Snowflake Cortex Code (via Snowsight)</span><span> </span></li>
</ul>
<ul>
<li><span>Step-by-step guide to set up and configure Snowflake Cortex Code (via Cortex Code CLI)</span><span> </span></li>
</ul>
<ul>
<li><span>Limitations of Snowflake Cortex Code</span><span> </span></li>
</ul>
<ul>
<li><span>Snowflake Cortex Code best practices</span><span> </span></li>
</ul>
<p><span>… and so much more!</span><span> </span></p>
<p>&nbsp;</p>
<p><a class="btn" href="https://www.flexera.com/about-us/contact-us-b"><span>Want to learn more? Reach out for a chat</span></a></p>
<p>&nbsp;</p>
<h2><span>FAQs</span><span> </span></h2>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>What is Snowflake Cortex Code?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake Cortex Code is an AI coding assistant built into Snowflake. You can use natural language to generate or modify SQL/Python, build pipelines and explore data, all within the context of your Snowflake account. It comes as both a Snowsight panel and a local CLI tool.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3><span class="TextRun SCXW138511155 BCX8" lang="EN" xml:lang="EN"><span class="NormalTextRun SCXW138511155 BCX8">Is Snowflake Cortex Code free to use?</span></span><span class="EOP SCXW138511155 BCX8"> </span></h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p><span class="TextRun SCXW196177905 BCX8" lang="EN-US" xml:lang="EN-US"><span class="NormalTextRun ContextualSpellingAndGrammarErrorV2Themed SCXW196177905 BCX8">Snowflake</span><span class="NormalTextRun SCXW196177905 BCX8"> </span><span class="NormalTextRun SpellingErrorV2Themed SCXW196177905 BCX8">Snowsight</span><span class="NormalTextRun SCXW196177905 BCX8"> (UI) version is currently free for eligible accounts. The CLI will eventually incur charges based on token usage (like other AI features). Your regular Snowflake compute/storage costs still apply when the agent runs queries.</span></span><span class="EOP SCXW196177905 BCX8"> </span></p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can Snowflake Cortex Code see and use my data?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Yes, but only under your permissions. Cortex Code runs inside Snowflake’s secure infrastructure, so your data and schemas stay inside your account. It can access tables, schemas and metadata that your role can see, but it will not share data externally or bypass any security rules.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>How does Snowflake Cortex Code respect masking and row access policies?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake Cortex Code fully inherits Snowflake’s security model. If your role has a column masked or row restrictions, the agent will not expose that data. In practice, Snowflake Cortex Code only operates with the privileges of the current user.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>Where are conversation logs stored for the CLI?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>The CLI stores your session transcripts on your local machine (typically under the Snowflake config directory). You can resume previous sessions by ID. These logs are not sent to Snowflake; they reside on your local machine for privacy.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>What is the difference between Snowflake Cortex Code and Snowflake Copilot?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake Copilot was a simpler chat-based assistant that suggested SQL in Snowsight. It&#8217;s deprecated. Cortex Code replaces it with a fully agentic system that can execute multi-step workflows, access your local file system (via CLI), build dbt projects, generate Streamlit apps and integrate with external tools via MCP, all governed by Snowflake&#8217;s RBAC.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>What models does Snowflake Cortex Code use?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake Cortex Code uses automatic model selection by default, drawing from the models available on Snowflake&#8217;s Cortex AI infrastructure. You can view and change the active model using /model in the CLI. Specific model availability depends on your account&#8217;s region and the models Snowflake currently provides.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can I use Snowflake Cortex Code CLI with VS Code or Cursor?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Yes. Open VS Code or Cursor, launch the integrated terminal and run Cortex from there. Files generated by Snowflake Cortex Code will appear in your Snowflake workspace for immediate review. This is the recommended workflow for active development.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>What is an AGENTS.md file, and do I need one?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>AGENTS.md is a special markdown file you can put in your project root. Any instructions or context you write in that file will be automatically included in every Snowflake Cortex Code conversation for that project. It’s optional but useful for codifying project-specific rules or objectives.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>Is Snowflake Cortex Code available in Snowflake Government or Sovereign cloud deployments?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Not yet. Snowflake Cortex Code (Snowsight and CLI) currently requires a Commercial Snowflake account with cross-region inference. It is not supported in FedRAMP/Government, International Sovereign, or other isolated cloud instances now.</p>
</div>
</div>
</div>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can I use Snowflake Cortex Code CLI in automated pipelines or GitHub Actions?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Yes, using programmatic access tokens (PATs) for authentication. Configure your connections.toml with a PAT credential, reference that connection in your pipeline and invoke Cortex non-interactively. For CI/CD workflows, disable auto-updates (&#8220;autoUpdate&#8221;: false) to prevent unexpected version changes in production.</p>
</div>
</div>
</div>
