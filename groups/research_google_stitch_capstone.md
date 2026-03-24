# AI wireframing tools for the classroom: Google Stitch and beyond

**Google Stitch — Google's free, AI-powered UI design tool — has emerged as the most accessible entry point for turning text prompts into polished wireframes and HTML/CSS code, but it works best as one piece of a larger toolkit.** Launched at Google I/O 2025 after Google acquired Galileo AI, Stitch received a transformative "vibe design" update on March 18, 2026, adding an infinite canvas, voice input, and a design agent powered by Gemini 3. For a community college capstone course, the most practical stack combines Stitch (free ideation and wireframing) with Lovable or Bolt (free-tier full-stack MVP building) and a manual conversion step to produce Flask/Jinja2 templates. No single tool handles the entire creative-brief-to-Flask pipeline automatically, but the workflow is now surprisingly smooth — and almost entirely free for students.

---

## Google Stitch has evolved fast since its May 2025 debut

Google Stitch lives at [stitch.withgoogle.com](https://stitch.withgoogle.com/) and is a product of **Google Labs**, built on Gemini models. Its origin story is notable: Google acquired the startup Galileo AI in May 2025, rebranded it as Stitch, and announced it at Google I/O on May 20, 2025. The original Galileo AI domain now redirects to Stitch.

The tool accepts four input types: **natural-language text prompts**, uploaded images (wireframes, hand-drawn sketches, screenshots), pasted URLs (which it analyzes for design language), and — as of March 2026 — **voice commands** via the new Voice Canvas feature. From any of these inputs, Stitch generates complete UI layouts for web or mobile in roughly 60–90 seconds, producing multiple variant designs for comparison.

Three major updates have defined Stitch's trajectory:

- **May 2025 (launch)**: Two modes — Standard (Gemini 2.5 Flash, ~350 generations/month) and Experimental (Gemini 2.5 Pro, ~50 generations/month). Text-to-UI, image-to-UI, Figma export, HTML/CSS code export.
- **December 2025**: Gemini 3 integration delivering sharper layout quality, plus interactive **Prototypes** — the ability to link screens into clickable user flows with auto-generated follow-up screens.
- **March 18, 2026**: The "vibe design" overhaul — a complete reimagining as an **AI-native infinite canvas**. New features include Voice Canvas (speak to your design and get real-time critiques), Direct Edits (manual text/image/spacing tweaks on the canvas), a Design Agent with project-wide reasoning, an Agent Manager for parallel exploration, and **DESIGN.md** — a portable markdown file capturing design rules that can travel between tools.

The primary output format is **HTML + Tailwind CSS**, which is crucial for Flask integration. Figma export works via one-click "Copy to Figma" with editable layers and Auto Layout. A React component conversion exists through the [Stitch Skills](https://github.com/google-labs-code/stitch-skills) GitHub library (2,400+ stars), though native one-click React export is not yet available. There is **no direct Angular, Vue, Flutter, or SwiftUI export**, though the new MCP (Model Context Protocol) server bridges Stitch to coding tools like Cursor, Claude Code, and Gemini CLI for downstream conversion.

The most important fact for classroom adoption: **Stitch is completely free**. No paid plans exist. It requires only a Google account and is available to users 18+ in 160+ countries where Gemini is accessible.

**Key limitations** matter for setting student expectations. Outputs are mid-fidelity mockups rather than production-ready designs — typography can feel flat, color combinations sometimes look dated, and visual hierarchy tends toward generic. The tool generates no animations, hover effects, or backend logic. It lacks real-time collaboration, sometimes forgets components between iterations, and can produce inconsistent results from the same prompt across runs. As a Google Labs experiment, its long-term survival is uncertain.

---

## The competitive landscape splits into design tools and app builders

The AI UI tooling ecosystem in early 2026 has bifurcated into two categories: **design-first tools** (generating wireframes and visual prototypes) and **full-stack app builders** (generating working applications from prompts). Understanding this split is essential for choosing the right tool for each phase of a capstone project.

**For wireframing and ideation**, Google Stitch competes primarily with **Uizard** (acquired by Miro Labs in 2024, starting at $12/month with a limited free tier of 3 AI generations/month) and **Figma Make** (Figma's native AI feature, free for verified students with 3,000 AI credits/month). Uizard's unique strength is converting hand-drawn sketches into wireframes — a natural fit for whiteboard-first classroom workflows. Figma Make's advantage is operating inside the industry-standard design tool where professional teams already work. Newer entrants like **Moonchild AI** (generates designs using your actual component library) and **Flowstep** (fastest screen generation with frictionless Figma copy-paste) are gaining traction but lack education-specific pricing.

**For building functional MVPs**, the "big three" are **Lovable**, **Bolt.new**, and **Vercel v0**. Each occupies a distinct niche:

- **Lovable** (formerly GPT Engineer) generates complete full-stack applications — React frontend, Supabase backend, authentication, database, deployment — from natural language. It reached **$20M ARR in 60 days**, the fastest in European startup history. Its Visual Edits mode consumes no credits, and its free tier supports **up to 20 collaborators** per project, making it the strongest option for group capstone work. Student discount: 50% off Pro (~$12.50/month).
- **Bolt.new** runs entirely in the browser via StackBlitz's WebContainers — zero local setup, which eliminates the "works on my machine" problem in classrooms. Its free tier offers **1M tokens/month** (the most generous among competitors). It provides **free Bolt Pro for verified educators** and 50% off for students.
- **Vercel v0** produces the highest-quality React/Tailwind UI components but is frontend-focused. Its output is tightly coupled to the Next.js/Vercel ecosystem. Pricing starts at $20/month after a limited free tier of $5 in monthly credits.

| Tool | Category | Free tier | Flask compatibility | Best for |
|------|----------|-----------|-------------------|----------|
| Google Stitch | Design/wireframe | 350 gen/month | ★★★★★ (HTML+Tailwind) | Ideation, wireframing |
| Uizard | Design/wireframe | 3 gen/month | ★★★☆☆ | Sketch-to-wireframe |
| Figma Make | Design/wireframe | Free for students | ★★★☆☆ | Teams in Figma |
| Lovable | Full-stack builder | 5 credits/day | ★★☆☆☆ (React output) | Group MVPs |
| Bolt.new | Full-stack builder | 1M tokens/month | ★★☆☆☆ (JS-only backend) | Solo full-stack MVPs |
| v0 (Vercel) | UI components | $5/month credits | ★★★☆☆ (React output) | Clean component code |
| Replit Agent | Full-stack IDE | Limited free | ★★★★☆ (supports Python) | Python-native projects |

---

## Educators are already building curricula around these tools

Documented educational use cases are proliferating, though community college programming courses remain underrepresented in the literature. The most rigorous academic evidence comes from a **2025 peer-reviewed study at Universitat Politècnica de València** (published in *Applied Sciences*), where UX design students used generative AI with scaffolded prompt templates — instructor-provided skeleton prompts that students completed with project-specific details. This approach prevented aimless prompting while preserving student agency.

At the **Georgia Institute of Technology**, researchers presented at SIGCSE 2025 on integrating AI tools (including for UI design) into an advanced CS capstone course, emphasizing that students should possess foundational skills before engaging with AI so they can "critically engage with AI, recognizing biases and managing challenges like hallucinations." A companion study in the *International Journal of Artificial Intelligence in Education* found that AI tools work best for "co-creative tasks" — collaborative prompt exchanges between students for persona development and mockup assessment.

**Practical classroom examples** are compelling. At the University of Cebu, a third-year IT student used Uizard to complete a mobile agritech platform design in **12 hours** after failing with Figma, earning the highest grade in an entrepreneurship course. An educator named Jonathan Davis built **50+ custom learning tools** with Bolt.new for students with intellectual disabilities, replacing 6–7 hour PowerPoint lesson creation with rapid AI-generated interactive apps.

Several structured programs have emerged for classroom use. **Lovable partnered with imagi** to create an "Hour of AI" program providing full lesson plans, automatic student account setup (COPPA-compliant with anonymous accounts), and teacher training. **Figma offers free accounts** for verified students and educators with 3,000 AI credits monthly. **Bolt offers free Pro access for educators** and 50% student discounts. The **Designlab AI Prototyping Camp** (4-day workshop) teaches a workflow from prompt to working code using Figma Make and Lovable.

The documented creative-brief-to-prototype workflow that appears most in educational contexts follows this pattern:

1. Map the user flow (whiteboard, Miro, or paper)
2. Generate a master prompt describing the application
3. Create wireframes/visual concepts in Stitch or Uizard
4. Feed wireframes into Lovable or Bolt for a functional prototype
5. Test, iterate, and refine

A key finding from the **Nielsen Norman Group** reinforces the pedagogical value of these tools: AI design tools' "limited grasp of design nuances and inconsistent output make them best suited for **ideation, concept exploration, and early-phase prototype testing**, rather than later stages." This aligns perfectly with a capstone course workflow where students should learn to critically evaluate and refine AI output rather than accept it wholesale.

---

## Converting Stitch output to Flask templates requires a clear pipeline

No dedicated "AI wireframe → Flask" tool exists as of March 2026 — this is a genuine gap in the ecosystem, since most AI builders target the JavaScript/React stack. However, **Google Stitch's HTML + Tailwind CSS output is the most Flask-compatible** of any AI design tool, requiring relatively modest conversion effort.

The practical conversion pipeline has six steps:

**Step 1: Generate and export from Stitch.** Create the UI in Stitch, then use "View Code" to export an HTML/CSS/JS archive. The output uses semantic HTML with Tailwind CSS utility classes via CDN.

**Step 2: Restructure for Flask.** Move HTML files into `templates/` and static assets (CSS, JS, images) into `static/`. Create a `base.html` template extracting shared elements (head, navigation, footer).

**Step 3: Convert static paths.** This is the single most critical transformation. Replace all relative paths with Flask's `url_for()`:
```html
<!-- Before --> <link rel="stylesheet" href="styles.css">
<!-- After -->  <link rel="stylesheet" href="{{ url_for('static', filename='css/styles.css') }}">
```

**Step 4: Add Jinja2 template inheritance.** Wrap page-specific content in `{% extends "base.html" %}` and `{% block content %}` tags. Convert repeated components (navbars, cards, footers) into `{% include %}` partials.

**Step 5: Replace hardcoded content with Jinja2 variables.** Swap static text and lists with `{{ variable }}` expressions and `{% for item in items %}` loops. Convert forms to use Flask-WTF with CSRF protection.

**Step 6: Set up Tailwind CSS build pipeline.** Install Tailwind via npm, configure `tailwind.config.js` to scan Flask templates (`content: ["./templates/**/*.html"]`), and add a build command. The `flask-tailwindcss` PyPI package simplifies this to a single pip install.

For tools that output **React/JSX** (v0, Lovable, Bolt), conversion is harder. The most practical approaches are: using Next.js static export (`output: 'export'` in `next.config.js`) to generate plain HTML files, manually converting JSX to HTML (changing `className` to `class`, `{variable}` to `{{ variable }}`), or using an LLM like Claude to batch-convert components to Jinja2 templates.

**Code quality caveat**: A 2025 CodeRabbit analysis found AI-generated code produces **1.7x more issues** than human-written code. For Flask specifically, AI output commonly omits CSRF protection, uses hardcoded paths instead of `url_for()`, and lacks template inheritance patterns. Students should treat AI output as a starting point requiring security review and structural refinement — which itself is a valuable pedagogical exercise.

---

## Ideation and MVP prototyping call for different tools

The distinction between "blue sky" brainstorming and rapid MVP building maps cleanly onto different tools, and understanding this distinction helps structure a capstone course timeline.

**For open-ended ideation**, Google Stitch is the clear winner: it's free, generates multiple design variants per prompt, and the new "vibe design" mode lets students describe business objectives or desired user emotions rather than specific interface elements. Stitch's infinite canvas supports divergent thinking without financial pressure. Uizard complements this with sketch-to-wireframe conversion — students can whiteboard ideas, photograph them, and upload directly. **The ideation phase should stay at low or mid fidelity** to avoid over-investing in visual detail before validating concepts.

**For MVP prototyping**, Lovable and Bolt dominate. Lovable's strength is comprehensiveness — from a single conversation, it generates frontend, backend, database schema, authentication, and deployment. One practitioner described it as "the most complete environment for going from tiny prototype to serious app." Bolt's strength is speed and zero-setup — everything runs in the browser, making it ideal for hackathon-style sprints or classroom sessions with limited time.

A practical **two-phase framework** for capstone projects:

- **Weeks 1–3 (Ideation)**: Students use Stitch to explore 3–5 design directions from their project brief. Low cost (free), high exploration, multiple variants. Export best concepts to Figma for refinement or as visual reference.
- **Weeks 4–8+ (MVP)**: Students feed their chosen design direction into Lovable or Bolt to build a functional prototype. Connect to Supabase for real data. If the target is Flask, extract the HTML/Tailwind from Stitch directly and build the backend manually — which teaches more transferable skills than using an AI app builder's JavaScript-only stack.

Expert practitioner **Till Freitag** recommends: "v0 for individual components → Lovable for the overall app → Claude Code for production cleanup." For a Flask-focused course, a modified version would be: "Stitch for design exploration → Stitch HTML export for templates → manual Flask backend development."

---

## Practical realities of classroom adoption

**Cost is the least barrier.** Google Stitch is entirely free with 350 generations per month — more than sufficient for a semester-long capstone. Figma provides free education accounts. Bolt offers free Pro for educators and 50% student discounts. Lovable offers 50% student discounts and a structured classroom program through imagi Edu with COPPA-compliant anonymous accounts. A zero-cost stack (Stitch + Lovable free tier + Figma education) is viable, though daily credit limits on Lovable (5 credits/day) mean students cannot cram work into a single session.

**Collaboration varies dramatically across tools.** Lovable leads with up to **20 collaborators on the free plan** with real-time multi-user editing — ideal for group capstone projects. Replit has the best built-in classroom environment with embedded collaboration. Google Stitch, critically, **has no real-time collaboration features** — it remains a single-player tool, though students can share via Figma export. For team projects, the practical workaround is using Stitch individually for ideation, then converging in Lovable or a shared GitHub repository for implementation.

**Learning curve favors Uizard and Lovable** for programming students. Uizard is consistently described as the easiest tool for beginners, with drag-and-drop interaction requiring no code knowledge. Lovable is "the most forgiving for people who have never shipped code before," with automatic debugging and conversational interaction. Stitch is simple for design generation but offers limited depth. v0 has the steepest learning curve, as its output requires understanding React/Next.js to customize effectively.

Five gotchas deserve explicit instructor attention:

- **Daily credit limits force planning.** Lovable's 5 free credits/day and v0's $5/month budget mean students must spread work across sessions. Build this into the project timeline.
- **AI-generated code requires review.** Security analysis shows AI code contains more bugs and vulnerabilities than human-written code. Teaching students to audit AI output is both pedagogically valuable and practically necessary.
- **Google Stitch is experimental.** As a Google Labs product, it could be discontinued. Students should always export and back up their work locally.
- **Most builders are JavaScript-only.** Bolt explicitly states "Bolt only supports JavaScript-based backends" — PHP and Python are not compatible. For a Flask course, Stitch's HTML/CSS output is the bridge, not a full-stack builder.
- **Privacy policies vary.** On Lovable's free tier, student code may be used for model training. Bolt stores API secrets client-side. Advise students not to include real credentials in AI tool conversations.

## Conclusion

The AI UI design tool landscape has matured enough for meaningful classroom integration, but not enough for a single-tool workflow. **The most effective approach for a Flask-focused capstone combines Google Stitch for free, generative ideation with manual conversion to Jinja2 templates** — a workflow that teaches both modern AI-assisted design and foundational web development skills. The gap between AI-generated wireframes and production Flask templates is actually a pedagogical feature, not a bug: it forces students to understand HTML structure, template inheritance, and security patterns rather than accepting black-box output.

The critical insight from both academic research and practitioner experience is that these tools work best when introduced **after students have foundational skills** and when instructors provide **scaffolded prompting frameworks** rather than unconstrained access. The combination of free tools (Stitch, Figma Education, Lovable/Bolt student tiers) makes cost a non-issue. The real challenge is curriculum design — structuring project timelines around daily credit limits, requiring code review of AI output, and teaching students to use AI as a thinking partner rather than a replacement for understanding.