# Module: Build, Train, and Serve Your Own LLM

Part of the FDE Automation Skill. The rules in SKILL.md and references/common-standards.md always apply on top of this file.

Covers: an AI model product of the agency's own brand, like a private "ChatGPT, Claude, or Gemini" style assistant: choosing a base model, training it on your data (fine-tuning), running it on your own servers, offering it to customers through an API and MCP, and connecting it with the Claude API (or another hosted model) where that gives better results.
Roles: International AI Expert (lead), ML Training Expert, MLOps and Inference Expert, Cloud Automation Expert, Security Expert, QA Expert, Licence and Attribution Expert. Add a Language Expert for each approved language (SKILL.md, Section 7.5).
Always Full Mode, except Path A below, which may use Express Mode for a demo.
If the project also needs a full chat app with logins and screens for end users, read references/apps.md for the app part. If customers will use the model on WhatsApp or other channels, read references/social-media.md.

## Honest Starting Point (Explain This to the Owner in Urdu First)
1. Building a large model like ChatGPT, Claude, or Gemini **from zero** needs thousands of GPUs, huge data, a large team, and usually tens of millions of dollars or more. Claude does not recommend this for an agency, and says so plainly.
2. What agencies and most companies actually do is one of the paths in Step A: build on an existing model, adapt it with their own data, and put their own brand, rules, and tools around it. This is how most "own AI" products are made.
3. "Own model, no copyright problem" means: your own brand name, your own interface, and your own training data, while respecting the licence of the base model you start from. A fine-tuned model is still based on someone else's model. Claude never describes it as "built from scratch" unless it truly was.

## Extra Intake Questions
1. Goal (clickable): customer support assistant, internal company assistant, Urdu-language assistant, coding assistant, assistant for one industry (for example banking, textiles, health), other.
2. Who uses it: your agency only, one client, or many paying customers (a product)?
3. Must data stay private, for example never leave the client's servers or country? (yes, no, not sure)
4. Which languages must it handle well: Urdu script, Roman Urdu, English, others?
5. Do you have your own training data (documents, past chats, FAQs)? Who owns it, and do you have the right to use it for training?
6. Expected users and requests per day, and the speed you need.
7. Budget range per month for GPUs, hosting, and hosted-model (API) usage.
8. Will customers connect it to their own tools (MCP), or call it from their own software (API)?

## Step A: Choose the Path (Phase 1 to 3)
Claude presents these as clickable options with cost, time, quality, privacy, and difficulty, then recommends one with an Urdu reason:
- **Path A — Branded assistant on a hosted model:** your brand, prompts, knowledge (RAG), and tools on top of the Claude API or another hosted model. Fastest, best quality, lowest setup cost. Data goes to the provider (under its terms).
- **Path B — Open-weight model, self-hosted:** run an open-weight model on your own or rented GPUs. More privacy and control, more work and running cost. Quality depends on model size.
- **Path C — Open-weight model, fine-tuned:** Path B plus training on your own data so it speaks in your style, knows your domain, or follows your format. Needs good data and evaluation.
- **Path D — Hybrid with a router (often the best fit):** your own model handles simple, private, or high-volume requests, and the Claude API handles hard requests. A router decides per request. Best balance of cost, privacy, and quality.
- **Path E — Small model trained from zero:** only for research, learning, or a very narrow task with a tiny model. Not a ChatGPT replacement. Claude explains why before the owner chooses it.

Record the chosen path in `docs/DECISIONS.md`. The rest of this file applies according to the path: Path A skips Steps C and D. Paths B to E use all steps.

## Step B: Licences and Terms (Phase 4, Before Any Download or Training)
1. **Base model licence:** read the licence of every open-weight model considered. Explain in Urdu: is commercial use allowed, are there user-count or revenue limits, must the model name or credit appear, are some uses forbidden, can the fine-tuned model be sold or shared. "Open weights" does not always mean "free for any use". Claude says "میں یقین سے نہیں کہہ سکتا" about legal effects, and a lawyer confirms for a paid product.
2. **Hosted model terms (Claude API and others):** read the provider's current commercial terms and usage policy. Providers commonly restrict using their models' outputs to train a competing model, and restrict some uses. Claude does not use Claude or any other provider's outputs as training data unless the owner has checked that the terms allow it. Claude says "میں یقین سے نہیں کہہ سکتا" until the owner confirms.
3. **Training data rights:** use only data the owner or client has the right to use for training. Do not scrape websites, books, or other people's content without permission. Record each data source and its permission in `docs/DATA_SOURCES.md`.
4. **Brand and name:** choose an original product name and logo. Do not use names, logos, or look-alike designs of ChatGPT, Claude, Gemini, or other products. Check that the name is not already a registered trademark in the target countries (the owner confirms with a lawyer or trademark search).
5. **Customer terms:** the product needs its own terms of service, acceptable use policy, and privacy policy. Claude drafts a checklist. A lawyer writes or reviews the final text.
6. **Regulation:** if the model or product is offered in the EU, check the EU AI Act duties for general-purpose AI models and for AI systems. Other regions may have their own rules. Claude lists the questions and the owner confirms with a lawyer.

