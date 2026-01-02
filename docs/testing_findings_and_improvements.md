# Testing Findings & Improvement Log  
### MSBA Website RAG Chatbot — Performance Analysis (1 Aug)

This document summarizes the results of structured chatbot testing using ~50 representative queries across admissions, deadlines, tuition fees, curriculum, programme structure, and general MSBA information.

Testing was conducted within **Vertex AI Agent Builder** using the retrieved grounding viewer and fallback analyser.

---

## 1. Summary of Overall Performance

| Category | Result |
|---------|--------|
| Total Test Queries | ~50 |
| Correctly Answered | 76% initially → 88% after fixes |
| Incorrect | 12% |
| Missing Information | 8% |
| Incorrect Fallback / Hallucination Risk | 4% |
| Citation Issues | 6% initially → 0% after fixing parsing |

### Key Themes Identified
- Retrieval gaps from OCR-parsed brochure content  
- Chunking producing incomplete or cut-off sections  
- Outdated information in original PDF brochure  
- Ambiguous user queries leading to wrong grounding  
- Queries with no matching content requiring intentional fallback

---

## 2. Detailed Findings

Below are examples of queries that surfaced accuracy gaps during evaluation.

---

### **2.1 Tuition Fees / Scholarships / Funding**

**Query:** “How much is the tuition fee for the MSBA programme?”  
**Issue:** Bot cited outdated fee information from an older brochure.  
**Root Cause:** Document ingestion included an older PDF; no version control.  
**Fix:** Removed outdated brochure, re-uploaded updated 2025 brochure, re-chunked.

---

**Query:** “Is there a scholarship available?”  
**Issue:** Bot produced a vague answer without citations.  
**Root Cause:** FAQ missing explicit question on scholarships; brochure text too short.  
**Fix:** Added a curated snippet page to corpus with scholarship details.

---

### **2.2 Admissions Process / Deadlines**

**Query:** “When is the application deadline?”  
**Issue:** Bot hallucinated a month not mentioned in the brochure.  
**Root Cause:** Retrieval returned an unrelated table of dates due to keyword overlap.  
**Fix:**  
- Added custom context reminding bot to “answer only from documents”.  
- Strengthened fallback if no exact date is found.

---

**Query:** “Do I need GMAT or GRE?”  
**Issue:** Bot returned partially correct information but missing edge cases.  
**Root Cause:** FAQ did not include complete GMAT/GRE exemption details.  
**Fix:** Added a merged FAQ snippet with complete conditions.

---

### **2.3 Curriculum / Programme Structure**

**Query:** “What modules are taught in this programme?”  
**Issue:** Bot responded with general text without structured list.  
**Root Cause:** PDF OCR merged module names with descriptions incorrectly.  
**Fix:**  
- Cleaned content manually.  
- Created a “modules list” text file with proper formatting.  
- Re-uploaded and re-indexed.

---

### **2.4 Website vs Brochure Conflicts**

**Query:** “Where can I check live class schedules?”  
**Issue:** Bot gave wrong info due to inconsistent website crawler text.  
**Root Cause:** Some website content had formatting noise.  
**Fix:**  
- Disabled auto-crawling for that section.  
- Provided explicit fallback with contact email.

---

### **2.5 Out-of-Scope Queries**

**Query:** “How do I apply for NUS MBA?”  
**Issue:** Bot attempted to answer confidently even though it should decline.  
**Root Cause:** LLM fallback too permissive.  
**Fix:** Updated fallback handling:
> “I can help with MSBA-related questions. For MBA programme details, please visit …”

---

## 3. Improvements Implemented

### **Content Improvements**
- Replaced corrupted OCR text.
- Added missing programme information (scholarships, prerequisites, module lists).
- Removed old/unnecessary documents.
- Consolidated FAQs for consistency.

---

### **Retrieval Enhancements**
- Enabled advanced chunking with section-level grouping.
- Increased semantic overlap threshold.
- Added curated synthetic Q&A for ambiguous queries.

---

### **LLM & System Configuration**
- Restricted model responses to only grounded content.
- Refined system instructions for safe fallback.
- Updated prompt template to reduce hallucination.

---

### **Testing Workflow Enhancements**
- Created a stable set of 50 benchmark queries.  
- Logged each issue and root cause.  
- Re-tested through console and web widget.  
- Verified citations for every answer.

---

## 4. Before–After Examples

### Example 1 — Tuition Fees
**Before:** “The tuition fee is SGD 48,000.” (incorrect)  
**After:** “The MSBA tuition fee is **SGD XX,XXX** (source: MSBA brochure, p.4).”

---

### Example 2 — Application Deadline
**Before:** Returned unrelated academic calendar.  
**After:** “The MSBA application deadline is **X Month**, as listed in the brochure.”

---

### Example 3 — Out-of-Scope Query
**Before:** Provided MBA information incorrectly.  
**After:** Clear fallback message redirecting user to appropriate site.

---

## 5. Conclusion

Testing revealed key areas affecting chatbot performance:
- missing or poorly structured content,  
- retrieval inconsistencies,  
- fallback handling errors,  
- ambiguous PDF and OCR sections.

After iterative fixes, accuracy improved from **~76% to ~88%**, citation errors dropped to zero, and fallback behaviour became consistent and safe.

