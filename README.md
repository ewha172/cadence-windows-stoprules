# closebot lead follow up: Setting Cadences, Windows and Stop Rules So Your AI Agent Doesn't Text Leads at 3am

Most people searching for CloseBot lead follow up are past the hard part. The bot replies. It qualifies. Then the lead goes quiet, the bot says nothing, and the conversation dies in an inbox.

That gap is by design, not a bug. Inside CloseBot, follow-ups are a separate system you have to switch on, and each job flow gets its own set of rules. If you never touch the Follow-Ups tab, your agent will not send a single follow-up message. Ever.

Here's how the system actually works, what it costs at volume, and where the settings live that most builds get wrong.

## Where follow-ups live inside CloseBot

Every job flow has its own follow-up settings. Open the job flow, click the **Follow-Ups** side tab, and you get a panel for cadence rules. New job flows start with one cadence labelled *Default*, and it contains no follow-ups at all — which is why so many people finish a build, test it once, and assume the agent is "following up" when it isn't.

A cadence is just a list of intervals. Say your rules are 5 minutes, then 3 hours. The agent sends its message, waits 5 minutes, sends again if nobody replies, waits 3 hours, sends again. The moment the lead replies, the sequence resets and starts counting from the newest message.

Two details worth knowing before you type numbers into that panel:

- **Each interval counts from the previous follow-up**, not from the first outbound message. A 30-minute and 4-hour pair means the second touch lands 4.5 hours after the original reply, not 4.
- **Not every node triggers follow-ups.** The agent node does; a statement node doesn't. The Follow-Ups panel shows a small icon next to node names that support follow-up behavior, so you can verify at a glance instead of guessing why nothing fires.

There's also an optional extra prompt inside the cadence, which lets you change what the agent says at that specific point in the sequence. CloseBot's own docs suggest leaving it blank at first and only editing after you watch how the default performs. That's reasonable advice — tuning copy before you know whether the timing is right is backwards.

One setting deserves caution: **repeating the final follow-up**. Turning it on loops the last message forever at its interval, so a three-week cadence keeps going indefinitely until something changes it. That's a powerful setting sitting one checkbox away from a bad afternoon.

## Cadence switching is where the real control shows up

You can build up to four cadences per job flow, which sounds like a lot until you see how they get used. The common pattern is a default cadence for engaged leads, a slower one for cooling leads, and an empty one for people who should never hear from the bot again.

The agent node can switch between them mid-conversation based on instructions you write, and the switch persists — even after the lead exits that node later. So if a lead starts giving real information, the agent can move them from a quiet cadence into a warm one. If they book, it can drop them to nothing.

In a live build CloseBot published, both agencies involved landed on the same architecture with different numbers: first follow-up at 30 minutes, then a second touch at 4 hours in one account and 12 hours in the other, with an empty "no follow-up" cadence available for anyone who clearly wasn't a fit. Two very different businesses, near-identical structure.

That convergence is probably the most useful signal in the whole system. The exact intervals matter less than the shape.

## Set the clock at the source, not in the prompt

This is the part that trips up nearly everyone building follow-up sequences.

If you want your agent to stop messaging at night, the instinct is to write a time rule into the prompt: "don't send follow-ups after 8pm." That's the wrong layer, and it's fragile — you're asking a language model to do timezone arithmetic on every single message, forever.

Follow-up windows live in **source settings**, under Availability. There's a Follow-Up Restrictions tab for exactly this, and a separate Reply Restrictions tab that also lets you pick which channels the restrictions apply to.

Set the window to 8am–8pm and a 4-hour follow-up that would have fired at 8:40pm gets held until the next morning automatically. The cadence stays simple. The clock stays sane.

> Follow-ups stay on the channel where the conversation is happening, with the exception of smart switching off Live Chat after enough time has passed. That's why follow-up restrictions don't require a channel selector the way reply restrictions do.

While you're in source settings, three other toggles are worth a look: auto shutoff when a human jumps into the conversation, leaving conversations unread after an AI reply so someone actually reviews them, and the graceful goodbye option. None of them affect follow-up timing directly, but the auto shutoff one prevents the classic embarrassment of a human replying and the bot continuing to talk.

## The stop rule, and why an empty cadence is mandatory

Most follow-up setups have one cadence and no opinion. Everybody who goes quiet gets chased with the same enthusiasm — the guy who said "not interested" and the guy who said "send me times for Thursday."