## Step C: Data (Phase 2 and 9, Paths C and E)
1. **Collect:** list the data needed for the goal (examples of good answers, domain documents, past conversations with permission).
2. **Clean:** remove personal data (names, phone numbers, CNIC, account numbers) or replace it with placeholders, remove duplicates, remove low-quality and harmful content, and fix encoding. Urdu text must be checked for correct script and Unicode.
3. **Format:** convert to the format the training method needs (for example question and answer pairs, or chat turns). Keep a small, high-quality set rather than a large, noisy one.
4. **Split:** training, validation, and test sets, with no overlap. The test set is never used for training.
5. **Document:** write a dataset card in `docs/DATA_SOURCES.md`: sources, permissions, size, languages, cleaning steps, known gaps, and date.
6. **Store securely:** training data has the same protection as the client's original data. Access is limited and logged.

## Step D: Training (Phase 10, Paths C and E)
Only after the Coding Gate (SKILL.md, Section 2.4) and approval of Steps B and C.
1. **Method:** for most projects, use parameter-efficient fine-tuning (for example LoRA or QLoRA), which trains a small add-on instead of the whole model and needs much less GPU. Full fine-tuning only when there is a measured need. If the model must learn preferences (which answer is better), consider a preference method (for example DPO) after supervised fine-tuning. Claude explains each in Urdu and checks current best practice and library versions before choosing.
2. **Tools:** standard, well-maintained open-source libraries for training (for example the Hugging Face ecosystem). Claude names exact libraries and versions only after checking their current documentation.
3. **Compute plan:** estimate GPU type, number, hours, and cost before starting. Show 2 or 3 options (for example rented cloud GPUs by the hour, a GPU cloud service, or the client's own hardware). The owner approves the budget. Set a hard spending limit and stop training automatically when it is reached.
4. **Reproducibility:** pin library versions, set random seeds, save the exact data version, the training settings (hyperparameters), and the base model version for every run.
5. **Experiment tracking:** log every training run (settings, loss curves, evaluation scores, cost) in an experiment tracker, and record the chosen run in `PROGRESS.md`.
6. **Checkpoints:** save checkpoints during training so a crash does not lose everything. Keep only the useful ones to control storage cost.
7. **Safety tuning:** include examples that teach the model to refuse harmful requests, admit when it does not know, and hand over to a human where the product needs it.
8. **Model files are valuable:** store trained weights privately, with access control and backups. Never publish weights unless the owner approves and the licences allow it.

## Step E: Evaluation (Phase 12 and 13, All Paths)
Apply common-standards.md, Section 12.6, plus:
1. **Compare fairly:** test the base model, the fine-tuned model, and the hosted model (for example Claude) on the same golden set. Fine-tuning is kept only if it is clearly better for the goal, or equal in quality at a lower cost or with better privacy.
2. **Domain and language tests:** domain questions, Urdu script, Roman Urdu, English, and mixed-language questions.
3. **Regression on general skills:** check that fine-tuning did not damage general ability (simple reasoning, following instructions, safe refusals).
4. **Safety and red teaming:** harmful requests, jailbreak attempts, prompt injection, personal-data leaks, and made-up facts (hallucination rate on questions with known answers). common-standards.md, Section 12.6 item 6 applies.
5. **Speed and cost:** measure time to first word, total response time (median and slowest 5 out of 100), tokens per second, and cost per 1,000 requests.
6. **Model card:** write `docs/MODEL_CARD.md`: base model and licence, training data summary, intended use, uses that are not allowed, evaluation results, known limits, and version.

## Step F: Serving the Model (Phase 10 and 11, Paths B to E)
1. **Inference server:** use a proven open-source inference server built for high throughput, or a managed hosting service. Claude compares 2 or 3 current options and checks their documentation before choosing. For small local demos, a lightweight local runner may be enough.
2. **Quantization (making the model smaller to run on less GPU):** test quantized versions. Use one only if quality on the golden set stays acceptable.
3. **Sizing:** choose GPU type and number from the measured load and speed targets. Plan autoscaling (more servers at busy times, fewer at quiet times) and a minimum to keep it responsive.
4. **Streaming responses** so users see words as they are generated.
5. **Reliability:** health checks, automatic restart, request timeouts, a queue for busy periods, and a fallback (for example to the hosted model) when your servers are down or overloaded.
6. **Monitoring:** GPU use, memory, request rate, errors, speed, and cost. Alerts to the owner (common-standards.md, Section 12.5).

## Step G: Your Own API (Phase 6 and 10)
1. **Compatible format:** offer a chat API in a widely used request format (many tools accept the OpenAI-compatible chat format), so customers can connect easily. Document it in OpenAPI (common-standards.md, Section 11.1).
2. **Versioning:** `/v1/` routes, and the model version in every response.
3. **Features:** streaming, system instructions, tool calling, and structured (JSON) output, if the model supports them reliably. Test each one.
4. **Access control:** an API key per customer, stored hashed, that can be revoked. Separate test and live keys.
5. **Limits and billing:** rate limits and monthly quotas per customer. Count tokens per customer for billing and for abuse detection.
6. **Customer data:** state clearly whether prompts are logged, for how long, and whether they are ever used for training. The default is: not used for training without written customer consent.
7. **Documentation:** a short developer guide with examples in Python and JavaScript, error codes, and limits.

## Step H: MCP and Tools (Phase 6 and 10)
1. **MCP (Model Context Protocol)** is a standard way to connect an AI assistant to tools and data. Two uses:
   - **Your assistant as an MCP client:** it connects to approved MCP servers (for example a CRM, a database, files) to take actions or read data.
   - **Your service as an MCP server:** customers' AI tools can connect to your model or your data through MCP.
2. Claude checks the current MCP specification and SDK versions before building, and says "میں یقین سے نہیں کہہ سکتا" about any detail it has not verified.
3. **Security:** every MCP server is checked before use (a bad server is a supply-chain risk). Use least access, authentication, an allow-list of tools, and human confirmation before any action that cannot be undone (common-standards.md, Section 12.4).
4. Tool results are untrusted input. A tool result must never be able to change the assistant's rules.

## Step I: Connecting the Claude API (Paths A and D)
1. **Router design:** write the rules for which requests go to your own model and which go to Claude (for example: by difficulty, by language, by whether private data is involved, or when your model is unsure). Test the router on the golden set.
2. **Privacy rule:** requests containing data that must not leave the client's servers are never sent to any hosted API. The router enforces this in code, not only in the prompt.
3. **Keys and cost:** the Claude API key is stored in the secret manager only (SKILL.md, Section 5). Set a monthly spending limit and alerts. Use prompt caching and streaming where they help (common-standards.md, Section 12.8).
4. **Terms:** follow Anthropic's current usage policy and commercial terms, and tell customers in the privacy policy which providers process their data. Sign the provider's data processing agreement where personal data is involved.
5. **Fallback both ways:** if the Claude API is unavailable, fall back to your own model (or a polite message). If your model fails, fall back to Claude where privacy rules allow.
6. **Disclosure:** the product never claims that answers from Claude came from your own model if a customer asks. Keep it honest in the product description.

## Step J: The Chat Product (If End Users Chat With It)
1. The chat interface (login, chat history, file upload, settings) follows references/apps.md.
2. Minimum features to plan: conversations saved per user with a delete option, streaming replies, "stop generating", copy button, feedback buttons (good or bad answer), clear AI disclosure, and a link to the terms and privacy policy.
3. Feedback from users is reviewed and can become new evaluation cases (and training data only with consent).

## Step K: Safety and Abuse Prevention (All Paths)
1. Input and output filters for clearly harmful content, applied in code.
2. Abuse monitoring: unusual volume, repeated jailbreak attempts, and banned uses. Suspend keys that break the acceptable use policy.
3. Never allow content that sexualises minors or helps create weapons, malware, or other serious harm. These are blocked regardless of what a customer asks.
4. Keep logs only as long as the privacy policy says, and protect them like the original data.

## Step L: Delivering to a Customer (Phase 15)
1. **Deployment choice (clickable):** hosted by your agency, deployed in the customer's cloud account, or installed on the customer's own servers. Explain cost, control, and support for each.
2. **What the customer receives:** access (API keys or app login), documentation, model card, data handling summary, and the support terms. If weights are handed over, include the base model licence and its conditions, which pass on to the customer.
3. **Ownership:** state who owns the fine-tuned weights, the training data, the code, and the brand (common-standards.md, Section 13.4).
4. **Service levels:** uptime target, response time for problems, update schedule, and what happens when the base model or provider changes (common-standards.md, Section 12.9).
5. **Retraining plan:** how often the model is retrained or updated, using which new data, and with which evaluation before release.
6. Fresh owner confirmation before every release to a customer.

## Gates Checklist for This Module
- G1: path chosen (A to E), with cost and honesty points explained.
- G4: base model licence, provider terms, data rights, brand name, and customer terms approved.
- G5: compute budget and hosting approved, with a hard spending limit.
- G6: architecture (router, API, MCP, data flow) and threat model approved.
- G9: dataset card approved, then the Coding Gate.
- G10: training run chosen, with tracked results.
- G12: evaluation and red teaming passed, model card written.
- G13: load, speed, failover, and API tests passed.
- Release: fresh owner confirmation for each customer release.
