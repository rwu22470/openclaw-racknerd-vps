# RackNerd OpenClaw VPS: Which Plan Do You Need, What Does It Cost, and Is 8 GB Really the Minimum? (Full Plan Table, Setup Steps and Safety Checklist)

A one-click OpenClaw install on RackNerd asks for 4+ vCPU and 8+ GB RAM, and only one plan in the current specials lineup clears that bar: the 8 GB KVM VPS at $119.99 per year. Install OpenClaw yourself over SSH, skip browser automation, and the 4 GB plan at $59.99 per year will run the agent. Both prices renew at the same rate for as long as you keep the server, so the number you see at checkout is the number you budget for in year two and year three.

The rest of this page covers the five plans on the Specials page, where the 8 GB figure comes from and when you can ignore it, both install paths step by step, the security work that decides whether this is a good idea, and the real annual cost once model API usage is added in.

> **Quick pick:** if you want the one-click route through ctrl, buy the 8 GB KVM VPS at $119.99/year. 👉 [Get the 8 GB plan and install OpenClaw from the panel](https://bit.ly/RacKnerd)
>
> Running it manually over SSH with no browser tasks? The 4 GB plan at $59.99/year is the cheaper fit.

## The full plan lineup, and the one that fits OpenClaw

RackNerd's Specials page currently lists five KVM VPS plans. All are annual, all include full root access, one IPv4 address, a 1 Gbps port, the ctrl control panel and a choice of deployment locations. They are unmanaged: RackNerd supports the platform, not your OpenClaw configuration.

| Plan | vCPU · RAM | SSD | Transfer | Price (yearly) | Order |
| --- | --- | --- | --- | --- | --- |
| 1 GB KVM VPS | 1 · 1 GB | 20 GB | 3 TB | $21.99/year | [ Order the 1 GB plan](https://bit.ly/RacKnerd) |
| 2 GB KVM VPS *(most popular)* | 2 · 2 GB | 35 GB | 5 TB | $35.99/year | [ Order the 2 GB plan](https://bit.ly/RacKnerd) |
| 4 GB KVM VPS | 3 · 4 GB | 60 GB | 7 TB | $59.99/year | [ Order the 4 GB plan](https://bit.ly/RacKnerd) |
| 6 GB KVM VPS | 6 · 6 GB | 100 GB | 12 TB | $89.99/year | [ Order the 6 GB plan](https://bit.ly/RacKnerd) |
| 8 GB KVM VPS | 7 · 8 GB | 150 GB | 20 TB | $119.99/year | [ Order the 8 GB plan](https://bit.ly/RacKnerd) |

The 6 GB plan is the awkward one for this particular job. It has six cores and 12 TB of transfer, more headroom than most people need, yet 6 GB of RAM sits under the 8 GB floor for the ctrl one-click installer. Buying it means paying $30 less than the 8 GB plan and giving up the one-click path entirely. If you're going to install over SSH anyway, the 4 GB plan gives you the same manual path for half the price.

Operating system choice at checkout covers CentOS, Rocky Linux, AlmaLinux, Fedora, Debian and Ubuntu. A custom ISO needs a support ticket. The ctrl panel can reinstall the OS later, and it also has an HTML5 VNC console for recovery work.

## How much RAM does OpenClaw actually need?

RackNerd's AI VPS page lists OpenClaw at 4+ vCPU and 8+ GB RAM, and notes that browser-based tasks consume more memory than that baseline suggests. OpenClaw's own Raspberry Pi installation guide lists 2 GB of RAM as the minimum with 4 GB recommended, which says the agent core is lighter than the one-click deployment requirement implies. A third-party walkthrough published in January 2026 went further and installed OpenClaw from the command line on a 2-core, 2.5 GB RackNerd VPS in Los Angeles running Debian 13, ending with a working Telegram bot.

None of those numbers contradict each other, because they describe different workloads:

- **One-click install through ctrl:** 8 GB is the stated floor. The installer takes the whole machine, so the plan has to fit the deployment as shipped.
- **Manual install, chat and API calls only:** 2 GB is the project's own minimum, 4 GB the recommended figure, and 2.5 GB has been demonstrated in practice.
- **Browser automation:** 4 GB and up. Each browser instance costs several hundred MB, and that's where small VPS installs fall over.
- **Local models on the same box:** 8 GB is entry level, and CPU inference is slow enough that most people abandon it and go back to a hosted model API.

The rule that follows from this: **one-click means 8 GB; manual SSH install without browser tasks means 4 GB is a sensible starting point, and 2 GB is possible but tight.** OpenClaw ships changes quickly, so confirm the current requirement against the project's own documentation before you order.

If you already know you're going the SSH route, the 4 GB plan is the value pick in this lineup. 👉 [Order the 4 GB KVM VPS at $59.99/year](https://bit.ly/RacKnerd)

## Installing from ctrl: four steps, 15 to 45 minutes, and a full wipe

The one-click path is short because RackNerd does the work. From the official flow:

1. Order a plan from the Specials page with enough RAM for the app you want.
2. Log into ctrl and open One-Click Apps on that server.
3. Select OpenClaw. ctrl reinstalls the operating system and deploys it, which takes roughly 15 to 45 minutes.
4. Collect the SSH root credentials and the application login details from the panel once it finishes.

Two things about that sequence aren't obvious from the button. The installer reinstalls the OS, so a server that has been running for months ends up on a fresh Ubuntu installation. And everything on the disk goes with it: existing OS, applications, files, configuration.

> Back up anything you care about before starting a one-click install. The process wipes the VPS, and there is no undo.

That makes ctrl the right choice for a new, empty VPS bought for OpenClaw and the wrong choice for a box already doing other work. If you want both, run the agent on its own server.

## Installing over SSH: what the manual path looks like

The manual route uses OpenClaw's official install script, a curl-to-bash one-liner from the project's own site, run as root or a sudo user on a fresh Debian or Ubuntu install. The January 2026 walkthrough on a 2-core, 2.5 GB machine went like this:

1. Run the official install script and answer **Yes** when it asks whether to continue.
2. Choose **QuickStart** as the install mode.
3. Pick **Qwen** as the model. It's free, which lets you get the agent running before deciding which paid provider you actually want.
4. Authorize the model through an OAuth link the script prints. The walkthrough author opened it in a browser and signed in with a Google account.
5. Confirm you want to keep the current model.
6. Choose **Telegram** as the interaction channel.
7. In Telegram, open the verified BotFather account (the one with the blue check, account ID 93372553), press START, and send `/newbot`.
8. Give the bot a display name, then a username in English letters ending in `bot`.
9. Copy the token BotFather returns and paste it into the terminal. Treat that token as the bot's password.
10. Decline skill configuration for now and let OpenClaw install what it needs on its own.
11. Select all three hooks (arrow keys to move, space to select, Enter to confirm).
12. Press START in your new bot's chat, then run `openclaw pairing approve telegram <code>` on the VPS, using the pairing code from the bot's message rather than the literal placeholder.

That's the whole install. The prompts change between versions, so expect slightly different wording, and read each question instead of copying keystrokes from any guide, including this one.

## Before it goes live: the security work

OpenClaw executes commands, reads and writes files, and reaches the outside world through accounts you've connected. Researchers have found tens of thousands of OpenClaw installations reachable from the public internet, some of them exposing credentials and chat history; early versions trusted local connections and shipped without a password, so a misconfigured proxy could make outside traffic look local. Public skills registries have been found hosting hundreds of malicious entries, including password stealers dressed up as install steps. Prompt injection has no complete fix, and an agent with long-term memory can carry an injected instruction for weeks before it fires.

The minimum set worth doing on day one:

1. **Give the agent its own VPS.** Nothing else important on that server, no saved logins to your other machines.
2. **Run it as a normal user,** not root, and don't add that user to the docker group.
3. **Bind any web panel or gateway to `127.0.0.1`** and reach it through an SSH tunnel or VPN. Use `ss -tulpn` to confirm nothing is listening on a public address.
4. **Restrict the bot to your own chat account ID.** Otherwise anyone who finds it can issue commands.
5. **Create a dedicated API key with a monthly spending cap** for the agent, and rotate it if anything looks off.
6. **Vet every skill before installing it.** Read the code and check the publisher, the same way you'd treat a stranger's script.
7. **Update weekly.** Several serious vulnerabilities have been patched already, including one where a malicious web page could take over a running agent.
8. **Set a minimal firewall.** `sudo ufw default deny incoming`, `sudo ufw allow OpenSSH`, `sudo ufw enable`. An agent that only makes outbound connections doesn't need anything else open.

A separate machine makes all of this easier, and at $59.99 a year it's cheap insurance. 👉 [Give OpenClaw its own RackNerd VPS](https://bit.ly/RacKnerd)

## What a year actually costs

The server is the predictable part. The 8 GB plan runs $119.99 a year, about $10 a month, and the 4 GB plan $59.99, about $5 a month. Over two years that's $239.98 versus $119.98, a $120 gap. RackNerd describes these specials as lifetime recurring and states you can renew at the same rate for as long as you keep the service, so the two-year math is the five-year math.

Model usage is the part people underestimate. An agent loops: one request from you can turn into several model calls as it plans, searches and checks its own output. Set a hard monthly spending limit in your model provider's account before the first task runs, not after the first surprising invoice. The free Qwen option in the installer exists precisely so you can defer that decision.

Time is the third cost. Installation is under an hour either way. Updates, log review and checking what the agent did with your files are ongoing, and skipping them is how the exposed-installation stories happen.

One more thing to know before you click Order: RackNerd's knowledge base states that there is no money-back guarantee and that refunds are not automatic. Pick the plan size you actually need rather than buying two and hoping to return one.

## Who shouldn't buy this

**If you only want to chat with a model,** a hosted chat subscription is cheaper, requires no maintenance and carries none of the exposure risk. A VPS running an agent is for work that has to happen on a schedule, with your accounts and your files.

**If the plan is to run models locally on this box,** 8 GB is the entry point and CPU inference will be slow enough to be frustrating. Local models want GPU memory or a much larger machine.

**If you won't maintain it,** the risk profile changes. An unpatched agent with shell access and a public port is a worse outcome than no agent.

Worth knowing as an alternative: Hermes Agent, from Nous Research, is also MIT licensed, lighter, and connects outward to your chat apps so it needs no open port. It's newer and less tested than OpenClaw. RackNerd lists it as a one-click app with a 2 vCPU / 4 GB minimum, which means the 4 GB plan covers it comfortably.

## OpenClaw on RackNerd: quick answers

**Can the 2 GB plan run OpenClaw?**
Not through the one-click installer. Manually, the project lists 2 GB as its minimum and 4 GB as recommended, and a 2.5 GB machine has been shown working. A 2 GB VPS leaves almost nothing for the OS and no room for browser tasks.

**Why doesn't the 6 GB plan work for one-click?**
The one-click requirement is 4+ vCPU and 8+ GB RAM. The 6 GB plan has six cores, so CPU is fine, but memory falls short and ctrl won't be the install path.

**How long does the one-click install take, and does it delete data?**
Roughly 15 to 45 minutes. It reinstalls the OS and erases everything already on the VPS, so back up first.

**Will the renewal price go up?**
RackNerd states these specials are lifetime recurring and renew at the same rate for as long as you keep the service.

**Do I need a separate model API subscription?**
Yes, unless you run a local model. OpenClaw connects to model providers through an OpenAI-compatible API, and the installer's free Qwen option is a way to start without one.

**Should the ctrl panel or agent gateway be reachable from the internet?**
No. Bind it to `127.0.0.1` and use an SSH tunnel or VPN.

## Pick by install path, then check two things

Want OpenClaw running today with the least fiddling, and want room for browser tasks later: 8 GB at $119.99/year. Comfortable with a terminal, running chat and API calls only: 4 GB at $59.99/year. Planning to use Hermes Agent instead: the same 4 GB plan covers its stated minimum.

Before you order, decide how you'll back up anything already on the server you're replacing, and pick your deployment location and OS at checkout. Both are easier to get right in the order form than to fix afterwards.

👉 [Start with the 8 GB KVM VPS and install OpenClaw from ctrl](https://bit.ly/RacKnerd)
