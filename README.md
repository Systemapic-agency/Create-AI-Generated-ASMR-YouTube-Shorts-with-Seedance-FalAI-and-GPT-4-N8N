# Create-AI-Generated-ASMR-YouTube-Shorts-with-Seedance-FalAI-and-GPT-4-N8N

A fully automated **n8n AI ASMR Shorts pipeline** that generates viral ASMR ideas, expands them into structured production prompts, creates multiple short video clips using **Seedance**, generates synchronized **ASMR sound effects** using **Fal AI**, combines the clips into a final vertical short, uploads the result to **YouTube Shorts**, and sends completion notifications through **Telegram** and **Gmail**.

This workflow is ideal for creators, automation builders, and faceless content brands who want to automate **idea → scenes → clips → sound → final short → upload**.

---

## Features

- Automatically runs on a **schedule**
- Generates **viral ASMR short-form ideas**
- Creates AI-ready:
  - caption
  - idea
  - environment
  - sound prompt
- Stores production data in **Google Sheets**
- Expands one idea into **3 cinematic scene prompts**
- Generates multiple short clips using **Seedance**
- Adds AI-generated **ASMR sound design**
- Merges all clips into one final **9:16 vertical short**
- Uploads automatically to **YouTube**
- Sends notifications via:
  - Telegram
  - Gmail

---

## Workflow Overview

This workflow follows the process below:

1. A **Schedule Trigger** starts the automation
2. AI generates a short **viral ASMR concept**
3. A second AI agent expands it into a structured production plan
4. The workflow stores the idea in **Google Sheets**
5. A third AI agent creates **3 detailed scene prompts**
6. The scenes are extracted and sent one-by-one to **Seedance**
7. Three separate AI video clips are generated
8. Each clip gets ASMR-style audio generated with **Fal MMAudio**
9. All clips are combined into one final short video
10. The final video is uploaded to **YouTube Shorts**
11. The YouTube link is saved back to the sheet
12. Notifications are sent to Telegram and Gmail

---

## Main Use Case

This workflow is built for automated **ASMR YouTube Shorts production**.

It is ideal for channels focused on:

- kinetic sand
- slime
- soap cutting
- satisfying scooping
- oddly satisfying visuals
- loop-friendly ASMR content

---

## Nodes Used

### 1. Schedule Trigger
The workflow begins with a **Schedule Trigger**.

This allows the system to generate and publish new ASMR content automatically at regular intervals.

---

## AI Idea Generation Layer

### 2. AI Agent
This first AI agent generates **one short, trendy, viral ASMR idea**.

### Prompt behavior:
- Idea must be under **10 words**
- Output is plain text only
- Focused on satisfying short-form trends

### Example idea style:
```text
sparkly purple soap being scooped
```

This creates the seed concept for the rest of the pipeline.

---

### 3. OpenRouter Chat Model
Connected to the first AI agent to power initial idea generation.

---

### 4. OpenRouter Chat Model3
A second OpenRouter model also connected to the same ideation stage.

This suggests the workflow is using multiple LLM routes for better output reliability or fallback support.

---

## Structured Production Planning Layer

### 5. AI Agent1
This second AI agent expands the short ASMR idea into a full **production-ready JSON object**.

It generates:

- **Caption**
- **Idea**
- **Environment**
- **Sound**
- **Status**

### Rules include:
- viral-friendly caption
- exactly **12 hashtags**
- under-13-word idea
- under-20-word environment
- under-15-word sound prompt
- status always set to `"for production"`

### Example output structure:
```json
[
  {
    "Caption": "That crunch! 🤤 #kineticsand #satisfyingvideos #asmrsand #sand #oddlysatisfying #viral #fyp #explore #trending #tiktok #diy #crafts",
    "Idea": "Layered rainbow kinetic sand being sliced",
    "Environment": "Macro close-up on a clean, bright white surface, cinematic slow-motion",
    "Sound": "Crisp, crunchy slicing sounds with a soft, gentle hiss",
    "Status": "for production"
  }
]
```

This becomes the structured production blueprint.

---

### 6. OpenRouter Chat Model1
Provides the language model for the structured prompt expansion stage.

---

### 7. Structured Output Parser
Ensures the AI output is valid JSON before it moves into the rest of the workflow.

This helps avoid broken downstream automation.

---

### 8. Think
A helper reasoning tool used by the AI agents to review and improve generated outputs.

---

## Google Sheets Logging Layer

### 9. Append row in sheet
Saves the generated production plan into **Google Sheets**.

Stored columns include:

- `idea`
- `caption`
- `production_status`
- `youtube_url`

This sheet acts as a lightweight **content tracking database**.

---

## Scene Expansion Layer

