# GitHub Spark: What I Learned

## Overview

As part of the Customer Outcomes onboarding series, I explored GitHub Spark as a tool for building micro-apps from natural language prompts. The exercise was to build four customer-facing tools: a Renewal Risk Dice Game, a Customer Health Checker, a Customer Outcomes Daily Wins tracker, and a Customer Renewal Simulator. This document captures what I learned, where Spark delivered, and where it fell short.

---

## What Spark Is

GitHub Spark is GitHub's answer to prompt-to-app builders — you describe what you want in plain language and it generates a full-stack React/TypeScript web app, hosts it, and gives you a live URL. It's available to Copilot Pro+ and Enterprise subscribers and is currently in public preview.

The pitch: go from idea to deployed app without writing code or configuring infrastructure.

---

## What Works

**It builds fast.** A working app from a single prompt in under a minute is real. For simple tools with clear logic, Spark gets you somewhere functional quickly.

**The hosting is seamless.** You get a live URL immediately — no GitHub Pages setup, no deployment configuration. For sharing a quick demo or prototype, that's genuinely useful.

**The logic holds up.** Given a detailed prompt, Spark understood the rules correctly — dice roll ranges mapped to risk levels, history tracked correctly, dropdown values populated from the list provided. The functional behavior was solid.

**It stays in GitHub.** For teams already living in GitHub, keeping prototypes, repos, and deployments in one ecosystem has real value.

---

## What Doesn't Work

**Design is generic.** The default output uses whatever component colors and layout Spark decides on — there's no consistent aesthetic, and it shows. The result looks like a form, not a product. Getting to something visually polished requires multiple iteration passes.

**Every pass costs money.** Spark runs on a prompt quota (375/month on Pro+) with overage charges around $0.16 per prompt. Iterating on design alone — fixing colors, layout, animations, behavior — burns through that budget fast with diminishing returns.

**Iteration takes longer than expected.** The promise is speed, but getting a design from functional to good requires a back-and-forth cycle that adds up. In practice, 10+ prompt passes to refine a single app is not uncommon, and you're still working around Spark's decisions rather than making your own.

**Files don't exist until you create a repo.** The app lives in Spark's managed environment by default. The code isn't accessible, version-controlled, or portable until you explicitly click "Create Repository." New users won't know this and may assume the URL is enough.

**Each app is its own repo.** There's no native concept of a multi-app project. Four apps means four repos, four URLs, and four separate things to manage and share — not ideal for a cohesive toolkit.

**No guided troubleshooting.** If something breaks, your options are to prompt your way out of it or open the code yourself in a Codespace and debug manually. There's no middle layer that explains what went wrong or helps you fix it. It either works or it doesn't. Silent logic errors are a real risk — in testing, the Renewal Risk Dice Game app only showed one company in the roll history regardless of which account was selected. The app ran without errors, but the behavior was wrong and Spark didn't flag it.

**It's tied to Spark's runtime.** The generated code depends on a private `@github/spark` node module that isn't publicly available, which limits portability outside of Spark's environment.

---

## Compared to Other Tools

| Tool | Strengths | Weaknesses |
|------|-----------|------------|
| **Replit** | Strong backend, real runtime environment, more framework flexibility | Requires more setup, less seamless for pure frontend tools |
| **Lovable** | Polished UI output, strong design defaults, faster to a finished look | Not inside GitHub, separate workflow |
| **GitHub Spark** | Native GitHub integration, instant hosting, no-code entry point | Generic design, prompt cost, limited portability, early-stage |
| **Direct AI build** | Interactive back-and-forth before a single line is built, option to plan and confirm scope first, full design control, no prompt cycles, option to preview as HTML before full implementation, single cohesive output | Best results when intent is clearly defined upfront |

---

## Key Takeaways

- Spark works best for quick functional prototypes where design quality doesn't matter much.
- The cost model makes iteration expensive — clear, detailed prompts upfront matter more than with other tools.
- For a multi-app toolkit meant to be shared as part of an onboarding exercise, a single-repo approach with a menu page is more practical than four separate Spark apps.
- Creating the repo is a manual step — Spark doesn't do it automatically, and until it's done, you don't own the code.
- Tools that allow full design control and the ability to preview a mock before committing to the full build produced better results with fewer cycles.

---

## Recommendation

Spark is worth knowing and worth showing customers as an example of what GitHub Copilot can enable. For internal use or onboarding exercises where speed matters more than polish, it gets the job done. For anything you'd want to put in front of a customer as a finished artifact, it needs more iteration than the prompt-and-go pitch suggests.

The onboarding exercise is a good fit for Spark — the point is to build, not to receive a finished product. Just set expectations that the first output is a starting point, not a final one.
