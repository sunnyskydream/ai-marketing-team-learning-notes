# ai-marketing-team-learning-notes

Personal learning notes from the YouTube tutorial [**Claude Code: Build Your Full AI Marketing Team (Agents + Claude Skills)**](https://www.youtube.com/watch?v=yLXLHnD4fco&t=2s). This repository captures hands-on steps, original video prompts, and personal takeaways for building an AI-powered marketing workflow with Claude Code — covering skill creation, generative image pipelines, autonomous data-analyst agents, and team workflow automation.

---

## Module 1: Claude Code Setup in VS Code

**Steps:**
- Install the Claude Code extension and configure it within VS Code
- Enable **Live Server**: right-click any `.html` file to spin up a local development port

**Reference:** [How To Use Claude Code In VSCode — Learn AI In 5 Minutes Series](https://www.youtube.com/watch?v=Nmr02XA9n1o)

---

## Module 2: Claude Skills — Official and Custom

Install Anthropic's official skill library via `/plugins > add document-skills`, then build custom marketing skills on top.

### 2a. Branded Deck Skill (`/skill branded-deck`)

**Steps:**
1. Download a free Canva template and save it to `/_templates/deck_templates/`
2. Upload the template to Claude and run the deck template analysis prompt below — save the output as `_templates/deck_template_analysis.md`
3. Package the template and analysis into a reusable `branded-deck` skill using `/skill skill-creator`

<details>
<summary>Video Prompt — Deck Template Analysis</summary>

```
I've uploaded our company's presentation template (/_templates/deck_templates/pptx) which I need to re-create as a new template. Please analyze it thoroughly and report back on:

1. COLOR SYSTEM — exact background colors, accent hex codes, and usage patterns
2. TYPOGRAPHY — fonts for headers vs. body text, text style patterns (uppercase, letter-spacing, bold weights), approximate font sizes
3. LAYOUT PATTERNS — list each distinct slide layout type and describe where content elements are positioned
4. DECORATIVE ELEMENTS — shapes, icons, background graphics, borders. Note what you can reliably reproduce vs. what needs simplification
5. RECURRING ELEMENTS — what appears on every slide (footers, logos, page numbers). Note logo position, size, and which slides include it

Be specific about what you can and cannot reproduce. For elements you cannot recreate from scratch, indicate whether they should be extracted from the original file and re-embedded.

Output your analysis in markdown format. Save it in the /_templates/ folder.
```

</details>

<details>
<summary>Video Prompt — Create Skill `/skill branded-deck`</summary>

```
Help to package our branded presentation template into a reusable Claude skill to be used for this project, extending the official pptx skill. This skill will be used to create branded strategy and campaign presentation decks. It will include a mix of layout types from the template that fit the storytelling of the presentation, and match the template's colors, fonts, spacing and style exactly.

- Our branded presentation template (_templates/deck_template.pptx)
- Our template analysis report (_templates/deck_template_analysis.md)

The branded presentation template should be included in the /assets folder inside the skill file. The deck_template_analysis.md should go in the /references folder inside the skill file.
```

</details>

**Video Prompt — Use Skill:**
> "Create a Q3 summer tea cold brewing campaign strategy deck targeting millennial fusion drinkers in the San Francisco Bay Area. Include campaign goals, audience segments, channel strategies, budget allocation, and a 12-week rollout timeline with data visualizations for channel budget breakdown, audience segment split, and projected bookings."

**My Notes:**
- Plain, illustration-free templates parse and reproduce far more reliably — avoid heavily decorated Canva designs

---

### 2b. Social Creative Designer Skill (MCP Image Generation)

Combines a custom Claude skill with the [Nano Banana MCP server](https://github.com/zhongweili/nanobanana-mcp-server) for AI-generated social visuals.

**Steps:**
1. Use Gemini to generate a `STYLE-GUIDE.md` and 5 reference images from the branded deck template
2. Create `.mcp.json` in the project root to connect to the Nano Banana MCP server (note: the tutorial uses macOS paths — ask Claude to adjust for Windows)
3. Create the skill using `/skill skill-creator` and save it to `.claude/skills/`

<details>
<summary>Video Prompt — Create Skill `/skill social-creative-designer`</summary>

```
I need a Claude skill for designing and creating carousel-style social media or single social graphics to be used for social platforms. When given a topic or content, it will design a set of carousel slides or single social visuals as PNG images using the Nano Banana MCP for image generation.

Default Behavior:
- Generate 3 slides per set unless specified otherwise
- Default aspect ratio is 4:5. Also supports 1:1, 3:4, and landscape 1.91:1 if requested
- Default platform is Instagram. Also supports LinkedIn, Facebook, and other platforms
- Slide 1 = bold, attention-stopping hook headline with minimal text. Middle slides = value/education/insights. Final slide = CTA or key takeaway
- Supports single static image mode when requested
- Unless the user specifies a style, first read /_templates/social-creatives/STYLE-GUIDE.md to understand available style directions, then reference the corresponding image set as directional inspiration. Do not replicate them exactly — adapt and combine elements to best suit the topic
- Use the Nano Banana MCP to generate all visuals. If unavailable, flag this to the user

Name this skill "social-creative-designer". Use the skill-creator skill for proper formatting. Save it under this project's .claude/skills/ folder.
```

</details>

**Video Prompt — Use Skill:**
> "Create a 5-slide Instagram carousel about 'Your perfect tea brewing ritual', using the illustrated playful style."

**My Notes:**
- Use the [TypeUI DESIGN.md Chrome Extension](https://github.com/bergside/design-md-chrome) to extract a `DESIGN.md` or `SKILL.md` from any website — this lets you apply any brand's design system to your creatives

---

## Module 3: Data Analyst Agent

**Steps:**
1. Install `data-viz-creator` skill from [MCP Market](https://mcpmarket.com/tools/skills/advanced-data-visualization)
2. Install `campaign-report` skill from [GitHub](https://github.com/alirezarezvani/claude-skills/blob/main/marketing-skill/campaign-analytics/SKILL.md)
3. Create the agent with the prompt below

**Video Prompt — Create Agent:**
> "Create an agent called 'data-analyst'. This agent takes raw campaign data, datasets, or performance metrics to produce clear, actionable reports or interactive visualizations. It should identify trends, flag anomalies, and present findings in a way that non-technical marketers can immediately act on. Equip it with the data-viz-creator skill and campaign-report skill."

**Video Prompt — Use Agent:**
> "@.claude/agents/data-analyst.md Analyze our summer tea brewing campaign performance using all data files in the reports folder. Create a campaign report and a performance dashboard ready to present and share with the team."

**My Notes:**
- Use Gemini to generate synthetic campaign CSVs first (daily performance, KPI scorecard, funnel details, A/B test results, cohort retention) — a practical way to test the agent before connecting real data

---

## Module 4: Team Workflow with Notion Kanban

**Steps:**
- Set up a Notion Kanban board and share tasks across the team
- Activate **Remote Control** via `/remote-control` to let Claude read the board, pick up to-do items, complete them, and move cards to "Done" for human review

---

## Technical Stack

| Component | Tool |
|---|---|
| AI Coding Assistant | Claude Code (VS Code extension) |
| Presentation Skill | `pptx` skill + custom `branded-deck` skill |
| Image Generation | Nano Banana MCP Server |
| Social Media Skill | Custom `social-creative-designer` skill |
| Data Analysis Agent | Custom `data-analyst` agent |
| Visualization | `data-viz-creator` skill |
| Campaign Reporting | `campaign-report` skill |
| Workflow Management | Notion Kanban + Claude Remote Control |

---

## Resources

| Resource | Link |
|---|---|
| Source Tutorial | [YouTube — Build Your Full AI Marketing Team](https://www.youtube.com/watch?v=yLXLHnD4fco&t=2s) |
| Claude Code VSCode Setup | [YouTube — Learn AI In 5 Minutes](https://www.youtube.com/watch?v=Nmr02XA9n1o) |
| Anthropic Official Skills | [github.com/anthropics/skills](https://github.com/anthropics/skills) |
| TypeUI DESIGN.md Extension | [github.com/bergside/design-md-chrome](https://github.com/bergside/design-md-chrome) |
| Nano Banana MCP Server | [github.com/zhongweili/nanobanana-mcp-server](https://github.com/zhongweili/nanobanana-mcp-server) |
| MCP Market Skills | [mcpmarket.com/tools/skills](https://mcpmarket.com/tools/skills) |
| Campaign Analytics Skill | [github.com/alirezarezvani](https://github.com/alirezarezvani/claude-skills/blob/main/marketing-skill/campaign-analytics/SKILL.md) |
| skill-creator-advanced | [Facebook Post](https://www.facebook.com/allanyiin/posts/pfbid02VpZi78ZPLm8FGJdtJAVd3rrLWMCpDEp4tYbMroVXZbHWbDYVY3CWDw6yCfYNUB2Kl) |
