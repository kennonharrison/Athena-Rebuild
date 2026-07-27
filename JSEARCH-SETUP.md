# Adding Job Search to SkillSpark GPT

## Step 1: Get a RapidAPI Key (2 minutes)

1. Go to https://rapidapi.com/letscrape-6bRBa3QguO5/api/jsearch
2. Sign up for RapidAPI (free)
3. Subscribe to JSearch -- select the **Free** plan (500 requests/month)
4. Copy your **X-RapidAPI-Key** from the code snippets panel on the right side

## Step 2: Add the Action to Your GPT (3 minutes)

1. Open your SkillSpark GPT in the editor: https://chat.openai.com/gpts/editor
2. Scroll down to **Actions** section
3. Click **Create new action**
4. In the **Schema** box, paste the entire contents of `jsearch-openapi-schema.json`
5. Under **Authentication**, select:
   - Authentication Type: **API Key**
   - API Key: paste your X-RapidAPI-Key
   - Auth Type: **Custom**
   - Custom Header Name: **X-RapidAPI-Key**
6. You also need a second header. Under the schema, in the **Headers** section (if available), add:
   - **X-RapidAPI-Host**: `jsearch.p.rapidapi.com`

   If there's no separate headers field, add this to the Authentication section or include it as a server variable. The GPT builder may handle this differently -- if you hit issues, let me know and I'll troubleshoot.

7. Click **Save**

## Step 3: Update the System Prompt

Add this to the end of the Instructions field (the system-prompt-short.md content):

```
## JOB SEARCH INTEGRATION
After presenting career matches (Step 4), offer: "Want to see real job openings for any of these?"

When the user says yes or picks a career:
1. Call searchJobs with query = "[Career Title] in [City from their ZIP], [State]" and date_posted = "month"
2. Present the top 5 results in this format:

"Here are open [Career Title] positions near you:

**1. [job_title]** at [employer_name]
   📍 [job_city], [job_state] | [job_employment_type]
   💰 [salary if available, otherwise "Salary not listed"]
   🔗 [Apply here](job_apply_link)

**2. [job_title]** at [employer_name]
..."

3. If no results, try broadening: remove city, use just state, or try a related job title.
4. Always say: "These are live listings -- they may change. Apply soon if one catches your eye."
5. Conserve API calls: only search when the user explicitly asks. Don't auto-search for all 5 matches.
```

## Step 4: Test It

1. Run through the full SkillSpark flow
2. When career matches appear, say "Show me jobs for #1"
3. Verify real listings appear with apply links
4. Test with a Richmond ZIP code
5. Test with a non-Richmond ZIP code

## Cost

- Free tier: 500 API calls/month (supports ~1,500 users)
- Basic tier ($30/month): 10,000 calls (supports ~30,000 users)
- You won't hit the free tier limit for a while

## Troubleshooting

**"Authentication failed":** Double-check your X-RapidAPI-Key is correct and the header name is exact.

**"No results found":** The query format matters. "Data Center Technician in Richmond, VA" works better than just "Data Center Technician." If a specific title returns nothing, the GPT should try broader terms.

**Rate limit errors:** You've hit the free tier cap. Upgrade to Basic ($30/month) or wait until the monthly reset.

**GPT doesn't call the API:** Make sure the system prompt addition is saved. The GPT needs explicit instructions on WHEN to call the action.

---

_Created: 2026-02-28_
