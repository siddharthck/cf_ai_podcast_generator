# AI Prompts Used in KT - AI-Powered Podcast Generator

This document contains all the AI prompts used during the development of this project.

## Project Setup & Architecture

### Initial Project Understanding
```
Look at the overview regarding the project. The problem is the Supabase edge function is using the Lovable API for completion which uses Gemini but we want to use OpenAI here. Can you do such changes? We don't need any call to Lovable.
```

**Result:** Migrated all edge functions from Lovable API (Gemini) to OpenAI API directly.

### Documentation Cleanup
```
Everything is working. Can you remove anything Lovable from the documentation?
```

**Result:** Removed all Lovable references from README.md and PROJECT_OVERVIEW.md, made documentation platform-agnostic.

## Edge Functions - OpenAI Integration

### Script Generation (generate-podcast)
**Prompt used in edge function:**
```
You are an expert podcast script writer. Create engaging, conversational podcast scripts with dialogue between a Host and an Interviewer.

CRITICAL FORMATTING RULES:
- Always use "Host:" and "Interviewer:" labels before each speaker's dialogue
- Write ONLY the spoken words - no stage directions, no sound effects, no music cues
- Never include text in parentheses like (pause), (laughs), (music fades)
- Never use asterisks or formatting markers like **bold** or *italic*
- Write natural, conversational dialogue that flows smoothly
- Use a ${style} style/tone

Write a ${duration} podcast script about: ${topic}

Format as a dialogue between Host and Interviewer with these rules:
1. Start each line with "Host:" or "Interviewer:"
2. Write ONLY spoken dialogue - no stage directions, parentheses, or formatting
3. Include:
   - Captivating introduction with both speakers
   - 3-4 key points explored through conversation
   - Natural back-and-forth dialogue
   - Engaging questions and answers
   - Memorable conclusion
```

**Purpose:** Generate natural, conversational podcast scripts using OpenAI GPT-4o-mini.

### Thumbnail Generation (generate-thumbnail)
**Prompt used in edge function:**
```
Create a professional podcast thumbnail image for a podcast titled "${title}" about ${topic}. 
Style: Modern, vibrant, eye-catching design with bold typography and relevant imagery. 
Include abstract shapes, gradients, and visual elements that represent the topic.
High quality, suitable for a podcast cover.
```

**Purpose:** Generate eye-catching podcast thumbnails using OpenAI DALL-E 3.

### Text-to-Speech (text-to-speech)
**Configuration:**
- Model: `tts-1`
- Voices: `alloy` (Host), `nova` (Interviewer)
- Format: `mp3`
- Processing: Text chunking at 4000 characters per request
- Output: Base64-encoded concatenated audio

**Purpose:** Convert podcast scripts to high-quality audio with distinct voices for each speaker.

## README Enhancement

### Project Description Update
```
The README is very basic. Can you add the information about the project?

The inspiration: 
Passive content consumption. Ever on a flight and you wanted to know about a topic that you want to listen to in terms of podcast, we have you covered. 

Just enter topic, the duration of podcast and the type of podcast, educational, conversational, storytelling etc. and you can generate the podcast quickly. And play it right away.

We have 3 agents:
- One is script writing
- Other is thumbnail creator
- Last one text to speech

These work together to generate end to end podcast.

Just sign up and enjoy.
```

**Result:** Created comprehensive README with inspiration, features, and clear value proposition.

### Live Demo Addition
```
Can you put live link in bold highlight at top of README?
https://cf-ai-podcast-generator.pages.dev/

Note: While signing up you might get email in the spam.
When podcast is generated wait few seconds when you hit generate audio button. Enjoy.
```

**Result:** Added prominent live demo link with user notes at the top of README.

### Architecture Diagram
```
Can you generate a simple mermaid architecture of the project?
```

**Result:** Created Mermaid diagram showing frontend, backend, AI agents, and OpenAI API integration flow.

```
Can you add the diagram in README along with some screenshots?
```

**Result:** Added architecture section with diagram and screenshots of key features.

## Key AI-Assisted Development Decisions

### 1. **Multi-Voice Audio Generation**
- Implemented speaker detection in scripts
- Assigned different voices (alloy for Host, nova for Interviewer)
- Created audio chunking system to handle long scripts

### 2. **Script Formatting Rules**
- Enforced clean dialogue format without stage directions
- Removed asterisks, parentheses, and formatting markers
- Ensured TTS-friendly text output

### 3. **Error Handling**
- Added rate limit handling (429 errors)
- Authentication error handling (401/402 errors)
- User-friendly error messages

### 4. **Image Generation**
- Used DALL-E 3 with 1792x1024 resolution
- Optimized prompts for podcast-style thumbnails
- Standard quality for faster generation

## Environment Configuration

### Required API Keys
```bash
# OpenAI API Key (for all AI agents)
OPENAI_API_KEY=your_openai_api_key

# Supabase Configuration
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_PUBLISHABLE_KEY=your_supabase_key
```

### Supabase Secrets Setup
```bash
supabase secrets set OPENAI_API_KEY=your_openai_key
```

## Deployment Commands

```bash
# Deploy individual edge functions
supabase functions deploy generate-podcast
supabase functions deploy text-to-speech
supabase functions deploy generate-thumbnail

# Or deploy all at once
supabase functions deploy
```

## Future AI Enhancement Ideas

1. **Custom Voice Cloning:** Allow users to upload voice samples for personalized narration
2. **Multi-Language Support:** Use OpenAI's translation capabilities for global reach
3. **Content Moderation:** Implement GPT-4 for content safety checks
4. **Smart Summarization:** Generate episode summaries automatically
5. **SEO Optimization:** AI-generated titles, descriptions, and tags
6. **Transcription:** Add whisper API for audio-to-text conversion
7. **Music Generation:** Integrate AI music generation for intro/outro
8. **Chapter Markers:** Auto-generate timestamps for key topics

## Credits

This project leverages:
- **OpenAI GPT-4o-mini** for natural language generation
- **OpenAI DALL-E 3** for image synthesis
- **OpenAI TTS (tts-1)** for speech synthesis
- **Supabase Edge Functions** for serverless deployment
- **GitHub Copilot** for AI-assisted code development