The fix is a second cadence with nothing in it, plus an instruction telling the agent when to switch. CloseBot's published build phrased it as: if this person is a friend, family, not interested, or it would be awkward to follow up, switch cadences. The "awkward" clause is deliberately vague, and it's doing real work. It catches the cases you'd never think to enumerate.

CloseBot also ships **smart follow-ups**, on by default in the agent node. When a lead says something like "I'm at work, I'll get back to you," the agent generally reschedules its own next touch instead of firing at the preset interval. Note the limitation: smart follow-ups don't apply if you're still building with objectives, which is one of the more practical reasons to move a legacy build over.

Pair those two and the sequence starts behaving less like a drip campaign and more like a setter who can read a room.

## Channel limits you can't prompt your way around

Meta's 24-hour messaging window applies on Instagram and Messenger. Once a day passes since the lead's last message, you can't just keep sending. If your cadence runs longer than a day — and it should — it needs somewhere to go.

CloseBot's agent moves the conversation to SMS automatically when a phone number was captured earlier in the chat. That's a meaningful detail for anyone running Instagram DM lead flow, because the alternative is a sequence that quietly expires inside the window while the dashboard shows an active cadence.

The other channel-adjacent thing worth configuring early is who the bot is allowed to answer at all. Prompt-level filtering is a judgment call the model has to get right on every message. Tag-based filters at the source level are deterministic — if every genuine lead arrives carrying a tag from your form or ad, you filter on that tag and the bot never sees your friends, your family, or other agencies pitching you. That's less a follow-up setting than a prerequisite for follow-ups not being embarrassing, but the two live in the same build.

## What follow-up volume actually costs

CloseBot's pricing is unusual in the category: on business plans, message costs are included in the base price rather than metered on top. Agency accounts work the other way, paying a flat per-message rate they rebill to clients.

Here's the full current plan structure from CloseBot's plans page. Note that mid-tier business prices sit behind a slider on the official page, so the per-volume figures below come from third-party pricing trackers and should be checked before you commit to a budget.

