# AI Video Generation Agent Workflow 🎬🤖

An end-to-end automated pipeline built in **n8n** that monitors incoming video ideas via form submission, analyzes and scripts them into highly detailed scenes using OpenAI, converts descriptions into technical prompts, and programmatically triggers the cutting-edge **Veo 3 AI** model to generate and log professional videos automatically.

---

## 💼 Business Case: Problem & Solution

### The Problem
Modern content creators, marketing agencies, and media companies face an overwhelming bottleneck when producing video content. Manually brainstorming concepts, scripting frame-by-frame scenes, converting ideas into optimized AI prompts, and constantly monitoring render progress leads to:
- **High Production Latency:** Delayed workflows due to manual prompt crafting and constant back-and-forth toggling between tools.
- **Inconsistent Quality:** Non-standardized prompt structures resulting in unpredictable AI video outputs.
- **Scalability Issues:** Scaling video content for multi-channel platforms becomes highly expensive and operationally exhausting when relying purely on manual rendering.
- **Disorganized Asset Tracking:** No automated database to back up, categorize, or track generated video URLs and historical prompts.

### The Solution (This Workflow)
This workflow fully automates the video production pipeline, reducing manual effort and helping teams create video content faster and more consistently.

**Key Benefits:**
- Saves hours of manual scripting and prompt creation
- Speeds up video production workflows
- Produces consistent AI-generated outputs
- Scales content creation with minimal human intervention
- Automatically tracks generated videos in Google Sheets
- Reduces operational workload for content teams

---

## 🛠️ Tech Stack & Nodes Used

- **Automation Platform:** n8n
- **AI Models:** OpenAI Chat Models (Combined with Structured Output Parsers)
- **Video Generation API:** Veo 3 AI Engine (REST API via HTTP Requests)
- **Databases/Sheets:** Google Sheets
- **Core n8n Nodes:** On form submission, AI Agent, HTTP Request, Wait, Switch, Merge, Append row in sheet

---

## 📈 Workflow Architecture & End-to-End Explanation

Here is exactly how the data flows through the pipeline from start to finish:

### 1. Ingestion & Multi-Agent Scripting
- **On form submission:** The workflow kicks off the moment a user inputs a raw video seed idea or theme into a front-facing form.
- **AI Agent (Scripting):** The raw idea lands into the first OpenAI-powered agent. Backed by a **Structured Output Parser**, it analyzes the theme and dynamically generates two highly detailed, distinct narrative scenes.
- **AI Agent 1 (Prompt Engineering):** The structured scenes are instantly passed to the second AI Agent. This agent acts as a prompt specialist, transforming the abstract scenes into heavily optimized, visually descriptive prompt text optimized for AI video rendering.

### 2. Dual-Processing Parallel Pipelines
To accommodate multiple scenes simultaneously without processing queues, the workflow splits into two identical execution branches running in parallel:
- **HTTP Request (POST):** Triggers an API call to the **Veo 3 Video Generation API**, sending the newly engineered prompt to begin the rendering process.
- **Wait Node (The Buffer):** The execution pauses for **10 seconds** per branch. This critical delay protects the system from rate limits and allows the engine time to process the media assets.
- **HTTP Request 1 (GET):** The system polls the Veo 3 server to fetch the real-time status and retrieval link of the generated video file.

### 3. Smart Routing & Output Management
A central **Switch Node** evaluates the API status response and branches into four dynamic paths:
- **In-Progress / Processing:** Safely routes the workflow back to the Wait node to allow more render time, eliminating runtime script errors.
- **Success:** Extracts the hosting video URL and passes it down the line.
- **Failed:** Gracefully halts execution at a terminal node if an API error occurs. *(Note: For production environments, this route is perfectly positioned to connect directly to WhatsApp or Telegram nodes for instant admin failure alerts).*

### 4. Consolidated Database Logging
- **Merge Node:** The independent parallel video paths are unified back into a structured dataset.
- **Append row in sheet:** The final generated video URLs, along with their matching prompt descriptions, are logged sequentially into dedicated rows inside **Google Sheets** for immediate deployment and download.

---

## 🚀 Scalability Notice
While this base layout is structured to output a **2-scene sequence (approx. 8 to 10 seconds of runtime)**, the underlying logic is infinitely scalable. Because of its modular parallel architecture, you can replicate the branching patterns to generate 10+ scenes seamlessly—effectively orchestrating a fully automated 1-to-2 minute cinematic video from a single text prompt.

---

## 📸 Workflow Assets

Access the structural resources of this repository through the links below:

- [View Workflow Architecture Blueprint (Screenshot)](./Screenshot%20ai%20video%20generation%20agent_workflow.png)
- [Download Workflow JSON File (n8n Blueprint)](./ai-_video_generation_agent_workflow.json)

---

## ⚙️ Setup & Installation

### Prerequisites
- Install and set up an **n8n** instance (Self-hosted or Cloud).
- An active **OpenAI API Key**.
- A **Veo 3 API** account with valid endpoint authorization tokens.
- Google account permissions synced with n8n for **Google Sheets** access.

### Importing the Workflow
1. Download the `ai-_video_generation_agent_workflow.json` blueprint file from this repository.
2. Open your n8n dashboard and click on **New Workflow**.
3. Click on the top-right menu, select **Import from File**, and upload the saved JSON file.

### Credential Configuration
- Open the OpenAI Chat Model nodes and connect your **OpenAI API credentials**.
- Access the HTTP Request nodes and insert your authorization tokens for the **Veo 3 API**.
- Authenticate and link your specific spreadsheet inside the **Google Sheets** nodes.

### Test & Deploy
1. Click **Execute workflow** at the bottom of the canvas to run a test submission.
2. Verify that the video URLs land successfully in your Google Sheet rows.
3. Once fully verified, toggle the **Active** switch in the top right corner to put the production pipeline live!

---

## 📜 License
This project is open-source and available for learning, portfolio display, and commercial scaling purposes.

---

 ## Author
 
Sharjeel.ai: 
AI Automation Specialist
Services:
- AI Agent Development
- n8n Automation
- Make.com Automation
- OpenAI Integrations
- Workflow Optimization
