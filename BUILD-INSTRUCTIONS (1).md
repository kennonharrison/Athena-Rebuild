# SkillSpark GPT -- Build Instructions

## What You Need
- A ChatGPT Plus account ($20/month) -- you likely already have one
- 15-20 minutes

## Files in This Folder

1. **system-prompt-short.md** -- The condensed system prompt (~3.2 KB, fits 8K char limit). Paste into GPT builder "Instructions" field.
2. **system-prompt.md** -- Full-length reference version (DO NOT paste this -- too long for Instructions field)
3. **knowledge-file-1-scoring-rubric.md** -- Upload as knowledge file
4. **knowledge-file-2-behavioral-questions.md** -- Upload as knowledge file
5. **knowledge-file-3-career-matching.md** -- Upload as knowledge file
6. **knowledge-file-4-extended-instructions.md** -- Upload as knowledge file (contains detailed flow, output formatting, subskill definitions that were moved out of the system prompt to fit the 8K limit)

## Step-by-Step

### 1. Open GPT Builder
Go to: https://chat.openai.com/gpts/editor
Click "Create a GPT" (or "Create" in the sidebar)

### 2. Switch to "Configure" Tab
Don't use the conversational builder. Click the "Configure" tab at the top for direct control.

### 3. Fill In the Fields

**Name:** SkillSpark

**Description:** Discover what you're great at and find careers that actually fit -- based on real skills, not just degrees or job titles. Free. 5 minutes. No signup required.

**Instructions:** Copy the ENTIRE contents of `system-prompt-short.md` and paste here. (This is the condensed version that fits the 8K character limit. The detailed flow and subskill definitions are in Knowledge File 4.)

**Conversation starters** (suggested opening messages users can click):
- "Let's find out what I'm great at"
- "I have a resume to upload"
- "Surprise me with careers I haven't considered"
- "I'm a student trying to figure out my path"

**Knowledge:** Upload these four files:
- knowledge-file-1-scoring-rubric.md
- knowledge-file-2-behavioral-questions.md
- knowledge-file-3-career-matching.md
- knowledge-file-4-extended-instructions.md

**Capabilities:**
- [x] Web Browsing (OFF -- not needed, keeps it fast)
- [x] DALL-E Image Generation (OFF)
- [x] Code Interpreter (OFF)
- [x] File Upload (ON -- needed for resume uploads)

**Actions:** None needed for MVP. (Phase 2 could add O*NET API for dynamic career lookup)

### 4. Test It
Use the Preview pane on the right to run through both paths:
- Test Path A: Upload a sample resume, answer follow-ups, check the Skillprint and matches
- Test Path B: Choose "just talk," answer all questions, check results
- Test with a Richmond ZIP code (e.g., 23220) to confirm local results trigger

### 5. Publish
Click "Save" → Choose visibility:
- **"Anyone with a link"** for initial testing with specific people
- **"Everyone"** when ready for public GPT store listing

### 6. Share
The GPT gets a unique URL like: https://chat.openai.com/g/g-xxxxxxxxxxxx-skillspark
Share this link directly. Anyone with a free or paid ChatGPT account can use it.

---

## What to Test For

1. **Does it feel like a conversation, not a questionnaire?** The questions should flow naturally.
2. **Are the Skillprint scores reasonable?** Try it yourself -- do the scores feel right for you?
3. **Are the career matches surprising and useful?** At least one match should make the user think "I never considered that."
4. **Does the Richmond enhancement trigger?** Enter a 230xx ZIP and check for local employer mentions.
5. **Is the tone right?** Warm, direct, encouraging. Not corporate. Not condescending.
6. **Does the resume path work?** Upload a real resume and check if extraction is reasonable.

## Iteration

After testing, the most likely things to tune:
- **Scoring rubrics** -- adjust thresholds if scores feel too high/low across the board
- **Career profiles** -- add more careers, adjust subskill requirements based on real-world feedback
- **Question wording** -- refine scenarios that consistently produce vague answers
- **Salary data** -- update with more current/precise Richmond numbers

All changes are just edits to the knowledge files or system prompt. Re-upload and the GPT updates instantly.

---

## Phase 2 Enhancements (After MVP)

- **O*NET API Action:** Live lookup of any of 923 occupations instead of static top 50
- **BLS API Action:** Real-time salary data by metro area
- **Shareable results card:** Generate an image summary users can share
- **Analytics:** Track completion rates, most common career matches, geographic distribution
- **Futurenav Edge deep link:** Direct integration with Edge assessment signup

---

_Created: 2026-02-27_
