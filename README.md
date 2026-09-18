# gohighlevel instagram chatbot: Build Instagram DM Auto-Replies Inside GoHighLevel (and Add an AI Setter That Actually Books Calls)

If you typed "gohighlevel instagram chatbot" into Google, you probably want one of three things: DMs answered the second they land, comments turned into conversations, or a bot that qualifies people and drops a call on your calendar without you babysitting the inbox. GoHighLevel can do the first two with its own workflow tools. The third is where people start looking for a brain to bolt on top.

Here's the part most tutorials skate past: GoHighLevel doesn't really "have" an Instagram chatbot. It has an Instagram connection, a comment trigger, and a workflow engine. Whether those three pieces add up to a chatbot depends entirely on what you build on top — and, if you want real conversation, what you plug into it.

## Instagram DMs are plumbing, not a chatbot feature

Instagram messaging doesn't live inside GoHighLevel natively. It arrives because you connected a Facebook Page that has an Instagram professional account attached to it, and Meta passes the messages through to your CRM inbox.

So the foundation is:

- An Instagram **Business or Creator** account, not a personal one.
- That account linked to a Facebook Page you control.
- The Facebook & Instagram integration connected inside your GoHighLevel sub-account (Settings → Integrations → Facebook & Instagram → Connect → Sync Leads).

Once that's done, DMs and Messenger messages land in **Conversations**, tied to a contact record. If someone messages you and later drops an email that matches an existing contact, GoHighLevel merges records — unless **Allow Duplicate Contact** is switched on in your deduplication preferences, in which case you can end up with two profiles for one human.

That's the whole "chatbot" part of the setup. Everything after this is either GHL workflows, an AI layer, or both.

## The 24-hour rule decides your entire flow

Meta's messaging windows are the constraint nobody mentions in the shiny YouTube demos, and getting them wrong is why half of the "why isn't my automation sending?" posts exist.

Two windows matter:

| Situation | Window | What breaks |
| --- | --- | --- |
| Replying to a DM the contact sent you | 24 hours | After 24 hours of silence, a "Reply to DM" action fails to deliver |
| Replying to a comment on your post via DM | 7 days | You must send the comment-to-DM reply within 7 days or delivery fails |

HighLevel's own documentation says the Instagram DM workflow action should only be used to message contacts who have sent a DM in the last 24 hours. Before August 31, 2024 you could DM anyone who commented on your post regardless — that was a bug on Meta's side, and it's been closed. If you build a comment-to-DM funnel assuming the old behavior, it will work in testing and quietly fail in production.

Worth knowing: GoHighLevel recommends the **Instagram Interactive Messenger** action over the older Instagram DM action, because it lets you pick the reply type explicitly — "Reply to comment via DM" for the first message, "Reply to DM" for everything after. You can attach up to three buttons (open a website, call a number, or trigger further workflow actions), and there's a default 1-minute wait with a timeout branch for people who don't tap anything.

## Building the comment-to-DM flow inside GoHighLevel

This is the classic "comment WORD and I'll send it" funnel, and GHL handles it without any third-party tool:

1. **Trigger:** Instagram → User comments on a post. Pick the Page first — every other filter depends on it and disappears if you delete it.
2. **Filter the post:** choose a published post, or paste the URL of a custom one. Then match the comment using **Exact Match** or **Contains Phrase**.
3. **Action:** Instagram Interactive Messenger, reply type = *Reply to comment via DM*.
4. **Follow-ups:** switch to *Reply to DM* for subsequent messages, because the first action opens the thread.
5. **Optional:** a "Reply in comments" action to drop a public reply or auto-like the comment.

Two gotchas: comment matching is fussy (Exact Match on "Price" fails if someone writes "share the price"), and if you want to look human, insert a Wait action before the reply. Also note the trigger fires on submission — if the person deletes the comment a second later, your workflow still runs.

That's a solid lead magnet machine. It is not a salesperson. It replies with the file, maybe asks one question, and stops.

