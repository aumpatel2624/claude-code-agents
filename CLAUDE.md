# Claude Multi-Agent Development Pipeline

This repository uses a fully automated Claude Code multi-agent pipeline.
All user prompts MUST flow through this pipeline, and every agent MUST follow the rules below.

When Claude Code opens this folder, it MUST automatically initialize the pipeline **without asking for confirmation**.

---

# 🚀 1. Mandatory Automatic Startup (NO USER CONFIRMATION)

Claude Code MUST NOT ask the user whether to proceed.

Immediately upon opening this workspace, Claude MUST automatically run:

## **1. explore-on-diet-coke**

Runs *immediately* on workspace load.

Responsibilities:

* Explore the entire project directory using Explore
* Detect if the project is:

  * frontend
  * backend
  * full-stack
  * unknown
* Detect frameworks, file structure, environment, dependencies
* Build a complete XML project context
* Save it to:

```
/project-context/context.xml
```

This XML becomes the **single source of truth** for all agents.

It MUST produce this final message:

```
Project context initialized.
```

---

## **2. pipeline-announcer**

Runs *immediately after* explore-on-diet-coke.

Its job:

* Announce the entire multi-agent architecture
* Explain the pipeline order
* Declare which agents are allowed to act
* Explain how routing must work
* Instruct Claude Code to always route user prompts through xml-generator → router → worker agents
* Instruct that worker agents NEVER speak directly to users

It MUST produce this final message:

```
Pipeline initialized.
```

---

# 🧠 2. Core Runtime Pipeline (Every User Prompt)

Every user prompt MUST be processed by this sequence:

1. **xml-generator**
2. **router-agent**
3. **Correct worker agent**

No other sequence is permitted.

---

## **3. xml-generator** (always runs FIRST for every user message)

Responsibilities:

* Capture raw user prompt
* Auto-generate a slug (lowercase, hyphens)
* Save raw prompt to:
  `/prompts/boring/<slug>-raw.txt`
* Convert prompt to an XML specification
* Save generated XML to:
  `/prompts/XML/<slug>-xml.xml`
* Ask tech stack questions *only once*, then save to:
  `/tech-stack/tech-stack.md`
* Load tech stack for future prompts
* Add `<target_agent>` for routing

It MUST end with:

```
XML generated and stored successfully.
```

---

## **4. router-agent**

The ONLY allowed dispatcher.

Responsibilities:

* Read XML `<target_agent>`
* Activate the correct worker agent
* NEVER modify XML
* NEVER produce code
* NEVER respond to the user
* NEVER skip steps

If `<target_agent>` = `frontend-developer`, router MUST send the XML to:

```
frontend-react-developer, process this XML.
```

And then route based on the delegator's instruction:

```
router: forward to html-frontend-agent
router: forward to react-component-agent
router: forward to css-styling-agent
```

---

# 🎨 3. Worker Agents (Generate Code Only When Router Tells Them)

These agents MUST NEVER speak to the user directly. They ONLY act when router-agent commands them.

---

## **5. backend-developer**

Generates backend code (API routes, controllers, services, logic).

---

## **6. frontend-react-developer** (delegator)

Does NOT generate code. Determines which frontend agent should execute:

* HTML → html-frontend-agent
* React components → react-component-agent
* Styles → css-styling-agent

Responds ONLY with:

```
router: forward to <agent>
```

---

## **7. html-frontend-agent**

Produces semantic, accessible HTML.

---

## **8. react-component-agent**

Produces React/Next.js components.

---

## **9. css-styling-agent**

Generates styling in the framework specified in `<tech_stack>`:

* Tailwind
* CSS Modules
* Bootstrap
* Styled Components
* SCSS
* etc.

---

# 📌 4. Pipeline Enforcement Rules

Claude Code MUST ALWAYS:

* ✔ Auto-run explore-on-diet-coke on startup
* ✔ Auto-run pipeline-announcer immediately after
* ✔ Pass **every** user prompt to xml-generator
* ✔ Pass xml-generator output ONLY to router-agent
* ✔ Allow router-agent to select the correct worker agent
* ✔ Prevent worker agents from talking to users
* ✔ Prevent worker agents from running without router-agent

### ❌ Forbidden:

* Worker agents replying directly to the user
* Worker agents running without router-agent
* Any agent producing XML except xml-generator
* Any agent writing files except xml-generator & explore-on-diet-coke
* Skipping pipeline steps
* Asking the user for confirmation during startup

---

# ✔ Summary

This workspace is configured for a **fully automated, AI-driven, XML-based development pipeline**.

Claude Code MUST ALWAYS process requests in this sequence:

1. **explore-on-diet-coke**
2. **pipeline-announcer**
3. **xml-generator**
4. **router-agent**
5. **Correct worker agent**

   * backend-developer
   * frontend-react-developer
   * html-frontend-agent
   * react-component-agent
   * css-styling-agent

This is the ONLY valid workflow for this project.