### 10. AI Agent2
This third AI agent takes the structured ASMR concept and expands it into **3 detailed scene prompts** for video generation.

It uses:

- `Idea`
- `Environment`
- `Sound`

### It generates:
- **Scene 1**
- **Scene 2**
- **Scene 3**

Each scene is designed to be:

- visually grounded
- tactile
- macro-detailed
- tool-focused
- physically realistic
- optimized for satisfying ASMR-style content

### Scene generation rules include:
- 1,000 to 2,000 characters per scene
- tool must always be interacting with the object
- macro-level texture detail
- no fantasy or surreal abstraction
- highly visual, not narrative

This is the core **scene engineering layer** of the workflow.

---

### 11. OpenRouter Chat Model2
Provides the language model for the multi-scene expansion stage.

---

### 12. Code in JavaScript
This node extracts the generated scenes from the AI response.

### What it does:
- searches for keys like `Scene 1`, `Scene 2`, `Scene 3`
- converts them into individual items
- outputs them as separate `description` entries

This allows each scene to be sent individually into the clip generation pipeline.

---

## AI Clip Generation Layer

### 13. Create Clips
This node sends each scene to the **Seedance video generation API**.

### API used:
- `bytedance/seedance-v1-lite-t2v-480p`

### Request includes:
- aspect ratio: `9:16`
- duration: `10 seconds`
- combined prompt built from:
  - video theme
  - scene description
  - environment

This creates **3 separate short vertical AI clips**.

---

### 14. Wait
Waits after clip generation requests so the videos have time to render.

---

### 15. Get Clips
Fetches the final generated clip results from the Seedance prediction endpoint.

This retrieves the completed video clip URLs.

---

## AI Sound Generation Layer

### 16. Create Sounds
This node generates ASMR-style audio for each generated clip using **Fal MMAudio v2**.

### Request includes:
- prompt based on the generated **Sound** field
- duration: `10 seconds`
- the generated clip URL

### Example sound prompt style:
```text
ASMR Soothing sound effects. Crisp, crunchy slicing sounds with a soft, gentle hiss
```

This gives each scene a matching AI-generated sound layer.

---

### 17. Wait for Sounds
Waits while the audio generation jobs are being processed.

---

### 18. Get Sounds
Retrieves the completed sound generation results from Fal.

This completes the sound pipeline for the generated clips.

---

## Final Video Assembly Layer

### 19. Code in JavaScript1
This node collects all generated video URLs into one array.

### Output example:
```json
{
  "video_urls": [
    "clip1.mp4",
    "clip2.mp4",
    "clip3.mp4"
  ]
}
```

This prepares the clips for final sequencing.

---

### 20. Sequence Video
This node sends the generated clips into **Fal FFmpeg Compose API**.

### What it does:
- stitches 3 clips together
- creates a single continuous video
- places each clip at:
  - 0s
  - 10s
  - 20s

This results in one final **30-second ASMR short**.

---

### 21. Wait for Final Video
Waits for the final composed video to finish rendering.

---

### 22. Get Final Video
Retrieves the completed final video URL from the FFmpeg compose request.

This is the final publishable output.

---

## Production Tracking Layer

### 23. Update row in sheet
Updates the existing Google Sheets row after the final video is generated.

This helps track production completion and asset output.

---

## Upload Layer

### 24. Download Final Video
Downloads the final generated short video file from the returned final video URL.

This prepares the asset for direct upload to YouTube.

---

### 25. Upload to YouTube
Uploads the final ASMR short directly to **YouTube**.

### Upload configuration includes:
- **Title**:
```text
AI ASMR : {{ idea }}
```

- **Description**:
```text
AI-Generated Video Idea: {{ idea }}

This video was created automatically using our automated workflow #asmrai #asmr #n8n
```

- **Privacy**: `public`
- **Notify Subscribers**: `true`
- **Tags**:
  - asmr
  - viral
  - asmrai
  - n8n
  - automation

This makes the workflow a full **AI YouTube Shorts publishing system**.

---

## Notification Layer

### 26. Telegram Notification
Sends a Telegram message after the video is uploaded.

### Message includes:
- success confirmation
- generated video title
- YouTube watch link

This is useful for instant monitoring and creator alerts.

---

### 27. Gmail Notification
Sends an email notification once the video is live.

### Email includes:
- video title
- YouTube watch URL

This provides a second notification channel for publishing confirmation.

---

### 28. Update Sheet with Youtube Link
Updates the Google Sheet with the final published YouTube URL.

This closes the loop so the sheet becomes a full **production + publishing tracker**.

---

## AI Models Used

This workflow uses **OpenRouter-based models** for:

- viral idea generation
- structured ASMR production planning
- multi-scene prompt generation

