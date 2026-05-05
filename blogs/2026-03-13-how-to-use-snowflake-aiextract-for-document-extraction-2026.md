---
title: "How to: Use Snowflake AI_EXTRACT for document extraction (2026)"
url: "https://www.flexera.com/blog/finops/snowflake-ai-extract/"
date: "Fri, 13 Mar 2026 20:06:46 +0000"
author: "Pramit Marattha"
feed_url: "https://www.flexera.com/blog/feed/"
---
<div><img alt="" class="attachment-card-hero size-card-hero wp-post-image" height="478" src="https://www.flexera.com/blog/wp-content/uploads/2025/09/blog-header-08-915x478.jpg" style="margin-bottom: 10px;" width="915" /></div><div id="toc" title="Table of Contents"></div>
<p>Manual document processing has long been a major headache for data teams. It&#8217;s costly, slow and kills productivity. You are probably sitting on thousands of PDFs, invoices, documents and image-heavy forms. The trouble is someone still has to dig manually through all that to get the usable data out. But manual extraction just doesn&#8217;t scale. Regex-based parsing falls apart the moment a document format changes. And piping files through external tools/APIs means moving sensitive data outside your security perimeter.</p>
<p>If you are a <a href="https://www.flexera.com/blog/finops/snowflake-vs-databricks/#what-is-snowflake" rel="noopener" target="_blank">Snowflake</a> user, you might have used <a href="https://www.flexera.com/blog/finops/snowflake-document-ai/" rel="noopener" target="_blank">Snowflake Document AI</a> to handle this very problem. It’s a GUI-driven feature of Snowflake Document AI that lets you define what data to extract, train models on sample docs and make predictions. But it had its issues. You needed to build and train a model just to get started; it only supported seven languages and required a separate workflow. To top it off, it was pretty limited. But <a href="https://docs.snowflake.com/en/release-notes/bcr-bundles/un-bundled/bcr-2156" rel="noopener" target="_blank">Snowflake Document AI is on its way out</a>. Snowflake is deprecating it and will eventually shut it down.</p>
<p>The good news is that there is a new replacement: the <a href="https://www.flexera.com/blog/finops/snowflake-cortex/#2-extractanswer" rel="noopener" target="_blank">Snowflake AI_EXTRACT function</a>, which is a native SQL function inside <a href="https://www.snowflake.com/en/product/features/cortex/" rel="noopener" target="_blank">Snowflake Cortex AI</a>. This function is just one of many in the Snowflake Cortex AI suite. It can extract structured data from text, documents and images right inside Snowflake. Single function call, no external infrastructure, no data movement and no model training required.</p>
<p>In this article, we’ll cover everything you need to know about what Snowflake AI_EXTRACT does, how it compares to Snowflake Document AI and AI_PARSE_DOCUMENT, how to set it up end to end, how to control and monitor costs, its real limitations and the best practices for using it in production. Let’s dive right in!</p>
<h2>What is Snowflake Cortex AI, anyway?</h2>
<p>Let&#8217;s start with the basics of <a href="https://www.snowflake.com/en/product/features/cortex/" rel="noopener" target="_blank">Snowflake Cortex AI</a> before we dive into Snowflake AI_EXTRACT. Snowflake Cortex AI is a fully managed service within the Snowflake platform. It offers advanced artificial intelligence and machine learning features, all driven by large language models (LLMs). It allows users to analyze unstructured data, answer natural language queries and automate tasks. Data remains within Snowflake, so no data movement is required.</p>
<p>Snowflake Cortex AI prioritizes privacy and security by running models within Snowflake&#8217;s secure boundary. Your data never leaves your perimeter and is not used for external training.</p>
<p>Snowflake Cortex AI includes a range of tools and features, such as <a href="https://www.flexera.com/blog/finops/snowflake-intelligence/" rel="noopener" target="_blank">Snowflake Intelligence</a>, <a href="https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents" rel="noopener" target="_blank">Cortex Agents</a>, <a href="https://www.flexera.com/blog/finops/snowflake-cortex/" rel="noopener" target="_blank">Cortex AI functions</a>, <a href="https://www.chaosgenius.io/blog/snowflake-cortex-search/" rel="noopener" target="_blank">Cortex Search</a>, <a href="https://www.chaosgenius.io/blog/snowflake-cortex-analyst/" rel="noopener" target="_blank">Cortex Analysts</a> and <a href="https://www.flexera.com/blog/finops/snowflake-document-ai/" rel="noopener" target="_blank">Snowflake Document AI</a>.</p>
<p>Speaking of which, the Snowflake Cortex AI functions (formerly known as Cortex AISQL) are a core feature of the Cortex AI family.</p>
<div class="wp-caption aligncenter" id="attachment_34068" style="width: 1930px;"><img alt="Snowflake Cortex AI (Source: Snowflake) " class="wp-image-34068 size-full" height="734" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-1.png" width="1920" /><p class="wp-caption-text" id="caption-attachment-34068">Figure 1: Snowflake Cortex AI (Source: Snowflake)</p></div>
<p>Snowflake Cortex AI functions work like regular SQL functions. You can use them to perform AI operations directly in your queries without needing custom code or external APIs. To use them, simply invoke the functions like you would standard SQL functions, passing in inputs like text, images or files and they will return structured outputs.</p>
<p>Snowflake Cortex AI functions give you a complete toolkit. It is made up of core AI functions and some handy helpers. Here&#8217;s a quick breakdown:</p>
<ul>
<li><b>AI_COMPLETE</b><b> </b>— Generates text or analyzes images based on a prompt and input</li>
<li><b>AI_CLASSIFY</b><b> </b>— Assigns categories to text or images</li>
<li><b>AI_FILTER</b><b> </b>— Evaluates text or images against a condition, returning true or false</li>
<li><b>AI_AGG</b><b> </b>— Aggregates text across rows and generates insights per a prompt</li>
<li><b>AI_EMBED</b><b> </b>— Creates vector embeddings from text or images</li>
<li><b>AI_EXTRACT</b><b> </b>— Pulls specific information from text, images, or documents</li>
<li><b>AI_SENTIMENT</b><b> </b>— Detects sentiment in text</li>
<li><b>AI_SUMMARIZE_AGG</b><b> </b>— Summarizes aggregated text across rows</li>
<li><b>AI_SIMILARITY</b><b> </b>— Computes similarity scores between embeddings</li>
<li><b>AI_TRANSCRIBE</b><b> </b>— Transcribes audio or video files</li>
<li><b>AI_PARSE_DOCUMENT</b><b> </b>— Extracts text or layout from documents</li>
<li><b>AI_REDACT</b><b> </b>— Removes personally identifiable information from text</li>
<li><b>AI_TRANSLATE</b><b> </b>— Translates text between languages</li>
<li><b>SUMMARIZE (SNOWFLAKE.CORTEX)</b> — Legacy function that condenses text; limited to 32,000 input tokens and 4,096 output tokens</li>
</ul>
<p>And the helper functions:</p>
<ul>
<li><b>TO_FILE</b><b> </b>— References staged files for use with AI_COMPLETE and other file-aware functions</li>
<li><b>AI_COUNT_TOKENS</b><b> </b>— Estimates token usage to avoid overflows</li>
<li><b>PROMPT</b><b> </b>— Helps you build prompt objects for use with AI_COMPLETE and other functions</li>
<li><b>TRY_COMPLETE (SNOWFLAKE.CORTEX)</b><b> </b>— Runs completions with error tolerance</li>
</ul>
<p>Of these, Snowflake AI_EXTRACT is the tool that extracts specific structured fields from unstructured documents.</p>
<h2>What is Snowflake AI_EXTRACT function?</h2>
<p>Snowflake AI_EXTRACT is a Cortex AI function that pulls structured info from unstructured sources like text, images, or documents. It does this using <a href="https://www.snowflake.com/en/engineering-blog/arctic-extract-vision-language-document-ai/" rel="noopener" target="_blank">Snowflake Arctic-Extract</a>, a vision-language model that is good at reading text and recognizing things like tables, logos, checkboxes and handwritten notes in documents. You do not need to train a custom model, just ask AI_EXTRACT some questions or tell it what you are looking for, and it will give you the answers. It supports both single-value (entity) extraction and list- or table-valued extraction in one shot.</p>
<p>Snowflake AI_EXTRACT is part of Snowflake&#8217;s Cortex AI family. It is an upgrade from the older <a href="https://docs.snowflake.com/en/sql-reference/functions/extract_answer-snowflake-cortex" rel="noopener" target="_blank">Snowflake EXTRACT_ANSWER function</a> and the Snowflake Document AI model. One substantial difference: Snowflake AI_EXTRACT does not need a pre-built model to work. You can use it for one-off zero-shot extractions (no training) or point it at a fine-tuned model in the Snowflake Model Registry to get better results. Now, if you just need to extract text or do optical character recognition (OCR), Snowflake has <a href="https://docs.snowflake.com/en/sql-reference/functions/ai_parse_document" rel="noopener" target="_blank">AI_PARSE_DOCUMENT</a> for that. But Snowflake AI_EXTRACT is designed for when you want specific fields or tables pulled out using LLM-powered Q&amp;A.</p>
<h3>Key features and capabilities of Snowflake AI_EXTRACT function</h3>
<p>Snowflake AI_EXTRACT can process documents of various formats in 29 different languages and extract information from both text-heavy paragraphs and content in a graphical format. Here is the full feature breakdown of Snowflake AI_EXTRACT function:</p>
<ul>
<li><b>Multimodal document processing </b>— Snowflake AI_EXTRACT uses a vision-based LLM, not just an OCR text parser. It can process visually structured documents like tables, diagrams, checkboxes, logos and handwritten signatures; not just machine-typed text.</li>
<li><b>Vision-language model </b>— Snowflake AI_EXTRACT is powered by Snowflake Arctic-Extract which sees the document layout and image content, so it’s not limited to plain text. It can read handwritten text, marked checkboxes and even printed logos or stamps.</li>
<li><b>Entity, list and table extraction in a single call </b>— One function call can return a string value, an array of values and a tabular structure simultaneously. You do not need to chain multiple API calls together.</li>
<li><b>29 supported languages </b>— Snowflake AI_EXTRACT supports Arabic, Bengali, Burmese, Cebuano, Chinese, Czech, Dutch, English, French, German, Hebrew, Hindi, Indonesian, Italian, Japanese, Khmer, Korean, Lao, Malay, Persian, Polish, Portuguese, Russian, Spanish, Tagalog, Thai, Turkish, Urdu and Vietnamese.</li>
<li><b>Flexible response formats </b>— Snowflake AI_EXTRACT allows you to define what to extract using a simple key-value object, an array of questions or a full JSON schema with typed properties and column ordering for tables.</li>
<li><b>Batch from stages </b>— Snowflake AI_EXTRACT can access the files stored in internal or external stages. A “directory table” on Snowflake Stage lets you batch-process many files at once.</li>
<li><b>Zero-shot extraction </b>— No training data required. The model generalizes across document types out of the box. If you previously fine-tuned a Snowflake Document AI model, note that AI_EXTRACT doesn&#8217;t support fine-tuning; it&#8217;s entirely zero-shot.</li>
<li><b>Runs inside Snowflake&#8217;s security boundary </b>— Snowflake AI_EXTRACT respects role-based access control (RBAC), data governance policies and all existing Snowflake access controls. No data movement, no external calls to third-party services.</li>
<li><b>Highly scalable </b>— Snowflake AI_EXTRACT automatically scales to process many documents in parallel, leveraging Snowflake’s compute.</li>
</ul>
<blockquote><p><b>TL; DR</b>: Snowflake AI_EXTRACT is Snowflake&#8217;s modern approach to extracting data from documents. You don&#8217;t need to create separate models; just write a prompt and let the LLM parse the page.</p></blockquote>
<h2>Syntax and argument breakdown of Snowflake AI_EXTRACT function</h2>
<p>Snowflake AI_EXTRACT takes two parameters: the input (either <i>text </i>or <i>file</i>) and responseFormat. These are the only two arguments, but responseFormat is where things get complicated.</p>
<p><b>Text input</b></p>
<pre class="codebox"><code>SELECT AI_EXTRACT(
   text  =&gt; '&lt;your_text_string&gt;',
   responseFormat =&gt; &lt;format&gt;
);</code></pre>
<p>Use this when you need to extract data from raw strings that are stored in a column.</p>
<p><b>File input</b></p>
<pre class="codebox">SELECT AI_EXTRACT( 

    file  =&gt; TO_FILE('&lt;stage&gt;', '&lt;path&gt;'), 

    responseFormat =&gt; &lt;format&gt; 

);</pre>
<p>Use this option for documents that are stored on either internal or external Snowflake Stages.</p>
<blockquote><p><b>Note</b>: You cannot use both a text string and a file object in the same call. Choose one.</p></blockquote>
<p><b>responseFormat</b></p>
<p>And the response format can be:</p>
<ul>
<li>An array of strings containing the information to be extracted</li>
<li>An array of arrays containing two strings (label and the information to be extracted)</li>
<li>A key-value map where the key is a name and the value is a question string</li>
<li>A JSON schema object (for complex extractions). You define properties with descriptions and types (string, array or table object)</li>
</ul>
<p><b>Format 1</b>—Simple key-value object</p>
<pre class="codebox">responseFormat =&gt; {'name': 'What is the employee last name?', 'city': 'What city does the employee live in?'}</pre>
<p><b>Format 2</b>—Array of labeled pairs</p>
<pre class="codebox">responseFormat =&gt; [['name', 'What is the first name?'], ['city', 'Where does the employee live?']]</pre>
<p><b>Format 3</b>—Array of questions (no labels, labels auto-generated)</p>
<pre class="codebox">responseFormat =&gt; ['What is the invoice total?', 'What is the name of the vendor?']</pre>
<p><b>Format 4</b>—JSON schema (supports entity strings, arrays and tables)</p>
<pre class="codebox">responseFormat =&gt; { 

  'schema': { 

    'type': 'object', 

    'properties': { 

      'vendor_name': { 

        'description': 'What is the vendor name on this invoice?', 

        'type': 'string' 

      }, 

      'line_items': { 

        'description': 'What are the product descriptions on this invoice?', 

        'type': 'array' 

      }, 

      'charges_table': { 

        'description': 'The detailed list of charges', 

        'type': 'object', 

        'column_ordering': ['item', 'quantity', 'unit_price', 'total'], 

        'properties': { 

          'item': { 'type': 'array' }, 

          'quantity': { 'type': 'array' }, 

          'unit_price': { 'type': 'array' }, 

          'total': { 'type': 'array' } 

        } 

      } 

    } 

  } 

}</pre>
<blockquote><p><b>Note</b>: You cannot mix JSON schema format with the other formats in the same call. If responseFormat contains the schema key, you must define all questions within the JSON schema. Also, string is currently the only supported scalar type in JSON schemas (numeric and boolean types aren&#8217;t supported yet).</p></blockquote>
<h2>Snowflake AI_EXTRACT vs Snowflake Document AI</h2>
<p>So, what is the difference between <a href="https://www.flexera.com/blog/finops/snowflake-document-ai/" rel="noopener" target="_blank">Snowflake Document AI</a> and Snowflake AI_EXTRACT?</p>
<p>Snowflake Document AI was Snowflake&#8217;s earlier approach to document extraction. Document AI and the &lt;model_build_name&gt;!PREDICT method are now deprecated. Snowflake recommends using the AI_EXTRACT function instead. If you are still using Snowflake Document AI pipelines, now is the time to migrate immediately.</p>
<pre><b><i>
Table 1</i></b><i>: Difference between Snowflake AI_EXTRACT vs Snowflake Document AI</i></pre>
<table>
<tbody>
<tr>
<td><b>🔮</b></td>
<td><b>Snowflake Document AI (deprecated)</b></td>
<td><b>Snowflake AI_EXTRACT (current)</b></td>
</tr>
<tr>
<td>Workflow</td>
<td>Create a model build in the UI, upload training docs, define fields, optionally fine-tune, then run PREDICT</td>
<td>Single-step SQL function call with AI_EXTRACT, no UI or training required</td>
</tr>
<tr>
<td>Model</td>
<td>Based on Snowflake Arctic-TILT LLM (with manual fine-tuning)</td>
<td>Uses Snowflake’s new Snowflake Arctic-Extract vision-LLM, no fine-tuning supported</td>
</tr>
<tr>
<td>Model training required</td>
<td>Yes (model builds + publish step)</td>
<td>No (zero-shot)</td>
</tr>
<tr>
<td>Fine-tuning support</td>
<td>Supported (Snowflake Arctic-TILT model)</td>
<td>Not available (zero-shot only)</td>
</tr>
<tr>
<td>Language support</td>
<td>7 languages (English, Spanish, French, German, Portuguese, Italian, Polish)</td>
<td>29 languages (Arabic, Bengali, Burmese, Cebuano, Chinese, Czech, Dutch, English, French, German, Hebrew, Hindi, Indonesian, Italian, Japanese, Khmer, Korean, Lao, Malay, Persian, Polish, Portuguese, Russian, Spanish, Tagalog, Thai, Turkish, Urdu, Vietnamese)</td>
</tr>
<tr>
<td>Document Extraction method</td>
<td>Model build defines extraction schema</td>
<td>responseFormat defines extraction schema at query time</td>
</tr>
<tr>
<td>Input</td>
<td>Files from Snowflake Stage (supported formats only)</td>
<td>Files or text (many formats including PDF, DOCX, PPTX, HTML, images)</td>
</tr>
<tr>
<td>Output</td>
<td>512 tokens per answer (entities), 2048 tokens for table answers</td>
<td>512 tokens per entity answer, 4096 tokens for a table answer</td>
</tr>
<tr>
<td>JSON schema response format</td>
<td>No</td>
<td>Yes</td>
</tr>
<tr>
<td>Confidence scores</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr>
<td>Max docs per query</td>
<td>1000 docs per query (for internal or external stage)</td>
<td>Each call handles one document file (up to 125 pages). Use batching via queries over directory tables</td>
</tr>
<tr>
<td>File size support</td>
<td>(client-side encryption only, small files)</td>
<td>Up to 100 MB per file</td>
</tr>
<tr>
<td>UI?</td>
<td>Has a Snowflake web UI for model building and testing</td>
<td>No UI; SQL interface only</td>
</tr>
<tr>
<td>Deployment</td>
<td>Models were account-specific (migrated to Model Registry)</td>
<td>Fully managed by Snowflake</td>
</tr>
<tr>
<td>Status and migration</td>
<td>Deprecated; UI and !PREDICT API to be decommissioned on February 28, 2026. It is advised to migrate to AI_EXTRACT for improved accuracy, speed and multilingual support</td>
<td>GA since November 2025. Recommended for all new development; supports migration of legacy Snowflake Document AI models via Snowflake Model Registry</td>
</tr>
<tr>
<td>Pricing</td>
<td>Compute-time billing (charged for GPU/warehouse time used during training and inference)</td>
<td>Token-based billing (input + output tokens)</td>
</tr>
</tbody>
</table>
<blockquote><p><b>TL; DR</b>: Snowflake Document AI used to be a more hands-on process with fixed models and limitations. Now, Snowflake AI_EXTRACT is a more modern approach that relies on code and uses large language models. It makes things easier by ditching the model building and fine-tuning steps &#8211; you can just ask questions as you need them. Plus, Snowflake AI_EXTRACT can handle a lot more languages and bigger outputs than the old solution. Switch to AI_EXTRACT if you are using Snowflake Document AI. Do it before February 28, 2026, because that is when the old service ends.</p></blockquote>
<h2>Snowflake AI_EXTRACT vs Snowflake AI_PARSE_DOCUMENT</h2>
<p>Snowflake offers another new function, <a href="https://docs.snowflake.com/en/sql-reference/functions/ai_parse_document" rel="noopener" target="_blank">Snowflake AI_PARSE_DOCUMENT</a>, for document processing. How does it differ from Snowflake AI_EXTRACT?</p>
<p>These two functions are easy to get mixed up with because they both deal with documents, but they are used for different things.</p>
<p><b>AI_PARSE_DOCUMENT </b>is an OCR/layout extraction tool. It takes a document and turns it into text or Markdown, keeping its original layout and structure intact (headings, paragraphs and tables). What it does not do is analyze the document and answer specific questions about it. Its main goal is to make the document readable.</p>
<p><b>Snowflake AI_EXTRACT</b> pulls out specific info from a document. You ask it a question and it gives you a simple, structured JSON answer. You will not get the full raw text of the document, just the details you are looking for.</p>
<blockquote><p><b>Note</b>: If you are porting an existing Snowflake Document AI pipeline, the rule of thumb is: use AI_EXTRACT when you need structured values and AI_PARSE_DOCUMENT when you just need OCR or text with layout. AI_PARSE_DOCUMENT is like exporting everything to JSON, whereas AI_EXTRACT picks out the specific parts you asked for.</p></blockquote>
<pre><b><i>
Table 2</i></b><i>: Difference between Snowflake AI_EXTRACT vs Snowflake AI_PARSE_DOCUMENT</i></pre>
<table>
<tbody>
<tr>
<td><b>🔮</b></td>
<td><b>Snowflake AI_PARSE_DOCUMENT</b></td>
<td><b>Snowflake AI_EXTRACT</b></td>
</tr>
<tr>
<td>Main purpose</td>
<td>Extract raw content (OCR) or layout info from a document. Useful for search indexing or RAG pipelines</td>
<td>Extract specific values (entities, lists, tables) by Q&amp;A. Outputs targeted JSON</td>
</tr>
<tr>
<td>Modes/Options</td>
<td>OCR mode (text only) or LAYOUT mode (includes tables, formatting)</td>
<td>Uses no explicit mode – it automatically handles layout, images, handwriting via the LLM</td>
</tr>
<tr>
<td>Asks questions?</td>
<td>No, returns all text</td>
<td>Yes, via responseFormat</td>
</tr>
<tr>
<td>Input</td>
<td>Files on internal/external stages: PDF, DOC/DOCX, PPT/PPTX, JPEG/JPG, PNG, TIFF/TIF, HTML/HTM, TXT/TEXT. Focuses on document and image formats with OCR fallback</td>
<td>Text strings; files including PDF, PNG, PPTX/PPT, EML, DOC/DOCX, JPEG/JPG, HTM/HTML, TEXT/TXT, TIF/TIFF, BMP, GIF, WEBP, MD. Handles raw text or staged files</td>
</tr>
<tr>
<td>Output format</td>
<td>JSON string containing pages of text (OCR) and/or layout (tables/paragraphs). Returns raw text or Markdown</td>
<td>JSON with only requested answers in fields you defined</td>
</tr>
<tr>
<td>Syntax/Usage</td>
<td>Invoked as a SQL function (AI_EXTRACT(text =&gt; &lt;text&gt;, responseFormat =&gt; &lt;schema&gt;) or with file)</td>
<td>Invoked as a SQL function (AI_PARSE_DOCUMENT(&lt;file&gt;, {&#8216;mode&#8217;: &#8216;LAYOUT&#8217;})). Requires staged files</td>
</tr>
<tr>
<td>Table support</td>
<td>Yes, via LAYOUT mode (Markdown tables)</td>
<td>Yes, via JSON schema</td>
</tr>
<tr>
<td>Image extraction</td>
<td>Can extract embedded images (LAYOUT mode)</td>
<td>Reads visual elements; doesn&#8217;t output images</td>
</tr>
<tr>
<td>Language support</td>
<td><b>9 languages OCR mode</b> (English, French, German, Italian, Norwegian, Polish, Portuguese, Spanish, Swedish)</p>
<p><b>12 languages in LAYOUT mode</b> (Chinese, English, French, German, Hindi, Italian, Portuguese, Romanian, Russian, Spanish, Turkish, Ukrainian)</td>
<td>29 languages (Arabic, Bengali, Burmese, Cebuano, Chinese, Czech, Dutch, English, French, German, Hebrew, Hindi, Indonesian, Italian, Japanese, Khmer, Korean, Lao, Malay, Persian, Polish, Portuguese, Russian, Spanish, Tagalog, Thai, Turkish, Urdu, Vietnamese)</td>
</tr>
<tr>
<td>Confidence scores</td>
<td>No</td>
<td>No</td>
</tr>
<tr>
<td>Common use case</td>
<td>Get all text/structure from docs for indexing or further parsing</td>
<td>Targeted data/document extraction</td>
</tr>
<tr>
<td>Limitations</td>
<td>Files must be staged; &lt; 100 MB and ≤ 125 pages; LAYOUT mode requires more processing; image extraction preview in some regions; no direct text string input</td>
<td>Max 100 entity questions or 10 table questions per call; files &lt; 100 MB and ≤ 125 pages; no confidence scores; no client-side encrypted stages; cannot mix text and file inputs</td>
</tr>
<tr>
<td>When to use it</td>
<td>When you just need text or layout extraction (to feed into another system or RAG). It is more like OCR</td>
<td>When you want structured answers or data fields out of documents directly in Snowflake</td>
</tr>
<tr>
<td>Pricing</td>
<td>Token-based pricing, like other Snowflake Cortex AI functions. Costs depend on document size and mode (LAYOUT more compute-intensive)</td>
<td>Token-based pricing, charged per input/output tokens processed. Cost-efficient for data/document extractions</td>
</tr>
</tbody>
</table>
<blockquote><p><b>TL; DR</b>: AI_PARSE_DOCUMENT is useful for extracting the full text of a document into a readable format, perfect for RAG pipelines, search indexing, or training a summary model. Snowflake AI_EXTRACT is a better choice when you only need to extract specific fields and have them formatted into table-ready, structured JSON.</p></blockquote>
<p>You can also try combining these two functions. First, use AI_PARSE_DOCUMENT to clean up the text and convert it to Markdown. Then, pass that text to Snowflake AI_EXTRACT with a text =&gt; &#8230; call. This two-step approach can sometimes improve accuracy on messy scanned documents.</p>
<h2>What can you use Snowflake AI-EXTRACT for?</h2>
<p>Snowflake AI_EXTRACT can be used anywhere you have documents or images with data you want to put in tables or fields. You can use Snowflake AI_EXTRACT in the following scenarios:</p>
<ul>
<li><b>Invoice and receipt processing </b>— Pull vendor name, invoice number, line items, totals, due dates and tax amounts from PDF invoices; load directly into a staging table for accounts payable processing</li>
<li><b>Contract and legal document analysis</b> — Extract parties, effective dates, termination clauses, renewal terms and more from legal agreements; scale across thousands of contracts without manual review</li>
<li><b>Table extraction for financial data ingestion</b> — Pull structured tables from earnings reports, financial statements and data sheets directly into Snowflake tables</li>
<li><b>Form processing</b> — Extract filled-in values from application forms, tax forms, insurance claims and survey responses; Snowflake AI_EXTRACT handles checkboxes and handwritten fields through its vision model and is not limited to typed text</li>
<li><b>Medical records/clinical notes</b> — Extract patient info, diagnoses, medications and test results from clinical notes and discharge summaries; since Snowflake AI_EXTRACT runs entirely inside Snowflake with role-based access control (RBAC) enforcement, it keeps protected health information (PHI) within your governance perimeter</li>
<li><b>Document search and RAG pipelines</b> — Extract metadata fields (title, author, date, category, key topics) and store them alongside the document reference for downstream search and retrieval; even pair with Cortex Search for semantic lookup</li>
<li><b>Financial reporting</b><b> </b>— Extract key performance indicators (KPIs), revenue figures, segment data and footnotes from quarterly reports across hundreds of filings at once using batch processing from a stage</li>
<li><b>Email and ticket grouping </b>— Pass the body of support tickets, complaint emails, or onboarding forms as text inputs to extract intent, product names, order numbers and customer details without storing them as files first</li>
<li><b>Regulatory and compliance document review</b> — Extract license numbers, compliance attestations, expiry dates and signatory names from regulatory filings at scale</li>
<li><b>General data enrichment</b> — Any time you want to add document-derived metadata to your database (like customer letters, support tickets, or user-written comments)</li>
</ul>
<p>So, whenever you have unstructured documents with data you want to query or analyze in Snowflake, AI_EXTRACT is a big help. You define the fields once, and the LLM does the rest. This saves engineering time and reduces errors, making it better than manual processes.</p>
<h2>How to calculate Snowflake AI_EXTRACT costs?</h2>
<p>Snowflake AI_EXTRACT incurs two main kinds of costs: <b>warehouse compute credits</b> and <b>Cortex (token-based) credits</b>.</p>
<h3>1) Warehouse compute costs</h3>
<p>Every query you run still runs on a Snowflake virtual warehouse, so you pay for the warehouse time (just like any query). Because AI_EXTRACT is I/O-bound (the heavy LLM work is done by Snowflake’s managed service), you do not need a large warehouse. In fact, Snowflake explicitly recommends using a MEDIUM warehouse or smaller for AI_EXTRACT. Remember that bigger clusters do not speed up the LLM inference or reduce latency for AI functions. It just burns more credits on warehouse compute without any benefit. Your warehouse cost is therefore usually minimal, but if you queue many documents in parallel on a large cluster, it can add up fast.</p>
<h3>2) Cortex AI (Token) billing</h3>
<p>This is where the actual cost comes from. Snowflake AI_EXTRACT bills on both<b> input tokens </b>and <b>output tokens</b> using the Snowflake Arctic-Extract model, which runs at <i>~5 credits per million tokens</i> (standard, non-fine-tuned).</p>
<p><strong>What counts as input tokens </strong></p>
<p>Input token billing has three components:</p>
<ul>
<li>Document content. Either page-based token counts for file inputs or actual text length for string inputs (more on this below)</li>
<li>Your responseFormat payload. The questions, field names, and schema you pass in the argument all count as input tokens</li>
<li>Snowflake&#8217;s managed system prompts. This is added automatically to every call, and you cannot see or control it</li>
</ul>
<p><b>How do document pages convert to tokens? </b></p>
<p>For file-based inputs, Snowflake does not tokenize the actual text on the page. It uses a flat rate based on document format:</p>
<p>&nbsp;</p>
<pre><b><i>Table 3</i></b><i>: Snowflake Cortex AI token cost breakdown</i></pre>
<table>
<tbody>
<tr>
<td><b>Input format</b></td>
<td><b>How pages are counted</b></td>
</tr>
<tr>
<td>PDF, DOCX, TIF, TIFF</td>
<td>Each page = 970 input tokens</td>
</tr>
<tr>
<td>JPEG, JPG, PNG</td>
<td>Each image file = 970 input tokens (treated as a single page)</td>
</tr>
<tr>
<td>Plain text strings</td>
<td>Actual token count of the text (~4 characters per token)</td>
</tr>
</tbody>
</table>
<blockquote><p><b style="font-size: 1.25rem;">Note</b>: The cost of a mostly blank PDF page is the same as a dense one with a lot of text, due to the flat 970 token rate.</p></blockquote>
<p><strong>Output token limits </strong></p>
<p>Output tokens are billed based on the length of the JSON response AI_EXTRACT returns. The caps depend on what you are extracting:</p>
<ul>
<li><b>Entity and list extractions </b>– 512 output tokens per question (maximum 100 questions per call)</li>
<li><b>Table extractions</b> – 4,096 output tokens per question (maximum 10 questions per call)</li>
</ul>
<p>Table extraction can be expensive. If you are pulling full structured tables from documents, those output tokens can easily dominate the total cost of a call.</p>
<p>The token billing rate itself (5 credits per million tokens for standard Snowflake Arctic-Extract) is consistent across cloud providers. What changes by region is the dollar value of a Snowflake credit.</p>
<blockquote><p>Check the current rates in <a href="https://www.snowflake.com/legal-files/CreditConsumptionTable.pdf" rel="noopener" target="_blank">Snowflake&#8217;s Service Consumption Table</a>.</p></blockquote>
<h4>Sample cost estimate</h4>
<p>Let us say we process 1k invoices. Each invoice has 3 pages, and we ask 5 entity questions per invoice.</p>
<ul>
<li>Input tokens per invoice ⇒ (3 pages × 970) + responseFormat tokens + system prompt ~3100 input tokens</li>
<li>Output tokens per invoice ⇒ 5 questions × ~100 tokens average ~ 500 output tokens</li>
<li>Total per invoice ⇒ ~3600 tokens</li>
<li>Total for 1k invoices ⇒ ~3.6 million tokens</li>
</ul>
<p>At 5 credits per million tokens, that is 18 credits. On AWS US East, at the Standard edition rate ($2/credit), it costs $36 for Cortex billing.</p>
<p>Add warehouse compute on top. An XS warehouse runs at 1 credit per hour. A 20-minute batch adds another ~0.33 credits, which is negligible.</p>
<p>These are rough estimates. Always run a sample before processing a large batch.</p>
<p>To monitor spend, use Snowflake’s account usage views.</p>
<p>Snowflake provides the <a href="https://docs.snowflake.com/en/sql-reference/account-usage/cortex_functions_usage_history" rel="noopener" target="_blank">CORTEX_FUNCTIONS_USAGE_HISTORY</a> account usage view to track Cortex token consumption.</p>
<p>You can also use <a href="https://docs.snowflake.com/en/sql-reference/account-usage/metering_daily_history" rel="noopener" target="_blank">METERING_DAILY_HISTORY</a> to track overall credit consumption at the account level and set budget alerts.</p>
<h2>How to configure and use Snowflake AI_EXTRACT to extract data from documents?</h2>
<p>Now, we have gotten the basics covered, so let us dive into the practical side of things. Next up, we&#8217;ll walk through setting up Snowflake AI_EXTRACT step by step, starting from an absolute scratch.</p>
<h3>Prerequisites and setups:</h3>
<p>Here is what you should have before you start:</p>
<ul>
<li>A Snowflake account on an edition/region that supports Snowflake Cortex AI functions and AI_EXTRACT.</li>
<li>The ACCOUNTADMIN role, or a role with the ability to grant CORTEX_USER privileges</li>
<li>A warehouse, database and schema to hold Snowflake Stages, tables and tasks. Use a dedicated warehouse for Cortex workloads to track credits separately.</li>
<li>Cross-region inference enabled if your region doesn&#8217;t natively support AI_EXTRACT (covered in Step 5 below)</li>
<li>Documents in a supported format (PDF, DOC, DOCX, PNG, JPEG, JPG, TIFF, TIF, HTML, HTM, TXT, TEXT, PPT, EML, BMP, GIF, WEBP, or MD). Files must be &lt; 100 MB and &lt; 125 pages. Snowflake AI_EXTRACT limits: up to 100 entity extraction questions or 10 table extraction questions per call; tables count as 10 entities each.</li>
</ul>
<h3>Step 1—Sign in to Snowsight or SnowSQL</h3>
<p>Log in to your Snowflake account via the <a href="https://www.flexera.com/blog/finops/snowflake-snowsight-guide/" rel="noopener" target="_blank">Snowsight web interface</a> or the <a href="https://www.flexera.com/blog/finops/snowflake-snowsql-cli/" rel="noopener" target="_blank">Snowflake CLI</a>. All SQL in this guide runs in Snowsight or via SnowSQL.</p>
<h3>Step 2—Create a dedicated warehouse, database and schema</h3>
<p>Now, with your session open, the first real task is setting up the infrastructure. Keep Snowflake AI_EXTRACT workloads on a dedicated, right-sized warehouse- that way, it is easier to track costs later. (The MEDIUM size is recommended; X-Small or Small also often suffice). The warehouse will charge credits while running your AI queries.</p>
<pre class="codebox">CREATE WAREHOUSE IF NOT EXISTS doc_extract_wh 

    WITH WAREHOUSE_SIZE = 'MEDIUM' 

    AUTO_SUSPEND = 60 

    AUTO_RESUME = TRUE;</pre>
<div class="wp-caption aligncenter" id="attachment_34073" style="width: 335px;"><img alt="Creating the doc_extract_wh Snowflake virtual warehouse" class="wp-image-34073 size-full" height="196" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-2.png" width="325" /><p class="wp-caption-text" id="caption-attachment-34073">Figure 2: Creating the doc_extract_wh Snowflake virtual warehouse</p></div>
<p>Now create the database and schema that will hold your stages, tables and tasks.</p>
<pre class="codebox">CREATE DATABASE IF NOT EXISTS doc_processing_db; 

CREATE SCHEMA IF NOT EXISTS doc_processing_db.extraction;</pre>
<div class="wp-caption aligncenter" id="attachment_34074" style="width: 421px;"><img alt="Creating the doc_processing_db database and extraction schema—Snowflake AI_EXTRACT " class="wp-image-34074 size-full" height="165" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-3.png" width="411" /><p class="wp-caption-text" id="caption-attachment-34074">Figure 3: Creating the doc_processing_db database and extraction schema—Snowflake AI_EXTRACT</p></div>
<h3>Step 3—Create a custom role and assign least privileges</h3>
<p>Do not run extraction workloads under ACCOUNTADMIN in production. The principle of least privilege applies here just as much as anywhere else—scope the permissions to exactly what the pipeline needs.</p>
<pre class="codebox">USE ROLE ACCOUNTADMIN; 

CREATE ROLE IF NOT EXISTS doc_extractor_role;</pre>
<div class="wp-caption aligncenter" id="attachment_34075" style="width: 339px;"><img alt="Creating the doc_extractor_role custom role—Snowflake AI_EXTRACT " class="wp-image-34075 size-full" height="204" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-4.png" width="329" /><p class="wp-caption-text" id="caption-attachment-34075">Figure 4: Creating the doc_extractor_role custom role—Snowflake AI_EXTRACT</p></div>
<p>Now, grant the role access to the warehouse, database and schema objects it&#8217;ll need.</p>
<pre class="codebox">GRANT USAGE ON WAREHOUSE doc_extract_wh TO ROLE doc_extractor_role; 

GRANT USAGE ON DATABASE doc_processing_db TO ROLE doc_extractor_role; 

GRANT USAGE ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

GRANT CREATE TABLE ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

GRANT CREATE STAGE ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

GRANT CREATE STREAM ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

GRANT CREATE TASK ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role;</pre>
<div class="wp-caption aligncenter" id="attachment_34076" style="width: 574px;"><img alt="Granting warehouse, database and schema privileges to doc_extractor_role—Snowflake AI_EXTRACT " class="wp-image-34076 size-full" height="233" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-5.png" width="564" /><p class="wp-caption-text" id="caption-attachment-34076">Figure 5: Granting warehouse, database and schema privileges to doc_extractor_role—Snowflake AI_EXTRACT</p></div>
<h3>Step 4—Grant Cortex privileges (control access)</h3>
<p>The CORTEX_USER database role in Snowflake includes the necessary privileges to call Snowflake Cortex AI functions. It is automatically granted to the PUBLIC role, which means every user and role gets it by default.</p>
<p>For a production environment, it is a good idea to have more control over who can call Cortex functions. To do this, grant the CORTEX_USER role directly to a custom role you have created.</p>
<pre class="codebox">GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE doc_extractor_role; 

GRANT ROLE doc_extractor_role TO USER &lt;your_username&gt;;</pre>
<div class="wp-caption aligncenter" id="attachment_34077" style="width: 464px;"><img alt="Granting SNOWFLAKE.CORTEX_USER to doc_extractor_role and assigning it to a user—Snowflake AI_EXTRACT " class="wp-image-34077 size-full" height="180" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-6.png" width="454" /><p class="wp-caption-text" id="caption-attachment-34077">Figure 6: Granting SNOWFLAKE.CORTEX_USER to doc_extractor_role and assigning it to a user—Snowflake AI_EXTRACT</p></div>
<p>You must run these as ACCOUNTADMIN or a similarly powerful role. Now, any user with doc_extractor_role can call Snowflake Cortex AI functions. Also make sure doc_extractor_role (or the granting role) has USAGE on the target warehouse, database and schema.</p>
<h3>Step 5—(Optional) Enable cross-region inference</h3>
<p>Snowflake AI_EXTRACT is natively available in select Snowflake regions. If your account is in a region that does not support it natively, you need to enable cross-region inference via the <a href="https://docs.snowflake.com/en/sql-reference/parameters#label-cortex-enable-cross-region" rel="noopener" target="_blank">CORTEX_ENABLED_CROSS_REGION</a> account parameter.</p>
<pre class="codebox">USE ROLE ACCOUNTADMIN;</pre>
<p>To allow inference in any supported region:</p>
<pre class="codebox">ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'ANY_REGION';</pre>
<p>Or restrict processing to a specific cloud or region group:</p>
<pre class="codebox">ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'AWS_US';</pre>
<blockquote><p><b>Note</b>: Cross-region inference is not supported in U.S. SnowGov regions. User inputs, service-generated prompts and outputs are not stored or cached during cross-region inference. If both the source and destination regions are on AWS, the data stays within the AWS global network and is automatically encrypted at the physical layer.</p></blockquote>
<h3>Step 6—Switch role, set context, create prompt/metadata tables</h3>
<p>Switch to your new role and set the working context. From here on, all objects are created under doc_extractor_role.</p>
<pre class="codebox">USE ROLE doc_extractor_role; 

USE WAREHOUSE doc_extract_wh; 

USE DATABASE doc_processing_db; 

USE SCHEMA extraction;</pre>
<div class="wp-caption aligncenter" id="attachment_34079" style="width: 291px;"><img alt="Setting the active role, warehouse, database and schema—Snowflake AI_EXTRACT " class="wp-image-34079 size-full" height="193" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-7-1.png" width="281" /><p class="wp-caption-text" id="caption-attachment-34079">Figure 7: Setting the active role, warehouse, database and schema—Snowflake AI_EXTRACT</p></div>
<p>Now create a table to track documents queued for processing.</p>
<pre class="codebox">CREATE OR REPLACE TABLE document_queue ( 

    document_id     VARCHAR(100), 

    file_path       VARCHAR(500), 

    document_type   VARCHAR(50), 

    uploaded_at     TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP(), 

    processed_at    TIMESTAMP_NTZ, 

    status          VARCHAR(20) DEFAULT 'pending'  -- (pending, processed, error) 

);</pre>
<div class="wp-caption aligncenter" id="attachment_34080" style="width: 526px;"><img alt="Creating the document_queue tracking table—Snowflake AI_EXTRACT" class="wp-image-34080 size-full" height="238" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-8.png" width="516" /><p class="wp-caption-text" id="caption-attachment-34080">Figure 8: Creating the document_queue tracking table—Snowflake AI_EXTRACT</p></div>
<p>And a table to store the extraction output.</p>
<pre class="codebox">CREATE OR REPLACE TABLE extraction_results ( 

    document_id     VARCHAR(100), 

    file_path       VARCHAR(500), 

    extracted_at    TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP(), 

    raw_response    VARIANT, 

    error_message   VARCHAR(500) 

);</pre>
<div class="wp-caption aligncenter" id="attachment_34081" style="width: 415px;"><img alt="Creating the extraction_results output table—Snowflake AI_EXTRACT" class="wp-image-34081 size-full" height="224" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-9.png" width="405" /><p class="wp-caption-text" id="caption-attachment-34081">Figure 9: Creating the extraction_results output table—Snowflake AI_EXTRACT</p></div>
<h3>Step 7—Create Snowflake internal stage for document storage</h3>
<p>Snowflake Cortex AI functions that process media files require the files to be stored on an internal or external stage. The Snowflake internal stage must use server-side encryption (client-side encrypted stages are not supported by Snowflake AI_EXTRACT). You also need a directory table to query stage contents or run batch processing.</p>
<p>Let&#8217;s create Snowflake internal stage with server-side encryption and directory table enabled.</p>
<pre class="codebox">CREATE OR REPLACE STAGE doc_processing_db.extraction.documents_stage 

  DIRECTORY = (ENABLE = TRUE) 

  ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE') 

  COMMENT = 'Snowflake Internal stage for documents for Snowflake AI_EXTRACT';</pre>
<div class="wp-caption aligncenter" id="attachment_34082" style="width: 512px;"><img alt="Creating “documents_stage” Snowflake internal stage with SSE encryption and directory table enabled—Snowflake AI_EXTRACT" class="wp-image-34082 size-full" height="196" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-10.png" width="502" /><p class="wp-caption-text" id="caption-attachment-34082">Figure 10: Creating “documents_stage” Snowflake internal stage with SSE encryption and directory table enabled—Snowflake AI_EXTRACT</p></div>
<p>The DIRECTORY = (ENABLE = TRUE) setting is what lets you run FROM DIRECTORY(@documents_stage) in batch queries later. Do not skip this.</p>
<blockquote><p><b>Note</b>: SNOWFLAKE_SSE is server-side encryption managed by Snowflake. Client-side encrypted stages are not supported by Snowflake AI_EXTRACT.</p></blockquote>
<h3>Step 8—Uploading files to the Snowflake Internal Stage</h3>
<p>Now, with the Snowflake Stage ready, you can load documents. There are a few ways to do it.</p>
<p><b>Via Snowsight: </b></p>
<p>Navigate to <b>Catalog &gt; Database Explorer &gt; doc_processing_db &gt; extraction &gt; Stages &gt; documents_stage &gt;Stage Files</b>, then drag and drop your files directly into the UI.</p>
<div class="wp-caption aligncenter" id="attachment_34083" style="width: 1903px;"><img alt="Uploading files via the Snowsight interface—Snowflake AI_EXTRACT " class="wp-image-34083 size-full" height="640" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-11.png" width="1893" /><p class="wp-caption-text" id="caption-attachment-34083">Figure 11: Uploading files via the Snowsight interface—Snowflake AI_EXTRACT</p></div>
<p><b>Via SnowSQL CLI:</b></p>
<pre class="codebox">snowsql -a &lt;account&gt; -u &lt;user&gt; 

PUT file:///local/path/to/&lt;your_file&gt;.pdf @doc_processing_db.extraction.documents_stage/invoices/ OVERWRITE = TRUE;</pre>
<p><b>Via SQL (Python connector or Snowpark):</b></p>
<pre class="codebox"># via snowflake-connector-python 

import snowflake.connector 

conn = snowflake.connector.connect( 

    user='&lt;user&gt;', 

    password='&lt;password&gt;', 

    account='&lt;account&gt;' 

) 

cs = conn.cursor() 

cs.execute("PUT file:///local/path/*.pdf @doc_processing_db.extraction.documents_stage/invoices/ OVERWRITE=TRUE")</pre>
<pre>After uploading, refresh the directory table, so Snowflake indexes the new files.</pre>
<pre class="codebox">ALTER STAGE doc_processing_db.extraction.documents_stage REFRESH;</pre>
<p>Finally, verify the upload:</p>
<pre class="codebox">SELECT RELATIVE_PATH, SIZE, LAST_MODIFIED 

FROM DIRECTORY(@doc_processing_db.extraction.documents_stage) 

WHERE RELATIVE_PATH LIKE 'invoices/%';</pre>
<div class="wp-caption aligncenter" id="attachment_34084" style="width: 676px;"><img alt="Verifying uploaded files via the DIRECTORY() table function—Snowflake AI_EXTRACT " class="wp-image-34084 size-full" height="182" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-12.png" width="666" /><p class="wp-caption-text" id="caption-attachment-34084">Figure 12: Verifying uploaded files via the DIRECTORY() table function—Snowflake AI_EXTRACT</p></div>
<p>Or</p>
<pre class="codebox">LIST @doc_processing_db.extraction.documents_stage/invoices/;</pre>
<div class="wp-caption aligncenter" id="attachment_34085" style="width: 854px;"><img alt="Verifying uploaded files via LIST—Snowflake AI_EXTRACT " class="wp-image-34085 size-full" height="150" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-13.png" width="844" /><p class="wp-caption-text" id="caption-attachment-34085">Figure 13: Verifying uploaded files via LIST—Snowflake AI_EXTRACT</p></div>
<blockquote><p><b>Note</b>: for external stages you can configure automatic directory refresh via cloud notifications; for Snowflake internal stages ALTER STAGE &#8230; REFRESH is the manual refresh command.</p></blockquote>
<p>Here is a screenshot of what our invoice looks like:</p>
<div class="wp-caption aligncenter" id="attachment_34086" style="width: 604px;"><img alt="Sample invoice for testing extraction logic—Snowflake AI_EXTRACT " class="wp-image-34086 size-full" height="753" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-14.png" width="594" /><p class="wp-caption-text" id="caption-attachment-34086">Figure 14: Sample invoice for testing extraction logic—Snowflake AI_EXTRACT</p></div>
<h3>Step 9—Define document extraction schema and sample responseFormat</h3>
<p>Here is where the real design work happens. A well-crafted responseFormat is the single biggest factor in extraction quality.</p>
<p>Let us look at some realistic examples.</p>
<p><strong>Simple key-value document extraction </strong></p>
<p>The simplest format is a flat object that maps field names to natural language questions.</p>
<pre class="codebox">SELECT AI_EXTRACT( 

  file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', 'invoices/snowflake_ai_extract_invoice_001.pdf'), 

  responseFormat =&gt; { 

    'vendor_name': 'What is the vendor or supplier name on this invoice?', 

    'invoice_number': 'What is the invoice number or ID?', 

    'invoice_date': 'What is the invoice date?', 

    'due_date': 'What is the payment due date?', 

    'total_amount': 'What is the total amount due?' 

  } 

);</pre>
<div class="wp-caption aligncenter" id="attachment_34087" style="width: 1480px;"><img alt="Running a key-value extraction with Snowflake AI_EXTRACT " class="wp-image-34087 size-full" height="418" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-15.png" width="1470" /><p class="wp-caption-text" id="caption-attachment-34087">Figure 15: Running a key-value extraction with Snowflake AI_EXTRACT</p></div>
<p><strong>Array document extraction (list of values) </strong></p>
<p>When you need a list of values rather than a single answer, use the JSON schema format with &#8220;type&#8221;: &#8220;array.&#8221;</p>
<pre class="codebox">SELECT AI_EXTRACT( 

  file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', 'invoices/snowflake_ai_extract_invoice_001.pdf'), 

  responseFormat =&gt; { 

    'schema': { 

      'type': 'object', 

      'properties': { 

        'line_item_descriptions': { 

          'description': 'What are all the product or service descriptions listed on this invoice?', 

          'type': 'array' 

        } 

      } 

    } 

  } 

);</pre>
<div class="wp-caption aligncenter" id="attachment_34089" style="width: 1480px;"><img alt="Extracting a list of values using an array schema—Snowflake AI_EXTRACT " class="wp-image-34089 size-full" height="513" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-16.png" width="1470" /><p class="wp-caption-text" id="caption-attachment-34089">Figure 16: Extracting a list of values using an array schema—Snowflake AI_EXTRACT</p></div>
<p><strong>Table extraction with column ordering </strong></p>
<p>For extracting tabular data (like a full line-items table) use an &#8220;type&#8221;: &#8220;object&#8221; with column_ordering and array-typed columns. You can mix table extraction with scalar fields in the same call.</p>
<pre class="codebox">SELECT AI_EXTRACT( 

  file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', 'invoices/snowflake_ai_extract_invoice_001.pdf'), 

  responseFormat =&gt; { 

    'schema': { 

      'type': 'object', 

      'properties': { 

        'line_items': { 

          'description': 'The itemized line items table on this invoice', 

          'type': 'object', 

          'column_ordering': ['description', 'quantity', 'unit_price', 'line_total'], 

          'properties': { 

            'description': { 'type': 'array' }, 

            'quantity': { 'type': 'array' }, 

            'unit_price': { 'type': 'array' }, 

            'line_total': { 'type': 'array' } 

          } 

        }, 

        'vendor': { 

          'description': 'What is the vendor name?', 

          'type': 'string' 

        }, 

        'invoice_total': { 

          'description': 'What is the total amount due?', 

          'type': 'string' 

        } 

      } 

    } 

  } 

);</pre>
<p>The description field is your main lever for improving extraction accuracy. Vague descriptions produce vague results. Be very very specific, especially on documents with multiple subtotals, nested tables, or repeated fields. That extra context helps the model locate the right value.</p>
<div class="wp-caption aligncenter" id="attachment_34090" style="width: 1525px;"><img alt="Extracting table data with column ordering defined—Snowflake AI_EXTRACT " class="wp-image-34090 size-full" height="808" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-17-1.png" width="1515" /><p class="wp-caption-text" id="caption-attachment-34090">Figure 17: Extracting table data with column ordering defined—Snowflake AI_EXTRACT</p></div>
<blockquote><p><b>Note</b>: In a single Snowflake AI_EXTRACT call, you can ask a maximum of 100 entity extraction questions, or up to 10 table extraction questions (each table question counts as 10 entity questions toward the limit).</p></blockquote>
<h3>Step 10—(Optional) Create Snowflake AI_EXTRACT wrapper function</h3>
<p>If you are running the same extraction schema repeatedly, wrapping it in a user-defined function (UDF) reduces repetition and makes schema updates a one-place change.</p>
<pre class="codebox">GRANT CREATE FUNCTION ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

CREATE OR REPLACE FUNCTION doc_processing_db.extraction.extract_invoice_fields( 

  stage_path VARCHAR, 

  file_name  VARCHAR 

) 

RETURNS VARIANT 

LANGUAGE SQL 

AS 

$$ 

  CAST( 

    AI_EXTRACT( 

      file =&gt; TO_FILE(stage_path, file_name), 

      responseFormat =&gt; { 

        'vendor_name':    'What is the name of the vendor or supplier?', 

        'invoice_number': 'What is the invoice number?', 

        'invoice_date':   'What is the invoice date?', 

        'due_date':       'What is the payment duenames,?', 

        'total_amount':   'What is the total amount due including taxes?', 

        'currency':       'What currency is used?', 

        'payment_terms':  'What are the payment terms?' 

      } 

    ) AS VARIANT 

  ) 

$$;</pre>
<div class="wp-caption aligncenter" id="attachment_34091" style="width: 518px;"><img alt="Creating extract_invoice_fields as a reusable SQL UDF wrapping Snowflake AI_EXTRACT " class="wp-image-34091 size-full" height="438" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-18.png" width="508" /><p class="wp-caption-text" id="caption-attachment-34091">Figure 18: Creating extract_invoice_fields as a reusable SQL UDF wrapping Snowflake AI_EXTRACT</p></div>
<p>Here&#8217;s how to call it:</p>
<pre class="codebox">SELECT doc_processing_db.extraction.extract_invoice_fields( 

  '@doc_processing_db.extraction.documents_stage', 

  'invoices/snowflake_ai_extract_invoice_001.pdf' 

) AS extraction_result;</pre>
<div class="wp-caption aligncenter" id="attachment_34092" style="width: 1531px;"><img alt="Testing the wrapper function with the sample invoice—Snowflake AI_EXTRACT " class="wp-image-34092 size-full" height="347" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-19.png" width="1521" /><p class="wp-caption-text" id="caption-attachment-34092">Figure 19: Testing the wrapper function with the sample invoice—Snowflake AI_EXTRACT</p></div>
<h3>Step 11—Test Snowflake AI_EXTRACT on sample documents</h3>
<p>Before building automation, test your document extraction logic manually. Run through each of these patterns to confirm everything works in your environment.</p>
<h4><b>Test 1</b>–Extract from a text string</h4>
<p>Snowflake AI_EXTRACT works on plain text too, not just files. It&#8217;s good for quick prototyping.</p>
<pre class="codebox">SELECT AI_EXTRACT( 

  text =&gt; 'Invoice #INV-2026-NVD-800 from Dragon Corp dated Feb 20, 2026. Total due: $7,432,250. Payment due by February 21, 2026.', 

  responseFormat =&gt; { 

    'vendor': 'What is the vendor name?', 

    'invoice_number': 'What is the invoice number?', 

    'total_due': 'What is the total amount due?', 

    'due_date': 'What is the due date?' 

  } 

) AS extraction_result;</pre>
<div class="wp-caption aligncenter" id="attachment_34093" style="width: 969px;"><img alt="Extracting fields from an inline text string using Snowflake AI_EXTRACT " class="wp-image-34093 size-full" height="367" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-20.png" width="959" /><p class="wp-caption-text" id="caption-attachment-34093">Figure 20: Extracting fields from an inline text string using Snowflake AI_EXTRACT</p></div>
<p>&nbsp;</p>
<h4>Test 2–Extract from a single PDF</h4>
<pre class="codebox">SELECT AI_EXTRACT( 

  file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', 'invoices/snowflake_ai_extract_invoice_001.pdf'), 

  responseFormat =&gt; {'vendor': 'Vendor name', 'total': 'Total amount due'} 

) AS result;</pre>
<div class="wp-caption aligncenter" id="attachment_34094" style="width: 884px;"><img alt="Extracting vendor name and total from a single PDF using Snowflake AI_EXTRACT " class="wp-image-34094 size-full" height="237" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-21.png" width="874" /><p class="wp-caption-text" id="caption-attachment-34094">Figure 21: Extracting vendor name and total from a single PDF using Snowflake AI_EXTRACT</p></div>
<p>&nbsp;</p>
<h4><b>Test 3</b>–Extract table data with JSON schema</h4>
<pre class="codebox">SELECT 

  relative_path, 

  AI_EXTRACT( 

    file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', relative_path), 

    responseFormat =&gt; { 

      'schema': { 

        'type': 'object', 

        'properties': { 

          'charges': { 

            'description': 'Itemized charges table', 

            'type': 'object', 

            'column_ordering': ['item', 'amount'], 

            'properties': { 

              'item': { 'type': 'array' }, 

              'amount': { 'type': 'array' } 

            } 

          } 

        } 

      } 

    } 

  ) AS extraction_result 

FROM DIRECTORY(@doc_processing_db.extraction.documents_stage) 

WHERE relative_path LIKE 'invoices/%';</pre>
<div class="wp-caption aligncenter" id="attachment_34095" style="width: 1529px;"><img alt="Batch table extraction across all invoice files in the Snowflake Stage using Snowflake AI_EXTRACT " class="wp-image-34095 size-full" height="626" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-22.png" width="1519" /><p class="wp-caption-text" id="caption-attachment-34095">Figure 22: Batch table extraction across all invoice files in the Snowflake Stage using Snowflake AI_EXTRACT</p></div>
<p>&nbsp;</p>
<h4><b>Test 4</b>–Parse the JSON output into columns</h4>
<p>The raw output from Snowflake AI_EXTRACT is a VARIANT with response and error keys. Use Snowflake&#8217;s semi-structured data operators to flatten it into columns.</p>
<pre class="codebox">WITH extracted AS ( 

  SELECT 

    relative_path, 

    AI_EXTRACT( 

      file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', relative_path), 

      responseFormat =&gt; { 

        'vendor': 'Vendor name', 

        'invoice_number': 'Invoice number', 

        'total': 'Total amount due' 

      } 

    ) AS result 

  FROM DIRECTORY(@doc_processing_db.extraction.documents_stage) 

  WHERE relative_path LIKE 'invoices/%' 

) 

SELECT 

  relative_path, 

  result:response:vendor::VARCHAR AS vendor, 

  result:response:invoice_number::VARCHAR AS invoice_number, 

  result:response:total::VARCHAR AS total, 

  result:error::VARCHAR AS error 

FROM extracted;</pre>
<div class="wp-caption aligncenter" id="attachment_34096" style="width: 1537px;"><img alt="Parsing JSON extraction output into relational columns—Snowflake AI_EXTRACT " class="wp-image-34096 size-full" height="390" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-23.png" width="1527" /><p class="wp-caption-text" id="caption-attachment-34096">Figure 23: Parsing JSON extraction output into relational columns—Snowflake AI_EXTRACT</p></div>
<p>&nbsp;</p>
<h3>Step 12—Set up Snowflake Streams and Snowflake Tasks for automated processing</h3>
<p>Once your document extraction logic is in place, automate it. A Stream on the stage directory table detects new files; a Snowflake Task processes them on a schedule.</p>
<p>First, create a <a href="https://www.flexera.com/blog/finops/snowflake-stream-guide/" rel="noopener" target="_blank">Snowflake Stream</a> on the directory table to detect new files.</p>
<pre class="codebox">CREATE OR REPLACE STREAM documents_stream 

    ON STAGE doc_processing_db.extraction.documents_stage;</pre>
<div class="wp-caption aligncenter" id="attachment_34097" style="width: 556px;"><img alt="Creating a stream to detect new file uploads—Snowflake AI_EXTRACT " class="wp-image-34097 size-full" height="151" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-24.png" width="546" /><p class="wp-caption-text" id="caption-attachment-34097">Figure 24: Creating a stream to detect new file uploads—Snowflake AI_EXTRACT</p></div>
<p>Then, create a <a href="https://www.flexera.com/blog/finops/automate-sql-snowflake-tasks/">Snowflake Task</a> that runs every 5 minutes and processes new documents</p>
<pre class="codebox">CREATE OR REPLACE TASK process_new_documents 

  WAREHOUSE = doc_extract_wh 

  SCHEDULE  = '5 MINUTE'  

  WHEN SYSTEM$STREAM_HAS_DATA('documents_stream') 

AS 

INSERT INTO extraction_results (document_id, file_path, extracted_at, raw_response, error_message) 

SELECT 

  MD5(RELATIVE_PATH) AS document_id, 

  RELATIVE_PATH AS file_path, 

  CURRENT_TIMESTAMP() AS extracted_at, 

  AI_EXTRACT( 

    file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', RELATIVE_PATH), 

    responseFormat =&gt; { 

      'vendor_name':    'What is the vendor name?', 

      'invoice_number': 'What is the invoice number?', 

      'invoice_date':   'What is the invoice date?', 

      'total_amount':   'What is the total amount due?' 

    } 

  ) AS raw_response, 

  AI_EXTRACT( 

    file =&gt; TO_FILE('@doc_processing_db.extraction.documents_stage', RELATIVE_PATH), 

    responseFormat =&gt; {'error_check': 'Return "NO_ERROR" if extraction successful, otherwise return the error message.'} 

  ):response:error::VARCHAR AS error_message 

FROM documents_stream 

WHERE METADATA$ACTION = 'INSERT';</pre>
<div class="wp-caption aligncenter" id="attachment_34098" style="width: 681px;"><img alt="Creating the process_new_documents Snowflake Task to automate Snowflake AI_EXTRACT on new stage files " class="wp-image-34098 size-full" height="422" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-25.png" width="671" /><p class="wp-caption-text" id="caption-attachment-34098">Figure 25: Creating the process_new_documents Snowflake Task to automate Snowflake AI_EXTRACT on new stage files</p></div>
<p>Finally, start the Snowflake Task. Tasks are created in a suspended state by default.</p>
<pre class="codebox"><code>ALTER TASK process_new_documents RESUME; </code></pre>
<div class="wp-caption aligncenter" id="attachment_34099" style="width: 313px;"><img alt="Resuming the task to enable automation—Snowflake AI_EXTRACT " class="wp-image-34099 size-full" height="133" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-26.png" width="303" /><p class="wp-caption-text" id="caption-attachment-34099">Figure 26: Resuming the task to enable automation—Snowflake AI_EXTRACT</p></div>
<h3>Step 13—Upload new documents for automated processing</h3>
<p>Once Snowflake Stream and Snowflake Task are running, any new file you upload will be picked up automatically on the next task run (within 5 minutes by default).</p>
<p>Let&#8217;s start by uploading a new invoice via Snowsight (the process is the same as above) or you can do so via SnowSQL</p>
<pre class="codebox">PUT file:///path/to/new_invoice.pdf @doc_processing_db.extraction.documents_stage/invoices/ OVERWRITE=TRUE;</pre>
<p>Then refresh the directory table so the Stream can detect the new file.</p>
<pre class="codebox">ALTER STAGE doc_processing_db.extraction.documents_stage REFRESH;</pre>
<h3>Step 14—Create error handling and validation</h3>
<p>Snowflake AI_EXTRACT does not raise SQL exceptions for extraction failures. It returns an error key in the JSON response instead. If you do not build validation into your pipeline, failed extractions will silently pile up in your results table.</p>
<p>To do so, first create a view to surface failed extractions.</p>
<pre class="codebox">GRANT CREATE VIEW ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

CREATE OR REPLACE VIEW extraction_errors AS 

SELECT 

  file_path, 

  extracted_at, 

  raw_response:error::VARCHAR AS error_message 

FROM extraction_results 

WHERE raw_response:error IS NOT NULL 

  OR error_message IS NOT NULL;</pre>
<div class="wp-caption aligncenter" id="attachment_34100" style="width: 511px;"><img alt="Creating a view to surface failed extractions—Snowflake AI_EXTRACT " class="wp-image-34100 size-full" height="244" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-27.png" width="501" /><p class="wp-caption-text" id="caption-attachment-34100">Figure 27: Creating a view to surface failed extractions—Snowflake AI_EXTRACT</p></div>
<p>Then set up a Snowflake Task that logs errors hourly.</p>
<pre class="codebox">CREATE TABLE IF NOT EXISTS extraction_error_log ( 

  logged_at TIMESTAMP_NTZ, 

  error_count INTEGER, 

  sample_files ARRAY 

); 

 

CREATE OR REPLACE TASK alert_on_extraction_errors 

  WAREHOUSE = doc_extract_wh 

  SCHEDULE = '60 MINUTE' 

AS 

INSERT INTO extraction_error_log (logged_at, error_count, sample_files) 

SELECT 

  CURRENT_TIMESTAMP(), 

  COUNT(*), 

  ARRAY_AGG(file_path) WITHIN GROUP (ORDER BY extracted_at DESC) 

FROM extraction_errors 

WHERE extracted_at &gt;= DATEADD(hour, -1, CURRENT_TIMESTAMP());</pre>
<div class="wp-caption aligncenter" id="attachment_34101" style="width: 437px;"><img alt="Setting up an hourly Snowflake Task for error logging—Snowflake AI_EXTRACT " class="wp-image-34101 size-full" height="258" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-28.png" width="427" /><p class="wp-caption-text" id="caption-attachment-34101">Figure 28: Setting up an hourly Snowflake Task for error logging—Snowflake AI_EXTRACT</p></div>
<p>For even more powerful error handling inside stored procedures, wrap Snowflake AI_EXTRACT in a TRY/CATCH block using Snowflake Scripting.</p>
<pre class="codebox">GRANT CREATE PROCEDURE ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

CREATE OR REPLACE PROCEDURE doc_processing_db.extraction.safe_extract_document( 

  stage_path VARCHAR, 

  file_path  VARCHAR 

) 

RETURNS VARIANT 

LANGUAGE SQL 

AS 

$$ 

DECLARE 

  result VARIANT; 

  err_msg STRING; 

BEGIN 

  -- call AI_EXTRACT and capture result 

  result := AI_EXTRACT( 

    file =&gt; TO_FILE(stage_path, file_path), 

    responseFormat =&gt; { 

      'vendor': 'Vendor name', 

      'total':  'Total amount due' 

    } 

  ); 

  INSERT INTO doc_processing_db.extraction.extraction_results (document_id, file_path, extracted_at, raw_response, error_message) 

  VALUES (MD5(file_path), file_path, CURRENT_TIMESTAMP(), result, result:error::VARCHAR); 

  RETURN result; 

EXCEPTION 

  WHEN OTHER THEN 

    -- capture SQL error and store a minimal failure row for later investigation 

    err_msg := SQLERRM; 

    INSERT INTO doc_processing_db.extraction.extraction_error_log (logged_at, error_count, sample_files, sample_errors) 

    VALUES (CURRENT_TIMESTAMP(), 1, ARRAY_CONSTRUCT(file_path), ARRAY_CONSTRUCT(err_msg)); 

    -- return a JSON-like VARIANT with the error so callers get a predictable shape 

    RETURN PARSE_JSON('{"error": "' || REPLACE(err_msg, '"', '''') || '", "response": null}'); 

END; 

$$;</pre>
<div class="wp-caption aligncenter" id="attachment_34102" style="width: 725px;"><img alt="safe_extract_document stored procedure with TRY/CATCH error handling wrapping Snowflake AI_EXTRACT " class="wp-image-34102 size-full" height="550" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-29.png" width="715" /><p class="wp-caption-text" id="caption-attachment-34102">Figure 29: safe_extract_document stored procedure with TRY/CATCH error handling wrapping Snowflake AI_EXTRACT</p></div>
<h3>Step 15—Monitoring and alerting</h3>
<p>Finally, with extraction running in production, monitor both Cortex token consumption and warehouse credit spend. Do not rely on a fixed token-per-page magic number. Snowflake reports Cortex usage in <a href="https://docs.snowflake.com/en/sql-reference/account-usage#overview-of-account-usage-schemas" rel="noopener" target="_blank">account-usage views</a>. Use the account usage views below to measure token and credit consumption.</p>
<p><strong>Query Cortex AI SQL usage =&gt; hourly aggregates </strong></p>
<pre class="codebox">SELECT 

  USAGE_TIME, 

  FUNCTION_NAME, 

  MODEL_NAME, 

  SUM(TOKEN_CREDITS) AS token_credits, 

  SUM(TOKENS)        AS tokens_used 

FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_AISQL_USAGE_HISTORY 

WHERE USAGE_TIME &gt;= DATEADD(day, -30, CURRENT_TIMESTAMP()) 

GROUP BY 1,2,3 

ORDER BY USAGE_TIME DESC, token_credits DESC;</pre>
<div class="wp-caption aligncenter" id="attachment_34103" style="width: 1260px;"><img alt="Analyzing token usage history—Snowflake AI_EXTRACT " class="wp-image-34103 size-full" height="281" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-30.png" width="1250" /><p class="wp-caption-text" id="caption-attachment-34103">Figure 30: Analyzing token usage history—Snowflake AI_EXTRACT</p></div>
<p>Use CORTEX_AISQL_USAGE_HISTORY for the most up-to-date AISQL usage view. CORTEX_FUNCTIONS_USAGE_HISTORY is deprecated for some use cases; prefer the AISQL view for SQL-based function calls.</p>
<p><strong>Query daily metering (credits) for warehouses </strong></p>
<pre class="codebox">SELECT 

  USAGE_DATE, 

  SERVICE_TYPE, 

  WAREHOUSE_NAME, 

  SUM(CREDITS_USED) AS credits_used 

FROM SNOWFLAKE.ACCOUNT_USAGE.METERING_DAILY_HISTORY 

WHERE USAGE_DATE &gt;= DATEADD(day, -30, CURRENT_DATE()) 

GROUP BY 1,2,3 

ORDER BY USAGE_DATE DESC;</pre>
<div class="wp-caption aligncenter" id="attachment_34104" style="width: 638px;"><img alt="Monitoring warehouse credit consumption—Snowflake AI_EXTRACT " class="wp-image-34104 size-full" height="779" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-31.png" width="628" /><p class="wp-caption-text" id="caption-attachment-34104">Figure 31: Monitoring warehouse credit consumption—Snowflake AI_EXTRACT</p></div>
<p>Last step: Set up a Snowflake Alert to notify your team when Cortex spending reaches a threshold within an hour. First, make sure you have a notification setup in place, as you&#8217;ll need it to send the alert. Also, verify that the email addresses of the recipients are valid. For more information, check out Notifications in Snowflake. Once that is done, execute the following code:</p>
<pre class="codebox">GRANT CREATE ALERT ON SCHEMA doc_processing_db.extraction TO ROLE doc_extractor_role; 

CREATE OR REPLACE ALERT doc_processing_db.extraction.high_cortex_spend_alert 

  WAREHOUSE = doc_extract_wh 

  SCHEDULE  = '1 HOUR' 

IF ( 

  EXISTS ( 

    SELECT 1 

    FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_AISQL_USAGE_HISTORY 

    WHERE USAGE_TIME &gt;= DATEADD(hour, -1, CURRENT_TIMESTAMP()) 

    GROUP BY FUNCTION_NAME 

    HAVING SUM(TOKEN_CREDITS) &gt; 100   -- choose threshold 

  ) 

) 

THEN 

  CALL SYSTEM$SEND_EMAIL( 

    'cortex_alerts', 

    'pramitx47@gmail.com', 

    'High Cortex AI Spend Detected', 

    'Cortex AI spend exceeded threshold in the last hour. Check CORTEX_AISQL_USAGE_HISTORY' 

  );</pre>
<div class="wp-caption aligncenter" id="attachment_34105" style="width: 549px;"><img alt="Configuring alerts for high credit spend—Snowflake AI_EXTRACT " class="wp-image-34105 size-full" height="372" src="https://www.flexera.com/blog/wp-content/uploads/2026/03/snowflake-ai-extract-32.png" width="539" /><p class="wp-caption-text" id="caption-attachment-34105">Figure 32: Configuring alerts for high credit spend—Snowflake AI_EXTRACT</p></div>
<p>Pair CORTEX_FUNCTIONS_USAGE_HISTORY and METERING_DAILY_HISTORY to get a complete view of Snowflake AI_EXTRACT costs. Note that token charges and warehouse charges are billed separately.</p>
<p>And that&#8217;s it. You should now have a good understanding of how to configure, set up and use Snowflake AI_EXTRACT.</p>
<h2>What are the limitations of Snowflake AI_EXTRACT ?</h2>
<p>Snowflake AI_EXTRACT has certain limitations that you should be aware of when you&#8217;re planning.</p>
<h3>1) Text and file parameters are mutually exclusive</h3>
<p>You can&#8217;t pass text =&gt; and file =&gt; in the same call. If your workflow sometimes receives text and file references, route them through separate query paths.</p>
<h3>2) Document size limits</h3>
<p>Documents longer than 125 pages will fail. The document limit is 125 pages. Also, files cannot be larger than 100 MB. Another thing: client-side encrypted stages are not supported here.</p>
<h3>3) Question limits</h3>
<p>The maximum output length for entity extraction is 512 tokens per question. For table extraction, the model returns a maximum of 4,096 tokens. Per call, you can ask up to 100 entity extraction questions. Table extraction counts as 10 entity questions regardless of actual column count.</p>
<h3>4) Output token truncation</h3>
<p>Big tables can cause problems. If a table exceeds 4,096 tokens, the output will get cut off. The JSON will just stop without warning. To fix this, you can break the table into smaller groups by column or process the data a page at a time.</p>
<h3>5) No confidence scores</h3>
<p>Snowflake Document AI gave you confidence scores to help flag extractions that were not accurate for human review. Snowflake AI_EXTRACT does not do this. You will need to create your own way to check the results, such as looking for missing values, running test queries, or using AI_FILTER to make sure the extracted values are correct.</p>
<h3>6) JSON schema type support</h3>
<p>The model is quite particular about the JSON schema it accepts. For entity questions, it only supports string types. In the JSON schema, you cannot specify numeric or boolean types &#8211; all values will be returned as strings. Make sure to cast them correctly in your subsequent SQL queries.</p>
<h3>7) Regional availability</h3>
<p>Snowflake AI_EXTRACT is natively available in select regions. For other regions, cross-region inference is required and must be explicitly enabled by an ACCOUNTADMIN. Cross-region inference is not available in U.S. SnowGov regions.</p>
<h3>8) Client-side encrypted stages not supported</h3>
<p>Server-side encryption (SNOWFLAKE_SSE or cloud provider SSE) works fine. Client-side encrypted stages are not supported.</p>
<h3>9) Custom network policies</h3>
<p>Custom network policies are not currently supported for Snowflake Cortex AI functions.</p>
<h3>10) No fine-tuning</h3>
<p>Zero-shot extraction is good enough for most documents. But documents with very specific or unique formats might not work as well with this method. If that is the case, you won&#8217;t be able to train the model using your own examples. On a positive note, legacy Snowflake Document AI models that have been fine-tuned can still be used with the Snowflake AI_EXTRACT(model =&gt; &#8230;, file =&gt; &#8230;) legacy syntax after you migrate. One thing to note, though, is that you won&#8217;t be able to create any new custom models from scratch.</p>
<p>Even with limitations, Snowflake AI_EXTRACT can handle a lot of common documents without issues. The trick is to create prompts and schemas that avoid super long answers and huge tables all at once.</p>
<h2>What are the best practices for using Snowflake AI_EXTRACT effectively?</h2>
<p>To make the most of Snowflake AI_EXTRACT, here are some best practices to keep in mind:</p>
<h3>1) Keep prompts specific and concise</h3>
<p>Keep your questions short and sweet. Vague questions usually get vague answers. The clearer you ask, the more exact the info you get.</p>
<h3>2) Use the description field to localize</h3>
<p>In a document with multiple tables or sections, the description field in JSON schema is how you point the model to the right one. To avoid confusion, provide some context for the model by describing the table&#8217;s title, location, or other details. That way, it knows exactly what you are referring to.</p>
<h3>3) Use AI_COUNT_TOKENS before large batch jobs</h3>
<p>Run a sample before committing. This takes 30 seconds and can save you from an unexpected credit bill. Use <a href="https://docs.snowflake.com/en/sql-reference/functions/ai_count_tokens" rel="noopener" target="_blank">AI_COUNT_TOKENS</a> to estimate the cost of a sample before committing a full run.</p>
<h3>4) Materialize results—do not reprocess</h3>
<p>Every Snowflake AI_EXTRACT call costs tokens. If you have already extracted data from a document, store the result in a table and query the table. Do not call Snowflake AI_EXTRACT on the same document twice. Use the Snowflake Stream and Snowflake Task pattern to process each document exactly once.</p>
<h3>5) Split large tables by column group</h3>
<p>When a table has a lot of rows, it might get cut off due to the 4,096-token output limit. To avoid this, try extracting just 3-4 columns at a time, rather than the whole table, and then combine the results based on the row index.</p>
<h3>6) Break large documents into chunks before extraction</h3>
<p>If you are near the 125-page limit, splitting your documents into smaller chunks is often a clever idea. Break them down by chapter, section, or topic and process each one separately. This usually gives you more accurate results than processing a large document all at once.</p>
<h3>7) Use MEDIUM or smaller warehouses</h3>
<p>Larger warehouses do not speed up AI_EXTRACT performance. That is because Cortex functions run on Snowflake&#8217;s serverless infrastructure, not your warehouse&#8217;s compute nodes. Your warehouse is mainly used for query orchestration and data movement. A MEDIUM-sized warehouse is plenty big enough.</p>
<h3>8) Use column_ordering in table extraction schemas</h3>
<p>In your JSON schema, the column_ordering field determines the order of columns in the extracted output. Without this field, the column order can be all over the place when looking at tables across different documents. Always include column_ordering when loading table data into a Snowflake table with predefined column positions.</p>
<h3>9) Process documents directly from stages</h3>
<p>Do not load file content into VARCHAR columns and pass it as text when you have documents &#8211; use the file parameter instead. It is more efficient and handles binary formats like PDFs correctly.</p>
<h3>10) Enable directory tables before batch queries</h3>
<p>If you forget to enable DIRECTORY = (ENABLE = TRUE) on your Snowflake Stage at creation time, you can alter the stage later. Just be sure to manually refresh the stage before querying the directory table.</p>
<h3>11) Use role-based access to scope Cortex function usage</h3>
<p>Grant SNOWFLAKE.CORTEX_USER only to the roles that need it for running Snowflake Cortex AI functions. Out of the box, the CORTEX_USER role is available to the PUBLIC role, so all users in your account can use Snowflake Cortex AI functions. But in enterprise environments, chances are you will want to limit access.</p>
<h3>12) Combine with AI_PARSE_DOCUMENT for complex layouts</h3>
<p>Documents with unusual formatting, such as multiple columns, mixed text and images, or dense tables, can be tricky. But running Snowflake AI_PARSE_DOCUMENT in LAYOUT mode first can improve AI_EXTRACT accuracy because the model gets a cleaner, Markdown-structured version of the document to work from.</p>
<p>And that&#8217;s a wrap!</p>
<h2>Conclusion</h2>
<p>Snowflake AI_EXTRACT is a powerful function that makes analyzing documents in Snowflake much easier. You can extract key information, lists and tables from documents without needing external tools or manual parsing. It is a one-step query that replaces the older Snowflake Document AI workflow and scales with your existing data pipelines. To get started, follow the setup steps above. Be very cautious that there are some limitations, such as page count and token length, and always keep track of your costs. The benefit is that you can now access all the data hidden in your reports, contracts and more, all within Snowflake.</p>
<p>In this article, we have covered:</p>
<ul>
<li>What is Snowflake Cortex AI?</li>
<li>What is the Snowflake AI_EXTRACT function?</li>
<li>Syntax and argument breakdown of the Snowflake AI_EXTRACT function</li>
<li>Snowflake AI_EXTRACT vs Snowflake Document AI</li>
<li>Snowflake AI_EXTRACT vs Snowflake AI_PARSE_DOCUMENT</li>
<li>What can you use Snowflake AI-EXTRACT for?</li>
<li>How to calculate Snowflake AI_EXTRACT costs</li>
<li>Step-by-step guide to configure and use Snowflake AI_EXTRACT to extract data from documents</li>
<li>Limitations of Snowflake AI_EXTRACT</li>
<li>Best practices for using Snowflake AI_EXTRACT effectively</li>
</ul>
<p>… and so much more!</p>
<p>&nbsp;</p>
<p style="text-align: center;"><a class="btn" href="https://www.flexera.com/about-us/contact-us-b" rel="noopener" target="_blank">Want to learn more? Reach out for a chat</a></p>
<p>&nbsp;</p>
<h2>FAQs</h2>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>What is Snowflake AI_EXTRACT and how does it work?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake AI_EXTRACT is a Snowflake Cortex AI function (formerly Cortex AISQL) that extracts structured data from text or documents using large language models. You can easily call it with a text string or a file reference (TO_FILE), plus a responseFormat describing what to extract (questions or JSON schema). The function returns a JSON object with the requested values.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>When was Snowflake AI_EXTRACT released and when did it reach GA?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake AI_EXTRACT launched in preview in August 2025. It became generally available October 2025.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>How is Snowflake AI_EXTRACT different from Snowflake Document AI?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake Document AI was Snowflake’s older document extraction feature (based on Snowflake Arctic-TILT models) that used a Snowsight UI to build models. AI_EXTRACT is the new approach (using Snowflake Arctic-Extract models) that replaces it. Key differences: Snowflake AI_EXTRACT is a one-step SQL function (no UI model build), uses token-based billing instead of compute time, and supports more flexible output (tables, arrays) in one call. Snowflake has announced that the Document AI and PREDICT method will be decommissioned, urging users to migrate to Snowflake AI_EXTRACT.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>How does Snowflake AI_EXTRACT differ from Snowflake AI_PARSE_DOCUMENT?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake AI_PARSE_DOCUMENT is for converting a document to raw text (OCR and layout). It returns the full text of a document (as JSON pages) and is best for indexing or feeding into search/RAG pipelines. AI_EXTRACT, by contrast, answers specific questions or fields: you define which pieces to pull out, and it returns just those in a structured format. In other words, use AI_PARSE_DOCUMENT when you need the entire text/content of a doc; use AI_EXTRACT when you need only certain data points (like invoice total, dates, etc.).</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>What file types and languages do Snowflake AI_EXTRACT support?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Snowflake AI_EXTRACT can handle a wide range of file types like PDF, PNG, PPTX, PPT, EML, DOC, DOCX, JPEG, JPG, HTM, HTML, TEXT, TXT, TIF, TIFF, BMP, GIF, WEBP, MD. Maximum file size is 100 MB; maximum document length is 125 pages. As for languages, Snowflake AI_EXTRACT supports 27 languages: Arabic, Bengali, Burmese, Cebuano, Chinese, Czech, Dutch, English, French, German, Hebrew, Hindi, Indonesian, Italian, Japanese, Khmer, Korean, Lao, Malay, Persian, Polish, Portuguese, Russian, Spanish, Tagalog, Thai, Turkish, Urdu and Vietnamese.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>What Snowflake roles and privileges are required to run Snowflake AI_EXTRACT?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>To get started, users need the SNOWFLAKE.CORTEX_USER database role. This role is normally granted to PUBLIC by default, so everyone in an account has it. But in controlled environments, it&#8217;s better to grant it to specific roles instead and take it away from PUBLIC.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>How do I estimate the cost of a Snowflake AI_EXTRACT job? (tokens, pages, credits)</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Input tokens = (number of pages × 970) + responseFormat tokens. Output tokens = sum of extracted field lengths (max 512 per entity field, max 4,096 for table extraction). Use AI_COUNT_TOKENS to measure your responseFormat token count before running. Check the Snowflake Service Consumption Table for current credit rates per token.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>What are the token and output limits for Snowflake AI_EXTRACT?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<ul>
<li>Maximum 100 entity questions per call</li>
<li>Maximum 10 table questions per call (each counts as 10 entity questions)</li>
<li>Maximum 512 output tokens per entity question</li>
<li>Maximum 4,096 output tokens for table extraction total</li>
</ul>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can I use client-side encrypted stages with Snowflake AI_EXTRACT?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>No. Client-side encrypted stages are not supported. Use server-side encryption (SNOWFLAKE_SSE or cloud provider managed keys) instead.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can Snowflake AI_EXTRACT handle handwritten text?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Yes. Snowflake AI_EXTRACT uses a vision-based model that processes the document visually, not just as parsed text. It can read handwritten text, filled checkboxes and signatures, though accuracy on poor handwriting will naturally be lower than on printed text.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>How secure is my data when using Snowflake Cortex AI functions?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>All Snowflake Cortex AI functions run inside Snowflake&#8217;s environment. Your data doesn&#8217;t leave the Snowflake security perimeter to third-party services. RBAC, data governance policies and access controls all apply normally. If you use cross-region inference, data is routed to a different Snowflake region for inference but stays within the Snowflake ecosystem.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can I extract data from scanned documents or images?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Yes. Snowflake AI_EXTRACT&#8217;s vision model processes images and scanned PDFs without requiring a separate OCR step. It processes the document visually, so it handles image-based PDFs and standalone image files (PNG, JPEG, TIFF, BMP, WEBP, GIF) natively.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can I fine-tune the Snowflake AI_EXTRACT model for my specific documents?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>No. Snowflake AI_EXTRACT is zero-shot only. You can&#8217;t train it on your specific documents. If you have legacy Snowflake Document AI fine-tuned models migrated to the Snowflake Model Registry, you can still call them via the AI_EXTRACT(model =&gt; &#8216;&#8230;&#8217;, file =&gt; &#8230;) legacy syntax, but you can&#8217;t create new fine-tuned models.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can I export Snowflake AI_EXTRACT results to external systems?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Yes. Snowflake AI_EXTRACT results are stored in standard Snowflake tables as VARIANT (JSON) columns. From there, you can use Snowflake&#8217;s standard data sharing, external table mechanisms, Kafka connectors, or any ETL tool that reads from Snowflake to push data to downstream systems.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>Can AI_EXTRACT process documents in multiple languages?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>Yes. Snowflake AI_EXTRACT handles 29 languages, including multilingual documents. If a document has multiple languages, the model will attempt to extract all texts it recognizes.</p>
</div>
</div>
</div>
<p>&nbsp;</p>
<div class="accordion-item">
<div class="accordion-header">
<h3>What happens if my document exceeds limits (too many pages or output tokens)?</h3>
</div>
<div class="accordion-body-wrapper">
<div class="accordion-body">
<p>If the document exceeds limits, the query will error out or truncate the result. To handle it, break the document into parts (like separate PDF for each section) or narrow the extraction (fewer questions at a time).</p>
</div>
</div>
</div>