## Where the native bot stops

GoHighLevel's built-in Conversation AI can be connected to Instagram DMs, and it has gotten noticeably better. Community threads on r/gohighlevel still describe it as clunky for real sales conversations, and CloseBot's own pricing write-up claims a roughly 10% quality gap against their agent in split testing. That's a vendor publishing its own comparison, so treat the number as marketing, not measurement. What's verifiable is the architectural difference:

- **GHL Conversation AI** is a general-purpose add-on inside an all-in-one CRM. Pricing for it is either pay-as-you-go at around $0.02 per message (rebillable to clients) or an AI Employee add-on at $97 per sub-account per month for unlimited usage of several AI features.
- **CloseBot** is a single-purpose AI setter. You build an agent with an objective, knowledge, and tools, connect it to a CRM source, and it replies to inbound text conversations in that CRM.

That second point is the honest catch, and most CloseBot reviews bury it: **CloseBot does not connect to Instagram itself.** It connects to GoHighLevel (or HubSpot, LeadConnector, a custom CRM, or a webhook source) and works on the channels that CRM receives. Instagram DMs reach it because Instagram is already flowing into your GoHighLevel Conversations inbox. The comments, keyword triggers, and story replies still have to be built in GHL or a flow builder.

If you don't run a CRM, there's no version of this where CloseBot is a standalone Instagram bot.

