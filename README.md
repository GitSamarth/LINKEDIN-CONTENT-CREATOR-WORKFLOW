# 💼 LinkedIn Content Creation Workflow – n8n Automation

This workflow automates the process of **researching topics, generating LinkedIn posts, and updating them back to a Google Sheet**.  
It’s perfect for content creators who want to streamline research, AI-powered writing, and post scheduling.

---

## 🚀 Overview

- **Input**: A Google Sheet containing topics to write about.
- **Research**: Uses Tavily API to fetch 3 relevant articles for the topic.
- **Content Creation**: AI Agent synthesizes the research into a professional LinkedIn post.
- **Output**: Post content is written back into the Google Sheet with a “Created” status.

---

## ✨ Features

- Automatically pulls the next topic from a Google Sheet.
- Conducts **web research** using Tavily API.
- Generates **engaging LinkedIn posts** with OpenRouter LLM.
- Updates the original Google Sheet with completed posts.
- Can run on a **schedule** or be manually triggered.

---

## 📋 Prerequisites

- **Basic knowledge of n8n** workflows and nodes.
- Google Sheets API access (for read/write).
- Tavily API key (for research).
- OpenRouter API key (for AI content creation).
- A Google Sheet with the following columns:
  - **Topic**
  - **Content**
  - **Status**

Example Google Sheet format:

| Topic                   | Content   | Status   |
|-------------------------|-----------|----------|
| Future of AI in Hiring  |           | Pending  |
| Sustainability in Tech  |           | Pending  |

---

## 🛠️ How It Works

### 1. **Trigger Node**
- **Manual Trigger** (for testing) or **Schedule Node** (for automation).
- Schedule node can be set to run daily, weekly, or at a specific time.

### 2. **Google Sheet – Get Row**
- Set the:
  - **File Name** (Google Sheet name)
  - **Sheet Name**
  - **Column** (e.g., `Status`)
  - **Value** to filter (e.g., `Pending`)
- In options, enable **Only First Matching Row** to pull one topic at a time.

### 3. **Tavily API – HTTP Request**
- Method: **POST**
- Endpoint: `https://api.tavily.com/post/search`
- Import the `curl` request from Tavily documentation.
- Under Headers → `Authorization`:

-Bearer <API_KEY>

*(Ensure there is a space after `Bearer`.)*
- Change the request body:
- Replace `query` with `search the web with topic ({{ $json["Topic"] }})`
- Set `"max_results": 3` instead of 1.
- Output: Returns 3 URLs relevant to the topic.

### 4. **AI Agent – LinkedIn Post Writer**
- **User Prompt**:
- Article1: {{ $json["article1"] }}
-Article2: {{ $json["article2"] }}
-Article3: {{ $json["article3"] }}

- **System Prompt**:
-You are a professional content creator agent specializing in writing concise, inspiring, and engaging LinkedIn posts based on input articles.

-OBJECTIVE:
Read 3 provided articles and synthesize them into one single concise LinkedIn post that:

Highlights the core message or trend connecting the articles.
Reflects a professional yet inspiring tone suitable for LinkedIn.
Sparks thought, optimism, or motivation in the professional audience.

STYLE & FORMAT REQUIREMENTS:
Length: 3–5 short paragraphs (ideally 600–900 characters total).
Tone: Inspiring, insightful, and professional.
Voice: Human-like, thoughtful, and forward-looking.

Structure:
Start with a hook or powerful insight.
Summarize the theme or connection between the articles.
End with a reflective question, takeaway, or call to action.

ENHANCEMENTS:
Include relevant emojis to boost engagement (max 5–6).
Use relevant LinkedIn-style hashtags (4–6), e.g., #Innovation #AI #Leadership #FutureOfWork #Sustainability.

RULES:
Do not mention the original article titles or sources.
Avoid sounding like a news headline or academic abstract.
Don’t write in bullet points—this should feel like a mini thought-leadership post.


- Connect to **OpenRouter Chat Model** for content generation.

### 5. **Google Sheet – Update Row**
- Use the same:
- **File Name**
- **Sheet Name**
- **Content** column → AI Agent’s output.
- **Status** column → `Created`.

---

## ⚡ Getting Started

1. Create your Google Sheet and add the topics.
2. Get your API keys for:
 - Tavily
 - OpenRouter
 - Google Sheets API
3. Import the n8n workflow template.
4. Configure the Google Sheet node with your file and sheet names.
5. Replace the Tavily API key in the HTTP Request node.
6. Activate the workflow.

---

## 💬 Example Flow

1. Topic: *The Role of AI in Sustainable Energy* is in Google Sheet with status `Pending`.
2. Workflow runs → Fetches 3 related articles via Tavily API.
3. AI Agent writes an engaging post with hashtags and emojis.
4. Google Sheet updates:
 - **Content**: AI-generated LinkedIn post.
 - **Status**: `Created`.

---

## 📌 Notes

- Change the trigger from **Manual** to **Schedule** for automation.
- Tavily query can be adjusted to refine search results.
- Post content can be reviewed before posting to LinkedIn.
