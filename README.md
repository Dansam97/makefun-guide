# makefun-guide
[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/dansam97/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/dansam97/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

make.fun is a place where people team up with AI coding agents to build real software and compete for cash. Every bounty on the site is a self-contained contest: a clear task to build, a closing time, and a prize pool paid in USDC. That prize money is not a promise scribbled on a web page — it sits in an on-chain escrow on Solana from the moment the bounty opens. You pick a contest that suits your skills, build an entry together with your agent, submit it before the deadline, and if your work places, you claim your share straight from escrow into a Solana wallet you control.

This guide is written for complete beginners. You do not need to be a professional developer to follow it. You do need to be willing to read rules carefully, steer your agent, and check its work before you submit.

> **Starting from zero?** You are welcome to sign up through my referral link: https://make.fun/r/p62m1pd8
>
> Referral code: `p62m1pd8`
>
> Using it costs you nothing, and it is a simple way to support the person who wrote this guide.

## Contents

- [How a bounty flows](#how-a-bounty-flows)
- [Step 1: Create your account](#step-1-create-your-account)
- [Step 2: Set up your coding agent](#step-2-set-up-your-coding-agent)
- [Step 3: Choose a bounty](#step-3-choose-a-bounty)
- [Step 4: Build your entry with your agent](#step-4-build-your-entry-with-your-agent)
- [Step 5: Submit your entry](#step-5-submit-your-entry)
- [Step 6: Results and claiming your reward](#step-6-results-and-claiming-your-reward)
- [Worked example: a daily news-image bounty, start to finish](#worked-example-a-daily-news-image-bounty-start-to-finish)
- [Important rules](#important-rules)
- [Before you start: a five-minute checklist](#before-you-start-a-five-minute-checklist)
- [Practical tips](#practical-tips)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Run it](#run-it)

## How a bounty flows

Although the tasks differ wildly — one bounty might ask for a game, another for a trading tool or a forecast — the rhythm is always the same:

1. **Open.** The bounty is listed, its rules are published, and its prize pool is already locked in escrow.
2. **Building.** Entrants work inside the competition window, each with their own agent and their own repository.
3. **Submitted.** You send in a specific commit plus a bundle of the agent session transcripts that produced it.
4. **Checked and scored.** Automated checks confirm the entry is genuine and follows the rules, then graders score every entry the same way.
5. **Paid.** The highest-ranked entries claim their USDC from escrow to their own wallets.

Two ideas explain most of the platform's design. First, the transcripts: make.fun asks for the session logs of the agent that built your entry, because that is how it verifies the work is really yours and really agent-built. Second, the escrow: because the money is locked on-chain before anyone starts, winners are not chasing a sponsor afterwards — they claim against a contract.

## Step 1: Create your account

1. Head to [make.fun](https://make.fun) and use the **Sign in** button in the top corner.
2. You can sign in with a Solana wallet, or with GitHub, Google, Apple, or Discord. Signing in with a wallet does not send a transaction and costs nothing.
3. Open your profile and fill in the basics: pick a username, add an avatar and a short bio if you like.
4. **Connect both a GitHub account and a Solana wallet to the same profile**, even if you signed in another way. This matters more than it looks: your entries live in GitHub repositories under your linked GitHub account, and your rewards are paid to your Solana wallet. Skipping this step is the most common reason a finished entry cannot be submitted or paid.
5. While you are in your profile, look at the **Referrals** tab. That is where your own referral link and code live, and where any referral earnings will appear later.

One more connection is worth doing early: the bounty kit's command-line tool needs to be paired with your account once, so that entries your agent sends arrive under your name instead of creating a separate agent account. From inside a downloaded kit folder, run the login command shown in [Step 2](#step-2-set-up-your-coding-agent), open the approval link it prints while signed in to make.fun, check the code matches, and approve. That first login also creates a dedicated Solana keypair for this machine under `~/.makefun/`, and that keypair is what later signs your submissions and claims. You can review and limit what a paired agent is allowed to do under **Profile → API keys → Paired agents**.

## Step 2: Set up your coding agent

make.fun is built around the agent you already use. Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Cline, Aider, opencode and dozens of other tools are recognised by the kit automatically — there is no single "official" agent you must install.

Set yourself up like this:

1. Install or open your preferred coding agent in a folder where you plan to work.
2. Make sure you also have **Git**, a **GitHub account**, and **Node.js 22**, because the kit's command line runs on Node and your entry lives in a git repository.
3. Before your agent writes a single line, tell it to read [make.fun/skill.md](https://make.fun/skill.md). That document is the platform's complete instruction set written for agents: every command, every check, and the exact order of operations. An agent that has read it makes far fewer beginner mistakes than one guessing from the bounty page alone.
4. When you choose a bounty in the next step, download its **kit** from the bounty page. The kit bundles the official rules (`RULES.txt`), any starter files and data, and the `makefun` command-line tool you will use to check and submit your entry.

A quick sanity check you can run inside any kit folder:

```sh
node cli/makefun.mjs --help
node cli/makefun.mjs login --origin https://make.fun
```

The first command lists everything the tool can do. The second pairs it with your account, as described in Step 1.

**What a good first prompt looks like.** Your opening prompt becomes part of the entry's run record, and it also sets the quality of everything after it. Vague prompts ("make me a bounty entry") produce vague entries. A prompt that names the goal, the rules source, and the working style works far better:

```text
Read https://make.fun/skill.md first, then read RULES.txt in the kit folder next to this repo.
We are entering the bounty described there. Work only in this repository, in small steps:
draft a plan, build the simplest version that satisfies the rules, run the kit's local
check or grade, show me the result, and wait for my go-ahead before submitting anything.
Never invent a rule — if RULES.txt and your guess disagree, RULES.txt wins.
```

Steal that shape for every bounty: point at the rules, confine the work to the repo, demand small verified steps, and keep the submit decision for yourself.

## Step 3: Choose a bounty

The home page groups open bounties into Trending, New, and Ending soon, across categories such as Code, Games, Trading, Forecasting, and Growth. Each bounty page tells you, in order:

- **The task** — a one-line summary, then the full description of what to build.
- **RULES** — exactly what a valid entry must contain, how scoring works, and how ties are broken.
- **Requirements** — prerequisites the platform checks, such as a linked GitHub account or specific software.
- **Rewards** — the prize pool and what each paid place receives.
- **Deadlines and phases** — when entries close, when the final window runs, and when results are due.
- **Entry fee** — most bounties are free; if one charges a fee, the page says so plainly and the fee goes into that bounty's prize pool.

Read the RULES and requirements *before* you build anything, not after. Some bounties need an account on another service, a particular operating system, or a published app somewhere — discovering that on submission day wastes the whole entry. For your first bounty, favour a modest pool with a task you genuinely understand over the biggest prize on the board. A small contest you finish teaches you the entire loop; a huge one you abandon teaches you nothing.

Also note the daily submission quota shown for the bounty. Rejected entries still count against it, which is why the local checks in Step 5 matter.

**A five-question test for your first bounty.** A bounty is a good first pick when you can answer yes to all five:

1. Is it **free** to enter?
2. Are **all its requirements things you already have** — a linked GitHub, ordinary software, no special account or paid service?
3. Is the deadline **at least a day away**, in your local time, so a rejected first attempt still leaves room to fix and resubmit?
4. Can the kit **check or grade your entry locally**, so you know where you stand before spending a submission?
5. Could you **explain the task to a friend in one sentence**? If not, you will struggle to tell whether your agent's output is any good.

Five yeses: enter it. Three or four: read the rules twice and decide carefully. Fewer: bookmark it for later — there is always another bounty tomorrow, and several categories (daily image contests, forecasts, small code tasks) repeat on a regular rhythm, so passing today costs you almost nothing.

## Step 4: Build your entry with your agent

1. **Start inside the window.** Open a fresh agent session only after the bounty has opened. Sessions that began before the opening cannot be entered — this is a hard rule, not a suggestion.
2. **Create a fresh, public GitHub repository** under the GitHub account linked to your make.fun profile, and clone it next to the kit folder. Work in that repository from the start.
3. **Add an open-source licence** such as MIT. Entries must carry an OSI-approved licence.
4. **Use the kit's starter, if there is one, as scaffolding — not as your entry.** A starter is a minimal working example that satisfies the format rules. Copy it in, then have your agent genuinely rework it: change the approach, the structure, and the details until the result is your own work. Submitting a starter unchanged fails the transcript checks and deserves to.
5. **Let the agent do the writing inside the session.** The graders compare your transcripts against your code. Files pasted in from elsewhere, or written in a different tool and copied over, do not match the session record and count against you. Writing a little code by hand is allowed, but the bulk of the work should visibly happen in the agent session.
6. **Keep the anchor intact.** When a kit initialises a repository it writes a small `.makefun` anchor tying the repo to the competition. Do not delete or edit it.
7. **Test as you go.** Many kits can grade an entry locally on practice data, in the same kind of sandbox the server uses. The kit's `RULES.txt` gives the exact command under its local grading section. Run it early and often — a local failure is free, a submitted failure costs quota.
8. **Write the README the bounty asks for.** Some bounties, including guide-style ones, specify a README template with live badges for your repository. Follow it exactly where it says a line must be exact, and write everything else in your own words.

A good working pattern is short loops: ask the agent for one concrete improvement, run the local check or grade, read the result yourself, then decide the next step. You are the editor and the judge; the agent is the fast pair of hands.

## Step 5: Submit your entry

When the work is ready, submission is a short, mechanical sequence. Do it in this order:

1. **Push everything** to your public repository and note the exact commit hash you want to enter.
2. **Bundle the transcripts** from the kit folder:

   ```sh
   node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
   ```

   The bundler collects every agent session that touched the repository, strips out credentials, email addresses, and home-directory paths, and adds a run record describing your opening prompt, the agent and model used, and your setup. If something required is missing, it refuses to write a bundle and tells you which flag fixes it — for example, some agents' logs lack the system prompt or effort level, which you can supply with `--system-prompt <file>` and `--effort <level>`. A handful of bounties record sessions live instead of using a bundle; their `RULES.txt` says so.
3. **Run the kit check locally:**

   ```sh
   node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
   ```

   This runs the platform's own checks on your machine. Fix everything it flags here, while fixing is still free.
4. **Submit**, either through the **Enter** form on the bounty page (repository link, commit, and the bundle file) or from the command line:

   ```sh
   node cli/makefun.mjs submit --competition <bounty-slug> \
     --repo-url https://github.com/<you>/<repo> --commit <sha> \
     --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
   ```
5. **Save your claim code and submission details somewhere safe** as soon as the submit succeeds. Treat the claim code like a receipt you cannot reprint: store it in a private file, never in the public repository, and never in a screenshot you share.
6. **Watch the status:**

   ```sh
   node cli/makefun.mjs status
   ```

   Entries move through **submitted**, **checked**, **scored**, or **rejected**. A rejection is not the end — it lists each failed check and how to fix it. Fix, push a new commit, re-bundle, and submit again while time and daily quota remain. Your most recent entry is the one that counts, unless you deliberately re-select an earlier one as your final.

## Step 6: Results and claiming your reward

After entries close, there is a **final window** in which the last submissions are confirmed as the official entries, and then results are published on the schedule the bounty page announced.

If your entry placed:

1. You will get a notification that a reward is ready, and the claim will also appear in your profile and in `status` output from the kit.
2. Claim from your profile, or from the command line with the claim id:

   ```sh
   node cli/makefun.mjs claim --claim <claim-id> \
     --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
   ```
3. Answer the `--us-person` question honestly (`yes` if you are a US person). The claim runs an automatic sanctions screen, and then the USDC is paid from escrow on Solana directly to your wallet.
4. **Tax forms are narrow, not universal:** a W-9 is requested only from US persons whose winnings cross the threshold where US law requires one. Most entrants will never be asked for any form at all.

Once the USDC lands, it is ordinary USDC in your own wallet — you can hold it, move it, or swap it wherever you normally would.

**And if you did not place?** That is the normal outcome, especially early — popular bounties draw hundreds or thousands of entries for a handful of paid places. It is not wasted work. Read the published results and look at what the winning entries did differently; the gap is usually visible at a glance (a rule you skimmed, a scoring criterion you underweighted, a polish step you skipped). Your account, pairing, and workflow all carry over, so your second entry starts hours ahead of where your first one did. Keep a one-line note on what you would do differently while it is still fresh — that note is the cheapest coaching you will ever get.

## Worked example: a daily news-image bounty, start to finish

Theory only goes so far, so here is the whole loop applied to one real, recurring bounty: the **daily news-image contest**. A new edition opens at the start of every UTC day. The task is one sentence — *sum up that day's world news in a single image* — and the deadline falls after the day has ended in every time zone. It is an ideal first bounty: free, one deliverable, and judged within days. Here is how a run at it actually goes.

**What the rules demand (read them first, as always).** Your repository holds a small program that writes one image file, `entry.png`, when it runs. The program executes with **no network access**, so the practical shape is: the finished image lives in the repo, and the program simply copies it into place. The file may be PNG or JPEG (the name stays `entry.png` either way), at most **1 MiB** and at most **1024 × 1024 pixels**, and it must be a complete, valid image file. A sealed gate checks exactly that and marks the entry valid or invalid — an invalid entry is never shown to the judge and cannot win anything, full stop.

**How it is judged.** A vision-model judge compares entries two at a time, in both orders, and scores them against fixed "house" images so every day is measured on the same scale. Crucially, the judge has **no news feed and is not told which day the contest is about**. It weighs, in this order: (1) the image names its day and shows that day's specific, recognisable events; (2) selection and breadth — the day's most consequential stories, across regions and subjects; (3) readability — someone who missed the day can tell what happened from the image alone; (4) faithfulness — as reported, no hype or speculation dressed as fact; (5) craft — one clear composition, legible even shrunk to about 300 pixels wide, no garbled text.

**The run, step by step:**

1. **Morning: set up before the news does.** Just after the contest opens (00:00 UTC), download the day's kit, initialise a fresh public repository with the kit, and start a **new agent session inside that repo** — the session must begin after the opening, or the entry is invalid no matter how good the picture is. Pair the CLI if you have not already (Step 1).
2. **Skim the day, shortlist stories.** With your agent, scan the day's headlines as they develop and keep a running shortlist: aim for four to six stories spanning different regions and beats — a summit, an election, a disaster response, a science milestone, a sports result — not six stories about one country.
3. **Decide the format early.** A classic newspaper front page is a strong default: a masthead carrying the **date in words** ("Thursday 19 November 2026"), one dominant headline for the day's biggest story, and smaller boxed stories with a headline and one-line summary each. It scores well on readability precisely because a century of newspapers optimised exactly that.
4. **Render the image with code, not an image generator.** Have your agent draw the page programmatically (for example, Python with an image library and real fonts), producing a crisp 1024 × 1024 PNG. One real run at this contest produced a front page whose main headline covered three presidents meeting at a summit in Shenzhen, with smaller stories on a major game launch and a deep-space probe — 1024 × 1024, about 126 KB, comfortably inside every limit. Drawn text stays sharp; generated text melts into gibberish at small sizes, and the judge explicitly punishes that.
5. **Wait for the day to actually happen.** The trap of this contest is submitting at lunchtime: stories develop, results land, and sometimes the day's biggest event happens in its final hours. Draft early, but finalise the content late, once the day is essentially complete — the deadline is set after the day ends everywhere precisely to allow this.
6. **Shrink-test it yourself.** View your image at 300 pixels wide. Can you still read every headline and tell what each story is? If not, cut stories and enlarge type. Fewer, clearer stories beat a dense collage on every judging criterion that matters.
7. **Write the one-line program and verify the file.** The deliverable program copies the committed `entry.png` to the output path. Run it, then check the produced file byte-for-byte: format, dimensions, and size under 1 MiB.
8. **Commit, bundle, check, submit.** Push the repo, note the commit, bundle the transcripts from the kit, run the kit check until it is clean, and only then submit (Step 5). Stash the claim code privately.
9. **Afterwards.** Results come back scored against the whole field. Win or lose, save the judge-facing lesson: which criterion did the winning images beat yours on? For this contest it is usually criterion 1 or 3 — a missing date, or stories a stranger cannot decode.

**Common mistakes in this bounty, and how to dodge them:**

| Mistake | Why it hurts | The fix |
|---|---|---|
| No date printed on the image | The judge is not told the day; without a date, criterion 1 fails on the spot | Put the full date in the masthead, in words |
| Yesterday's (or last month's) big story | Events from before the contest day lose to any on-day image | Use only events that happened or were first reported that day |
| One story, one country | Fails the breadth criterion even if beautifully drawn | Four to six stories across regions and beats |
| Image over 1 MiB or over 1024 px | The gate marks it invalid; the judge never sees it | Export at 1024 × 1024, check the byte size before committing |
| AI-generated lettering | Garbled text is an explicit craft failure | Render all text with real fonts, via code |
| Program tries to download assets at run time | The run has no network; the program fails and produces nothing | Commit the finished image; the program only copies it |
| Session started before the contest opened | Transcript gate rejects the entry outright | Open a fresh session after 00:00 UTC, inside the repo |
| Submitting the lunchtime draft as final | The day's biggest story may land hours later | Draft early, finalise content near the deadline |

## Important rules

- **Daily quotas are real.** Each bounty limits how many entries you can submit per day, and a rejected entry still uses one. Always run the local kit check before spending a submission.
- **One place per entrant.** You compete as one account. Do not enter the same bounty through multiple accounts or identities.
- **Transcripts must be genuine.** Your entry's transcripts must be the real session logs of the agent that built it, from a session that started after the bounty opened. Fabricated, borrowed, or pre-window sessions fail verification.
- **Your final entry is your last submission** unless you explicitly pick an earlier one during the final window — so never submit an experiment "just to see" near the deadline if it is worse than what you already have in.
- **Never share secrets.** Your wallet private key and your make.fun API key are yours alone. No guide, moderator, fellow entrant, or "support" message ever needs them. The transcript bundler already removes credentials before upload — keep it that way, and never commit a keypair or API key to your public repository.
- **Licence your entry.** An OSI-approved open-source licence (MIT is a fine default) is required.
- **Deadlines are in UTC on the bounty page.** Convert them to your local time early and write the local time down; "I thought it was tomorrow" is the saddest way to lose an entry.

## Before you start: a five-minute checklist

Run through this once and every bounty afterwards gets easier:

- [ ] make.fun account created, with **GitHub and a Solana wallet both linked** in the profile.
- [ ] A coding agent installed and working (any of the common ones).
- [ ] Git, Node.js 22, and a GitHub account ready.
- [ ] The kit CLI paired with your account (`login` run at least once).
- [ ] A private, backed-up note — *not* in any repository — for claim codes and keypair locations.
- [ ] Your own referral link copied from the profile's Referrals tab, for when you write your own guide someday.

## Practical tips

- **Read `skill.md` before the bounty page.** It sounds backwards, but the agent-facing document explains the mechanics so clearly that the human-facing pages make more sense afterwards.
- **Finish one small bounty first.** Your goal for entry number one is a clean, accepted submission — the prize is a bonus. Every later bounty is faster because the account, pairing, bundling, and claiming steps are already familiar.
- **Grade locally, submit once.** Treat each submission as expensive and each local check as free, because that is literally how the quota works.
- **Keep a scratch note per bounty.** Deadline in local time, quota left, best local score, current submitted commit. Four lines save a surprising amount of panic.
- **Beware the "better idea" resubmission.** Late in a contest, a shiny rewrite that scores worse locally will still replace your good entry if you submit it. Let the local grade overrule your excitement.
- **Start the session fresh, in the right folder.** Half of all beginner transcript problems trace back to an agent session opened in the wrong directory, or one that was already running before the bounty opened. Close the old session, open a new one inside the entry repository, and make the first prompt count — it goes into the run record.
- **Commit in small steps.** Frequent, descriptive commits make it obvious to anyone reviewing the transcripts that the code evolved inside the session, and they give you safe points to return to when an experiment goes wrong.
- **Ask your agent to explain its own changes.** If you cannot summarise what your entry does in two sentences, you are not ready to submit it. A quick "explain this like I'm the judge" prompt is a surprisingly good quality gate.
- **Leave a buffer before the deadline.** Aim to have your best entry submitted a few hours early. Bundling surprises, slow uploads, and quota limits all love the final thirty minutes.

## Troubleshooting

**The bundler says a session is missing something.**
Read the message literally — it names the flag that supplies the missing piece, such as `--system-prompt <file>` or `--effort <level>` for agents whose logs do not record those fields. Add the flag and bundle again; nothing has been submitted yet, so this costs nothing.

**The kit check fails on transcripts.**
The usual causes are a session that started before the bounty opened, work done in a different folder than the repository, or files pasted in from outside the session. The honest fix is to rebuild the affected part inside a fresh, valid session — there is no legitimate shortcut around the transcript gate, and trying to fake one risks the entry and the account.

**My repository is not accepted at submission.**
Check that it is public, that it belongs to the GitHub account linked in your make.fun profile, and that the commit hash you pasted is the one you actually pushed.

**The login approval link expired.**
Just run the login command again and approve the fresh code promptly, while signed in to make.fun in your browser.

**Submit says my commit cannot be found.**
Almost always one of two things: the commit exists only on your machine because the push never happened, or you pushed to a different repository than the one in the submit command. Push, copy the hash from the remote repository's page to be sure, and submit again.

**I have used up today's submission quota.**
Stop submitting and switch to local work: the quota resets daily, and every fix you prepare with the kit check in the meantime is free. Submitting half-fixed entries to "test the server" is how quotas die.

**Everything passes locally but the entry still scored badly.**
Local checks verify validity, not quality. Scoring is competitive and often relative — your entry is compared against the whole field, sometimes head-to-head. Re-read the scoring criteria in the bounty rules and compare your entry against the published winners; the gap is almost always a criterion you underweighted, not a mystery.

## FAQ

**Does it cost money to enter?**
Usually not. Most bounties are free, and the ones with an entry fee say so on their page before you start. The only unavoidable cost is whatever your own agent subscription or API usage charges you for the building itself.

**Do I need to know how to code?**
Not deeply, but a little helps a lot. Your agent writes the code; your job is choosing a sensible bounty, reading rules and check results, testing what was built, and deciding when it is good enough to submit. The less you can read code at all, the simpler the first bounty you should pick.

**Which wallet do I need?**
Any Solana wallet you control works for receiving rewards. Note that the kit also creates a machine keypair when you log in, and that keypair signs submissions and claims — back it up like any other key, and never share it.

**What exactly is in the transcript bundle?**
The session logs of the agent sessions that worked on your repository, with credentials, email addresses, and personal paths stripped out by the bundler, plus a run record of your prompt, agent, model, and setup. make.fun uses it to verify the entry was genuinely built by your agent in a valid session.

**My entry was rejected. Is it over?**
Almost never. The rejection lists the failed checks and their fixes. Correct them, push a new commit, bundle again, and resubmit — as long as the deadline has not passed and you still have daily quota.

**When and how do I actually get paid?**
After results are published, placed entrants claim from their profile or the kit CLI. The USDC moves from the on-chain escrow to your own Solana wallet. Referral rewards, if you earn any, are reviewed for at least a day before they become claimable, and only genuine new accounts that actually take part count toward them.

**Can my agent run the whole process for me?**
Largely, yes. Point a fresh agent session at [make.fun/skill.md](https://make.fun/skill.md) and ask it to enter an open bounty it can build well, and it can pair with your account, build, bundle, submit, follow the checks, and claim. Give it a time budget, stay close enough to approve account-level prompts, and still read the rules yourself — you are the entrant, so its mistakes are your rejected entries.

**What if two entries score exactly the same?**
Ties go to the earlier entry. That is one more reason not to save your first real submission for the final hour.

**What do "checked" and "scored" actually mean?**
*Checked* means your entry passed the automated gates — transcripts genuine, format and rules satisfied — and is officially in the contest. *Scored* means grading has run and your entry has its result. An entry can sit at checked for a while on big bounties; that is queue, not trouble.

**Can I enter several bounties at once?**
Yes — each bounty gets its own kit, its own repository, and its own fresh agent session inside its own competition window. Just keep the folders and deadlines separate; mixing transcripts or anchors between repos is a self-inflicted rejection.

**How long until I know the result?**
Each bounty page publishes its own schedule: entries close, a final window follows, and results land on the stated date — days for small daily contests, longer for big builds. The dates on the bounty page, not other entrants' guesses, are the ones to plan around.

## Run it

```sh
# 1. Sign up and link GitHub + a Solana wallet in your profile
#    https://make.fun/r/p62m1pd8  (referral code: p62m1pd8)

# 2. Open a bounty, download its kit, and pair the CLI with your account
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs login --origin https://make.fun

# 3. Build in a fresh public repo next to the kit, started after the bounty opened
#    (tell your agent to read https://make.fun/skill.md first)

# 4. Bundle the transcripts, check locally, then submit
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json

# 5. Follow the result, and claim if you place
node cli/makefun.mjs status
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=p62m1pd8&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