### Model nodes:
- `OpenRouter Chat Model`
- `OpenRouter Chat Model1`
- `OpenRouter Chat Model2`
- `OpenRouter Chat Model3`

---

## Example Workflow Output

### Example generated idea:
```text
Layered rainbow kinetic sand being sliced
```

### Example structured output:
```json
{
  "Caption": "That crunch! 🤤 #kineticsand #satisfyingvideos #asmrsand #sand #oddlysatisfying #viral #fyp #explore #trending #tiktok #diy #crafts",
  "Idea": "Layered rainbow kinetic sand being sliced",
  "Environment": "Macro close-up on a clean, bright white surface, cinematic slow-motion",
  "Sound": "Crisp, crunchy slicing sounds with a soft, gentle hiss",
  "Status": "for production"
}
```

### Example final pipeline result:
- Idea generated
- Structured production plan created
- 3 clips generated
- ASMR sounds created
- Clips sequenced into one short
- Video uploaded to YouTube
- Telegram + Gmail notifications sent

---

## Requirements

To run this workflow successfully, you need:

- **n8n**
- **OpenRouter API credentials**
- Access to **Seedance / Wavespeed API**
- Access to **Fal AI APIs**
- **Google Sheets OAuth2 credentials**
- **YouTube OAuth2 credentials**
- **Telegram Bot credentials**
- **Gmail credentials**

---

## External Services Used

This workflow integrates with:

- **OpenRouter**
- **Seedance (Wavespeed)**
- **Fal AI MMAudio**
- **Fal AI FFmpeg Compose**
- **Google Sheets**
- **YouTube**
- **Telegram**
- **Gmail**

---

## Setup Instructions

1. Import the JSON workflow into n8n
2. Connect your **OpenRouter credentials**
3. Connect your **Google Sheets account**
4. Connect your **YouTube account**
5. Connect your **Telegram bot**
6. Connect your **Gmail account**
7. Replace any placeholder values such as:
   - `YOUR_CHAT_ID`
   - `your-email@gmail.com`
   - `YOUR_GOOGLE_SHEET_ID`
   - `YOUR_SHEET_NAME`
8. Confirm all API credentials for:
   - Seedance / Wavespeed
   - Fal AI
9. Activate the workflow
10. Run a test execution
11. Verify that:
   - ASMR idea generation works
   - structured output is valid
   - 3 scenes are generated
   - clips are rendered successfully
   - sounds are generated correctly
   - final video sequencing works
   - YouTube upload succeeds
   - notifications are sent

---

## Google Sheet Structure Used

The workflow expects a Google Sheet with columns similar to:

| Column | Purpose |
|---|---|
| idea | Short ASMR video idea |
| caption | Final social / video caption |
| production_status | Workflow production status |
| youtube_url | Final published YouTube URL |

---

## Important Notes

- The workflow is currently **inactive**
- The final short is designed for **9:16 vertical format**
- Each clip is generated at **10 seconds**
- Final video length is approximately **30 seconds**
- The workflow is highly optimized for **ASMR-style content pipelines**
- Some placeholders must be replaced before production use

---

## Limitations

At the moment, the workflow has a few production limitations:

- No approval step before YouTube publishing
- No thumbnail generation
- No subtitle / text overlay system
- No retry handling for failed renders
- No content moderation layer
- No analytics collection after publishing

---

## Suggested Improvements

You can improve this workflow further by adding:

- automatic thumbnail generation
- auto-generated Shorts titles and descriptions
- subtitle / caption burn-in
- YouTube analytics logging
- duplicate idea prevention
- content approval before publishing
- cloud storage backup
- multi-channel posting
- batch content queue generation
- AI voice whisper layers for ASMR enhancement

---

## Recommended Workflow Expansion

To turn this into a full **AI ASMR content engine**, pair it with:

1. thumbnail generator workflow
2. YouTube metadata optimizer
3. Shorts analytics tracker
4. TikTok / Reels cross-posting workflow
5. content performance dashboard

This can become a full **faceless ASMR automation system**.

---

## Example Real-World Flow

An automated ASMR channel wants to publish one new Short daily.

This workflow can:

1. generate a satisfying ASMR idea
2. turn it into structured production prompts
3. create 3 tactile scene clips
4. generate matching sound
5. combine them into a final short
6. upload directly to YouTube
7. notify the owner when it goes live

This creates a nearly **hands-free AI ASMR YouTube Shorts pipeline**.

---

## Author

Built as an **n8n AI ASMR Shorts automation workflow** for creators, automation builders, and faceless YouTube content systems.

---

## License

You can add your preferred license here, such as:

- MIT
- Apache-2.0
- Proprietary
