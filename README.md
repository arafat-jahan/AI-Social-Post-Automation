# AI Agent Workflow Automation (n8n + Human-in-the-Loop)

This project demonstrates a production-ready AI automation workflow built on **n8n**. It manages, composites, and schedules consistent, brand-aligned social media posts with a mandatory human approval step before publishing.

## 🚀 Workflow Overview
1.  **Input Ingestion:** Accepts user input (Topic and Date) via an n8n Form.
2.  **Content Generation:** Uses **Groq (Llama 3.3 70B)** to generate a post (90-150 words).
3.  **Image Compositing:** Programmatically generates a branded image card using **HTML/CSS-to-Image**.
4.  **Human-in-the-Loop (HITL):** Sends an email via **Gmail** with the content and an approval link.
5.  **Publishing:** Upon approval, the post is published to **Facebook** and **Instagram** mock endpoints (Webhook.site).

---

## 🧠 Must Explain: Technical Implementation

### 1. Prompt Engineering Strategy (Word Count Constraint)
To strictly enforce the **90–150 word count** without truncation, I utilized a "Strict Constraint Persona" strategy. 
- **Persona:** Defined the LLM as a "Professional Social Media Manager."
- **Logic:** Instead of a vague instruction, I provided a "Hard Rule" within the system prompt: *"The post MUST be strictly between 90 and 150 words. This is a critical legal/brand requirement."*
- **Formatting:** I instructed the model to exclude introductory text and word-count tags, ensuring the raw output is ready for use while maintaining the exact length.

### 2. Technical Image Pipeline (Brand Consistency)
To maintain brand guidelines, I avoided unpredictable AI image generation. Instead, I used the **HTML/CSS-to-Image** node to render a programmatic template:
- **Static Logo Placement:** I used CSS `position: absolute; top: 48px; right: 52px;` to fix the **NEXUS AI** logo in the top-right corner, ensuring it never moves regardless of the content.
- **Lower-Middle Typography:** The dynamic post title is positioned using a centered flex-box layout at the lower-middle part of the card.
- **Scalability:** I implemented a JavaScript auto-scaling function within the HTML to ensure the text fits perfectly within the card boundaries without overflow.

### 3. Human-in-the-Loop Architecture (Pause & Resume)
The approval mechanism is architected using n8n's asynchronous **Wait (On Webhook Call)** node:
- **Notification:** A Gmail node triggers first, sending the post text, image, and a unique `$execution.resumeUrl` to the reviewer.
- **Execution Pause:** The workflow enters a "Waiting" state, effectively halting the process.
- **Secure Response:** Final publishing nodes (Facebook/Instagram Mocks) are only triggered once the human reviewer clicks the resume link in their email, which sends a POST request back to the n8n webhook.

---

## 🛠 Tech Stack
- **Orchestration:** n8n
- **LLM:** Groq (Llama-3.3-70b-versatile)
- **Image Generation:** HTML/CSS to Image API
- **Communication:** Gmail API
- **Mock Publishing:** Webhook.site

## 📁 Deliverables
- **Workflow File:** `workflow.json` (Available in the root directory)
- **Demo Video:** [Click here to watch the end-to-end demo](YOUR_VIDEO_LINK_HERE)

---
*Created as part of the AI Automation Workflow Assessment.*
