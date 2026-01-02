# Chatbot Testing Guide (Vertex AI Agent Builder)

This document outlines the step-by-step process for testing the MSBA Website Chatbot built using **Google Vertex AI Agent Builder**. These instructions were used during the evaluation phase of the capstone project to validate conversation accuracy, grounding, citations, and fallback behaviour.

---

## 1. Accessing the Chatbot for Testing

### **Option A — Using Vertex AI Agent Builder Console**
1. Log in to **Google Cloud Console**.
2. Navigate to **Vertex AI → Agent Builder**.
3. Open the agent titled **“MSBA Virtual Assistant”**.
4. Click **Test Agent** in the top-right corner.
5. The testing window opens, allowing you to simulate user queries and review:
   - grounding documents,
   - retrieved chunks,
   - model responses,
   - fallback triggers.

---

## 2. Preparing for Test Runs

For consistent evaluation:

- Use a **fixed list of representative queries** (admissions, fees, deadlines, curriculum, programme structure, scholarships, contact details, etc.)
- Ensure that:
  - the knowledge corpus is updated,
  - chunking is enabled,
  - citation mode is turned on,
  - fallbacks are configured.

---

## 3. Running Test Queries

1. Type the query into the **Test Agent** panel.
2. Observe the bot’s response.
3. Click **View grounding** to see:
   - retrieved text chunks,
   - document source,
   - matching score,
   - missing / partial recall.
4. Evaluate:
   - **Accuracy** — Is the answer factually correct?
   - **Grounding** — Is the response based on the brochure, FAQ, or scraped content?
   - **Citation** — Are correct sources shown?
   - **Clarity** — Is the response easy to understand?
   - **Scope Control** — Does the bot avoid hallucinating or answering unrelated questions?

---

## 4. Evaluating Fallback Behaviour

Fallback is triggered when:

- no relevant document is retrieved,
- the query is out-of-scope,
- the system cannot produce a reliable answer.

Evaluation criteria:
- Does the fallback message provide a **polite, helpful alternative**?
- If contact email is provided, does it match the configured content?
- For non-MSBA queries, does the bot avoid hallucination and remain in-scope?

---

## 5. Logging Issues During Testing

For each query, log the following:

- **User Query**  
- **Bot Response**  
- **Expected Response / Ground Truth**  
- **Issue Type** (Incorrect answer, missing citation, wrong fallback, outdated content, etc.)
- **Root Cause** (e.g., brochure missing a section, PDF OCR issues, chunking errors)
- **Fix Implemented** or proposed action

Use a table structure for clarity.

---

## 6. Iterating and Re-testing

After making configuration changes:

1. Re-run the same set of queries.
2. Compare before/after responses.
3. Confirm whether:
   - retrieval quality improved,
   - citations appear correctly,
   - fallback is more consistent,
   - ambiguity is reduced.

Repeat iterations until performance stabilizes.

---

## 7. Testing on the Embedded Website Widget (Optional)

When integrating the chatbot into the MSBA website (via Messenger widget):

1. Navigate to the test webpage with the embedded script.
2. Open the widget and submit the same queries used in console testing.
3. Check:
   - response time,
   - formatting,
   - link rendering,
   - user experience,
   - widget visibility and scrolling.

---

## 8. Conclusion

This testing guide ensured a structured approach to evaluating the chatbot’s:
- accuracy,
- reliability,
- retrieval grounding,
- user safety,
- and overall conversational quality.

Testing played a key role in identifying missing FAQ content, OCR parsing errors, outdated brochure sections, and optimal fallback configurations.