| Plan | Price | Billing period | What's included | Purchase |
| --- | --- | --- | --- | --- |
| Free | $0 | Always free | 100 messages/mo, 1 agent, 1 user seat, 1 MB storage, unlimited account connections. Overage billed at $0.08/message | [ Start on the free plan](https://app.closebot.com/a?fpr=li87) |
| Core — Business (500 msgs) | $64/mo | Monthly | Message costs included, 15+ templates, human support, extra seats at $5 each | [ Check the Core business plan](https://app.closebot.com/a?fpr=li87) |
| Core — Business (1,000 msgs) | ~$84/mo | Monthly | Same as above, higher message ceiling | [ Check the Core business plan](https://app.closebot.com/a?fpr=li87) |
| Core — Business (2,000 msgs) | ~$109/mo | Monthly | Same as above, higher message ceiling | [ Check the Core business plan](https://app.closebot.com/a?fpr=li87) |
| Core — Business (5,000 msgs) | ~$176/mo | Monthly | Same as above, higher message ceiling | [ Check the Core business plan](https://app.closebot.com/a?fpr=li87) |
| Core — Business (20,000 msgs) | ~$454/mo | Monthly | Same as above, higher message ceiling | [ Check the Core business plan](https://app.closebot.com/a?fpr=li87) |
| Core — annual | $53/mo, billed as $640/yr | Annual | Two months free versus monthly, plus a larger 50+ template library | [ See annual pricing](https://app.closebot.com/a?fpr=li87) |
| Agency | $397/mo | Monthly | Unlimited agents across unlimited sources, white-label client portal, rebill all costs | [ Check the Agency plan](https://app.closebot.com/a?fpr=li87) |
| Agency — annual | ~$331/mo equivalent | Annual | Same as above on annual billing | [ See Agency annual pricing](https://app.closebot.com/a?fpr=li87) |
| Growth | Custom quote | Custom | HIPAA compliance, quarterly audits, 99.99% priority uptime, priority support, SLA terms | [ Request Growth pricing](https://app.closebot.com/a?fpr=li87) |

A few things the table doesn't tell you:

**The agency per-message rate is inconsistent across CloseBot's own pages.** The plans page currently states a flat $0.012 per message that can be rebilled, while CloseBot's documentation and a pricing comparison blog post both cite $0.006. If your margin model depends on that number, confirm it inside the account before you quote a client.

**Token costs may sit outside the plan.** CloseBot's documentation states that V2 requires your own AI provider API keys and doesn't cover provider costs, while the plans page FAQ says bring-your-own-key isn't allowed for security reasons. Those two statements don't line up. Ask billing directly.

**Overage on business plans runs at a 2x rate drawn from wallet credits**, and the free plan's ceiling is a hard 100 messages before the $0.08 per-message charge kicks in. Follow-up sequences are message-hungry, so this is the number that decides whether free is really free for you.

There are no refunds. There is a 7-day trial of any paid plan, and the free plan stays free as long as you stay under 100 messages a month. Everything is month to month.

## How long should the sequence run?

CloseBot published a benchmark covering more than 1.1 million appointments booked by its agents, and the follow-up findings are the most transferable part of it.

- Just over half of bookings land outside 9-to-5 in the lead's local time. About 11% arrive between midnight and 6am.
- The booking-weighted average runs roughly **132 messages per booking**, counting inbound messages and leads who never reply at all.
- One lead booked **355 days after first contact**, off an automated sequence that never stopped.

That last number is the one worth taping to your monitor. Nearly every default follow-up window an agency configures runs 30 days and expires long before the tail converts. If your sequence has a hard stop at day 30, you're not measuring the long tail — you're cutting it off.

The same dataset shows Sunday produces the fewest messages per booking by a wide margin while Friday needs about 29% more, and weekends overall close with 14% fewer messages than weekdays. Lower volume, higher intent. That's an argument for keeping weekend coverage on, and for throttling Thursday and Friday if you need to cut spend.

## The checks that catch most broken follow-up setups

If you already have a CloseBot agent live, run these in order of how much they usually cost you:

1. **Do you have a no-follow-up cadence?** If every lead gets the same cadence, uninterested leads keep getting chased.
2. **Are your follow-up windows set at the source?** If not, your bot can text someone at 3am and nobody will notice until a lead complains.
3. **Do your cadence intervals account for the reset?** Each gap counts from the previous follow-up, not the first message.
4. **Is something switching cadences, or is the agent stuck in default?** Cadence switching has to be instructed inside the agent node.
5. **Do you have anywhere for a sequence longer than 24 hours to go?** On Instagram and Messenger, that means SMS.

## Who this fits, and who it doesn't

CloseBot is CRM-native, not channel-native. It answers the text conversations flowing through GoHighLevel, HubSpot, LeadConnector, or a custom CRM. If your entire pipeline is Instagram DMs and you don't run a CRM, you'd be buying two products to run one agent.

The setup effort is real, too. One third-party evaluation put initial configuration at 5 to 10 hours to build knowledge bases and connect things properly. The payback is control — four cadences, source-level time windows, per-flow stop rules — that you simply don't get from a one-shot auto-responder.

For agencies reselling this to clients, the math is different. Rebillable message costs and a white-label portal turn the subscription into a revenue line rather than an expense, which is the whole reason the agency plan exists at $397 a month.

For everyone else, the honest summary is this: the reply gets you the conversation, and the follow-up gets you the booking. CloseBot's own benchmark data says the follow-up is worth more than the reply, and it's the half most people never turn on.

## FAQ

**Does CloseBot follow up automatically by default?**
No. New job flows ship with a single cadence labelled Default that contains no follow-ups. Nothing gets sent until you add intervals to that cadence.

**How many follow-up cadences can one job flow have?**
Up to four. The agent node can switch between them mid-conversation based on your instructions, and the switch persists after the lead leaves that node.

**How do I stop CloseBot from following up at night?**
Set Follow-Up Restrictions in the source settings under Availability. Messages that fall outside the allowed window are held until the next permitted time, so your cadence stays simple and you never rely on the model to handle timezones.

**What happens if a lead says "I'll get back to you"?**
Smart follow-ups are on by default in the agent node and will generally reschedule the next touch rather than firing at the preset interval. This doesn't apply to older builds still using objectives.

**Can a CloseBot follow-up sequence run longer than a day on Instagram?**
Only with a destination. Meta's 24-hour messaging window limits what you can send on Instagram and Messenger, so CloseBot moves the conversation to SMS automatically when a phone number was captured.

**What does follow-up cost on the free plan?**
The free plan includes 100 messages a month. Beyond that you pay $0.08 per message as you go, which gets expensive quickly once you're running multi-touch cadences.

**Is there a free trial?**
Yes — the free plan is free indefinitely under 100 messages a month, and any paid plan comes with a 7-day trial before billing starts. CloseBot states plainly that there are no refunds, so the trial is where you test.