👉 [Start with the free CloseBot plan and connect a HighLevel sub-account as a source](https://app.closebot.com/register?fpr=li87)

## The actual setup order

Sequence matters here, and doing it backwards is the main reason people get stuck.

**1. Fix the Instagram side first.** Convert to a professional account, link it to the Facebook Page, and confirm messages appear in GoHighLevel Conversations by sending a DM from a different Instagram account. If the test message doesn't show up, don't touch anything else — check permissions and the Page/IG link.

**2. Build the trigger layer in GHL.** The comment automation from the previous section, plus a "Customer Replied" trigger filtered to Instagram DM if you want the AI involved from the first message.

**3. Add the CRM as a source in CloseBot.** Sources → add source → HighLevel Sub-Account → Connect, which opens an OAuth window. One source per sub-account.

**4. Build the agent.** In CloseBot V2 that means a persona (voice, timing, quirks) plus a job flow built on objectives rather than a wall of prompt text. The free plan gives you one agent and 100 monthly messages, which is enough to test properly.

**5. Filter the channels.** This is the step that saves agencies from embarrassment. In the source filters you can restrict an agent to specific channels and require or block tags. Want the agent to answer Instagram DMs and SMS but stay out of live chat? Set the channel filter. Want the bot to stop the moment a human types? CloseBot can attach an "ai off" tag automatically on manual replies.

**6. Test, then hand off.** CloseBot has an in-flow testing portal, so you can run conversations before anything goes live. Then leave the workflow's AI step pointed at the source and let it answer.

The tags matter more than they look. A tag like `ai off` stops responses, `ready-to-book` can trigger a scheduling link, and `lead-qualified` can hand the thread to a human closer. It's a cheaper mechanism than rebuilding routing logic in workflows.

## What it costs, including the part you keep forgetting

CloseBot publishes its plans openly. Here's the current page as of this writing — Business and Agency are two views of the same **Core** tier, priced differently because they do different jobs.

| Plan | Best for | What you get | Price | Billing | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | Testing, low-volume DMs, one-person operations | 100 messages/mo, 1 agent, 1 user seat, 1 MB knowledge storage, unlimited account connections | $0 | Always free (overage $0.08/message) | [Start on the free plan](https://app.closebot.com/register?fpr=li87) |
| Core (Business) | Businesses running their own pipeline | Message costs included, 500 messages/mo at entry, 15+ templates, human support, seats $5 each, add-on agents and storage | From $64/mo; $53/mo equivalent on annual (billed $640/yr) | Monthly or annual | [Start a 7-day Business trial](https://app.closebot.com/settings?tab=subscription&plan=business&toggle=monthly&fpr=li87) |
| Core (Agency) | Agencies building and rebilling AI setters for clients | Unlimited agents and accounts, white-label client portal, rebill all costs, per-message rebillable rate you mark up yourself | $397/mo; $331/mo equivalent on annual | Monthly or annual | [Start a 7-day Agency trial](https://app.closebot.com/settings?tab=subscription&plan=agency&toggle=monthly&fpr=li87) |
| Growth | High volume, regulated industries | HIPAA compliance, quarterly audits, 99.99% priority uptime, priority support, 50+ templates | Custom quote | Custom | [Compare all plans and request a Growth quote](https://closebot.com/plans/?fpr=li87) |

A few details that matter before you pick:

- **Message ladder.** The Business plans scale with monthly message volume on a slider, from 500 messages up to 100K+. One third-party review recorded pricing of $84 at 1,000 messages, $109 at 2,000, $176 at 5,000, $454 at 20,000, $806 at 50,000 and roughly $1,059 at 100,000 in August 2026 — the official page renders that pricing in JavaScript, so verify your own tier before committing.
- **Overage.** Go over your Business ceiling and you're charged per message at a 2x overage rate, drawn from your wallet.
- **Discrepancy worth flagging.** CloseBot's plans page FAQ says agencies are billed a flat $0.012 per message they can rebill, while the V2 help docs still say $0.006. Both are official sources and they don't agree. Confirm in-app before you build a margin model on either number.
- **AI provider costs.** The V2 documentation states CloseBot V2 requires your own API keys and doesn't cover AI provider costs; the plans page FAQ says the opposite, that bring-your-own-key isn't allowed. Same advice: ask support before budgeting.

Then add the CRM underneath. GoHighLevel runs $97/mo for Agency Starter (3 sub-accounts), $297/mo for Unlimited, and $497/mo for Agency Pro with SaaS mode. A solo operator wanting 1,000 AI messages a month is realistically looking at roughly $84 plus $97 — before any per-message rebilling math.

## Instagram limits that survive every setup

AI doesn't suspend Meta's rules. Things that stay true no matter how good your agent is:

- **24 hours to reply to a DM thread, 7 days to reply to a comment.** Outside those windows, the message fails. Your follow-up cadence has to live inside them or move to SMS and email.
- **Comment triggers need a professional account and a linked Page.** Personal accounts won't flow into Conversations at all.
- **Aggressive DM automation carries account risk.** You'll find Reddit threads from GoHighLevel users reporting Instagram restrictions after running comment-to-DM automations. One thread isn't data, but Meta's enforcement is real, and high-volume blasting is the usual trigger.
- **CloseBot has no Instagram-native triggers.** Comment-to-DM, story replies, and keyword funnels are built in GoHighLevel or a tool like ManyChat. The agent handles the conversation after it starts; it doesn't start it.
- **One message isn't always one billing segment.** Turn on the agent node's expanded tooling and unlimited instruction size and you're billed on tokens instead, which can consume several segments per message. Budget conservatively.

## Who this stack actually suits

**You're a good fit if** you already run GoHighLevel (or HubSpot) and Instagram DMs are one of several channels feeding your inbox. You want one agent handling SMS, DMs and Messenger, with real qualification and booking instead of a keyword tree. Agencies selling AI setup as a service get the added benefit of white-labeled client portals and rebilling, which is the part CloseBot is genuinely built around.

**You're probably not a fit if** your entire pipeline is Instagram DMs and you don't run a CRM. Adding a $97/mo CRM plus a CRM-only AI layer to answer DMs is a lot of scaffolding for something ManyChat-style tools do directly. The architecture, not the product quality, is the mismatch.

The practical test: build the comment-to-DM flow in GHL, connect one sub-account as a CloseBot source on the free plan, and watch what happens when someone asks a question your workflow doesn't have an answer for. That's the moment you find out whether you need an agent or just better branches.
