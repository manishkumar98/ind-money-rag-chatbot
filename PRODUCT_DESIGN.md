# Product Design: RAG-Based Mutual Fund FAQ Chatbot (M1)

## Project: INDMoney RAG Chatbot
**Product:** INDMoney  
**Milestone:** M1 (Milestone 1)  
**Due Date:** Mar 8, 11:59:00 PM (Asia/Calcutta)  
**Repository:** [manishkumar98/ind-money-rag-chatbot](https://github.com/manishkumar98/ind-money-rag-chatbot)

---

## 1. PRODUCT VISION

**Facts-Only MF Assistant** is a RAG-based chatbot that answers factual questions about mutual fund schemes using verified sources from AMC, SEBI, and AMFI websites. The assistant provides concise, citation-backed responses while strictly avoiding any investment advice or performance claims. It's designed for retail investors comparing schemes and support/content teams answering repetitive MF questions.

**Who it helps:**
- 🎯 **Retail investors** — Quick factual answers about mutual fund schemes without sales pitch
- 🎯 **Support/content teams** — Consistent, sourced answers to repetitive questions
- 🎯 **Financial advisors** — Educational materials to share with clients
- 🎯 **Compliance teams** — Standardized, fact-checked responses with audit trail

---

## 2. PRIMARY USERS

| User | Goal | Pain Point | Solution |
|------|------|-----------|----------|
| **Retail Investor** | Compare funds; understand scheme details | Scattered info across multiple sites | One place for all factual scheme info with citations |
| **Support Agent** | Answer "What is exit load?" 10x per day | Repetitive, time-consuming | Pre-generated, sourced answer ready to copy |
| **Content Team** | Create educational materials | Hard to verify facts | RAG ensures every claim is sourced + cited |
| **Compliance Officer** | Ensure no investment advice given | Risk of chatbot giving tips | Strict refusal flows + audit trail of all responses |

---

## 3. KEY PAIN POINTS

**For Retail Investors:**
- ❌ Information scattered across 10+ websites (AMC, SEBI, AMFI, fund website, etc.)
- ❌ Hard to find factual details (expense ratio, exit load, lock-in period)
- ❌ Risk of outdated info from blogs/forums (not official sources)
- ❌ No single source of truth; different sources say different things
- ❌ Ads and sales pitch mixed with factual info

**For Support/Content Teams:**
- ❌ Same questions asked 100+ times per week
- ❌ Manual research for each answer wastes time
- ❌ Responses may be inconsistent (different agent, different answer)
- ❌ No audit trail of what was told to customers
- ❌ Hard to keep responses up-to-date as scheme rules change

**For Compliance/Risk Teams:**
- ❌ Risk of chatbot accidentally giving investment advice
- ❌ No way to audit what was said to customers
- ❌ Missing disclaimers/refusal flows
- ❌ Potential liability if wrong information given
- ❌ No PII protection (could accept customer account numbers)

---

## 4. CUSTOMER JOURNEY

### **Current State (Manual & Fragmented):**
```
Investor 
  → Google "SBI Bluechip expense ratio"
  → Finds 5 results (official, blogs, fund comparison sites)
  → Clicks official SBI MF page
  → Navigates through PDF factsheet (hard to find)
  → Gets number but no context
  → May also read blog with potentially outdated info
  → Confused about which source to trust

Support Agent
  → Customer asks "What is exit load?"
  → Searches internal docs/wiki
  → Crafts answer from memory (may miss details)
  → Sends response
  → Customer asks follow-up; cycle repeats
  → No audit trail of what was communicated
```

### **Desired State (RAG-Powered & Consistent):**
```
Investor
  → Opens INDMoney FAQ chatbot
  → Types "What is the expense ratio of SBI Bluechip ELSS?"
  → Gets instant answer: "0.65% per annum. Source: [official SBI SID link]"
  → Types "What is exit load?" → Gets answer + link
  → Types "Should I buy?" → Polite refusal + educational link

Support Agent
  → Customer asks "How do I download capital gains statement?"
  → Types question in chatbot
  → Copies pre-generated answer with link
  → Pastes to customer
  → Audit log records: "Question asked, Answer provided, Source cited"
  → Done in 30 seconds vs 5 minutes
```

### **Three-Phase User Journey:**

| Phase | Interaction | Input | Output |
|-------|-------------|-------|--------|
| **1. Discovery** | User types factual question | "What is ELSS?" | Factual answer (≤3 sentences) + source link |
| **2. Exploration** | User asks follow-up questions | "What's the lock-in period?" | Consistent answers across all similar questions |
| **3. Refusal** | User asks for advice | "Should I buy this fund?" | Polite refusal + educational link + advisor referral |

---

## 5. WHAT WE'RE SOLVING

### **Challenge A: Single Source of Truth**
- **Problem:** Investors bounce between 10+ websites; get conflicting info
- **Solution:** One chatbot that pulls from only official sources (AMC, SEBI, AMFI)
- **Success Metric:** 100% of answers come from official sources; 0% third-party blogs

### **Challenge B: Scalable Support**
- **Problem:** Support team answers same question 100+ times per week manually
- **Solution:** FAQ chatbot provides instant, consistent answer to any question
- **Success Metric:** 90% of common questions answered by chatbot; 50% reduction in support time

### **Challenge C: Compliance & Safety**
- **Problem:** Risk of chatbot giving investment advice or accepting PII
- **Solution:** Strict refusal flows + PII detection + audit trail of all responses
- **Success Metric:** 0% investment advice given; 0% PII accepted; 100% of refusals logged

### **Challenge D: Citation Accuracy**
- **Problem:** Answers without proper sources are useless/risky
- **Solution:** Every answer includes exactly 1 clear citation link to official source
- **Success Metric:** 100% of answers have valid, live citation links

### **Challenge E: Up-to-Date Information**
- **Problem:** Scheme rules change; outdated info persists on internet
- **Solution:** RAG pulls only from curated, verified source docs (15-25 official URLs)
- **Success Metric:** Factsheet outdated? Update corpus; chatbot auto-uses new info

---

## 6. CORE FEATURES & TECHNICAL SPECS

### **Feature 1: Curated Source Corpus (15-25 Official URLs)**

**AMC Sources (SBI Mutual Fund):**
- Scheme Factsheets (SID/KIM for each selected scheme)
- Fee/Charges pages
- Tax/Capital Gains documentation guides
- Scheme-specific FAQs
- Risk/Riskometer explanation pages
- Benchmark & Performance guides

**Regulatory Sources (SEBI/AMFI):**
- SEBI Mutual Fund Basics guide
- AMFI Investor Education pages
- Exit Load definitions + regulations
- ELSS lock-in rules
- SIP guidelines

**Official Fund Pages:**
- SBI Bluechip Equity Fund (Large-cap)
- SBI Flexicap Fund (Flexi-cap)
- SBI ELSS Tax Saver Fund (ELSS)
- [Additional schemes as selected]

### **Feature 2: Question Classification & Answer Routing**

```
Incoming Question
  ↓
[Classify]
  ├─ FACTUAL (go to RAG pipeline)
  │   • "What is ELSS?"
  │   • "Expense ratio of?"
  │   • "Minimum SIP?"
  │   • "How to download statement?"
  │   • "What is exit load?"
  │   • "Lock-in period?"
  │   • "Riskometer?"
  │   • "Benchmark?"
  │
  └─ ADVICE/OPINION (go to refusal flow)
      • "Should I buy this fund?"
      • "Which fund is best?"
      • "Will this fund beat the market?"
      • "What's the best return?"
```

### **Feature 3: RAG Pipeline (Retrieval + Generation)**

```
Investor Question: "What is the exit load for SBI ELSS?"
  ↓
[RETRIEVAL]
  • Embed question semantically
  • Search curated corpus (15-25 official URLs)
  • Retrieve top 3 relevant chunks:
    - Chunk 1: "Exit load: 1% if redeemed before 3 years"
    - Chunk 2: "ELSS Lock-in: 3 years mandatory"
    - Chunk 3: "After 3 years: no exit load"
  • Identify source URLs
  ↓
[GENERATION]
  • Pass question + retrieved chunks to LLM
  • Prompt: "Answer using ONLY the provided chunks. Keep ≤3 sentences. Include source link."
  • LLM generates: "Exit load for SBI ELSS is 1% if redeemed within 3 years of purchase. 
                   After 3 years, no exit load applies. See: [SID link]"
  ↓
[CITATION]
  • Extract source URL from retrieved chunk
  • Format: "Source: SBI Bluechip ELSS SID - [link]"
  • Verify URL is live + official
  ↓
[RETURN TO USER]
  • Question: "What is the exit load for SBI ELSS?"
  • Answer: "Exit load for SBI ELSS is 1% if redeemed within 3 years. After 3 years, 
           no exit load. Source: [official SID link]"
```

### **Feature 4: Refusal Flows (Advice & PII)**

**Investment Advice Refusal:**
```
Investor: "Should I buy the SBI ELSS fund?"
Chatbot Response:
  "I can only provide factual information about schemes, not investment advice.
   Please consult with a qualified financial advisor to make decisions.
   
   Learn more about choosing funds:
   • SEBI: How to Choose a Mutual Fund — [link]
   • AMFI: Investor Education — [link]
   
   Would you like to know facts about the SBI ELSS scheme instead?"
```

**Performance Claim Refusal:**
```
Investor: "What returns will I get from this fund?"
Chatbot Response:
  "I can't predict future returns. Past performance is not indicative of future results.
   
   View official factsheet for historical data:
   • SBI Bluechip Equity Fund — [factsheet link]
   
   Remember: Returns depend on market conditions, not guaranteed.
   
   Need help with other factual questions?"
```

**PII Refusal:**
```
Investor: "My PAN is ABC1234567D"
Chatbot Response:
  "Please don't share your PAN, Aadhaar, account numbers, OTP, or email in this chat.
   This information is for verification only and shouldn't be shared with chatbots.
   
   For account-specific questions, contact support: [support link]"
```

### **Feature 5: Citation Management**

**Source Validation:**
- Every source URL must be official (AMC domain, sebi.gov.in, amfiindia.com)
- No third-party blogs, YouTube, or unverified sources
- Links checked daily for validity
- Outdated links replaced with current versions

**Citation Format (Standard):**
```
"Last updated from sources: Mar 7, 2024
Source: [Official SBI Bluechip ELSS Scheme Information Document](link)"
```

### **Feature 6: Minimal, Clean UI**

```
┌─────────────────────────────────────────┐
│   INDMoney Mutual Fund FAQ Assistant    │
├─────────────────────────────────────────┤
│                                         │
│  Welcome! Ask factual questions about   │
│  mutual fund schemes.                   │
│                                         │
│  ⚠️  DISCLAIMER                         │
│  Facts-only. No investment advice.      │
│  Always verify with official sources.   │
│                                         │
│  Example Questions:                     │
│  • What is the expense ratio of SBI     │
│    Bluechip?                            │
│  • What is ELSS lock-in period?         │
│  • How do I download statements?        │
│                                         │
│  ┌────────────────────────────────────┐ │
│  │ Ask your question here...          │ │
│  └────────────────────────────────────┘ │
│                                         │
└─────────────────────────────────────────┘

User: "What is exit load?"
Bot: "Exit load is a fee charged when you redeem 
     mutual fund units before the specified period. 
     For ELSS, it's 1% if redeemed within 3 years.
     
     Source: [official SID link]"
```

---

## 7. SCOPE: AMC & SCHEMES SELECTED

**AMC:** SBI Mutual Fund

**3-5 Schemes:**
1. **SBI Bluechip Equity Fund** — Large-cap equity fund
2. **SBI Flexicap Fund** — Flexible allocation across market caps
3. **SBI ELSS Tax Saver Fund** — Equity Linked Savings Scheme (ELSS) with 3-year lock-in

**Optional additions:**
4. SBI Liquid Fund — Short-term investment
5. SBI Gold Fund — Commodity fund

---

## 8. SOLUTION PRIORITIZATION

| Priority | Feature | Effort | Impact | Reasoning |
|----------|---------|--------|--------|-----------|
| **P0** | **Curated source corpus (15-25 URLs)** | High | High | Foundation; quality of entire system depends on sources |
| **P0** | **RAG retrieval + generation pipeline** | High | High | Core product; must retrieve + answer accurately |
| **P0** | **Citation linking** | Medium | High | Every answer must cite source; non-negotiable |
| **P0** | **Refusal flows (advice, PII)** | Medium | High | Compliance critical; prevents liability |
| **P1** | **Question classification** | Medium | Medium | Improves answer routing; not critical for MVP |
| **P1** | **Minimal UI + 3 example questions** | Low | Medium | Nice polish; helps users understand what to ask |
| **P2** | **Historical performance data** | Medium | Low | Nice-to-have; not core for facts-only MVP |
| **P3** | **Multi-language support** | Low | Low | Can add later |

---

## 9. KEY CONSTRAINTS & COMPLIANCE REQUIREMENTS

### **Source Constraints:**
- ✅ **Official sources only** — AMC, SEBI, AMFI, official fund pages
- ✅ **No third-party blogs** — No Moneycontrol, BSE India, Morningstar, etc.
- ✅ **No screenshots** — Must link to actual pages, not screenshots
- ✅ **15-25 URLs curated** — Every source reviewed + verified
- ✅ **Live link validation** — Daily check; replace broken links

### **Data Privacy Constraints:**
- ✅ **No PII collection** — Refuse PAN, Aadhaar, account #, OTP, email, phone
- ✅ **No PII in responses** — Use `[REDACTED]` if needed
- ✅ **No data storage** — Don't store chat history with PII
- ✅ **Audit logging** — Log all questions + refusals (without PII)

### **Compliance Constraints:**
- ✅ **No investment advice** — Refuse "Should I buy?", "Which is best?", etc.
- ✅ **No performance claims** — Don't compute/compare returns
- ✅ **No predictions** — Don't say "This will return 15% next year"
- ✅ **Facts-only tone** — Neutral, educational, no sales pitch
- ✅ **Clear disclaimers** — "Facts-only. No investment advice." on UI
- ✅ **Last updated date** — Every answer includes source date

### **Answer Constraints:**
- ✅ **Conciseness** — Keep answers ≤3 sentences
- ✅ **One citation per answer** — Exactly 1 clear source link
- ✅ **Verifiability** — User can click link and verify answer
- ✅ **Completeness** — Answer the specific question asked (not generic)

---

## 10. SUCCESS METRICS

| Metric | Target | Test Type | Owner |
|--------|--------|-----------|-------|
| **Source Verification** | 100% of 15-25 URLs are official + live | Link audit | PM |
| **Answer Accuracy** | 90%+ answers match official sources | Golden dataset test | QA |
| **Citation Completeness** | 100% of answers include valid citation link | Content check | QA |
| **Factuality** | 0% incorrect/misleading information | Manual review | Compliance |
| **Advice Refusal Rate** | 100% of investment advice requests refused | Adversarial test | Compliance |
| **PII Refusal Rate** | 100% of PII attempts detected + refused | Adversarial test | Security |
| **Answer Conciseness** | 100% of answers ≤3 sentences | Word count check | PM |
| **Response Time** | <2 seconds per answer | Performance test | Eng |
| **User Satisfaction** | ≥80% of users find answer helpful | Survey/UX test | PM |

---

## 11. TECHNICAL IMPLEMENTATION ROADMAP

### **Phase 1: Source Curation (Week 1)**
- Identify 15-25 official sources (AMC SIDs, SEBI pages, AMFI resources)
- Verify each URL is official + currently live
- Download/extract text from each source
- Create corpus metadata (URL, title, scheme, topic, last-updated)
- Store in vector database (Pinecone, Weaviate, or local embeddings)

### **Phase 2: RAG Pipeline (Week 1)**
- Set up embedding model (OpenAI, Cohere, or open-source)
- Implement retrieval (semantic search over corpus)
- Build LLM prompt for generation (with constraints: ≤3 sentences, cite source, no advice)
- Implement citation extraction (map retrieved chunk to source URL)
- Test with 10 sample questions

### **Phase 3: Refusal Flows & Safety (Week 1-2)**
- Build question classifier (factual vs. advice vs. PII)
- Implement refusal responses (advice, performance, PII)
- Add educational links to refusal messages
- Build PII detector (regex + NLP for PAN, Aadhaar, etc.)
- Implement audit logging (question, answer, source, timestamp)

### **Phase 4: UI & Integration (Week 2)**
- Build minimal Streamlit/Gradio UI
- Add welcome message + 3 example questions
- Add disclaimer: "Facts-only. No investment advice."
- Add "Last updated from sources: {date}" to answers
- Test end-to-end (question → retrieval → answer → citation)

### **Phase 5: Testing & Deployment (Week 2-3)**
- Golden dataset (10 factual questions with expected answers)
- Adversarial tests (10 advice requests, 5 PII attempts)
- Manual review of 50 random Q&As for accuracy + tone
- Deployment (Streamlit Cloud, Hugging Face, etc.)
- Demo video + documentation

---

## 12. DELIVERABLES CHECKLIST

- [ ] **GitHub Repository Link** — https://github.com/manishkumar98/ind-money-rag-chatbot
- [ ] **README.md** — Setup steps, scope (AMC + schemes), known limits, sources list
- [ ] **Source List (CSV/MD)** — 15-25 URLs with titles, dates, topics
- [ ] **Sample Q&A (5-10 examples)** — Questions, answers, source links
- [ ] **Disclaimer Snippet** — Copy-paste text: "Facts-only. No investment advice."
- [ ] **Working Prototype Link** — Live Streamlit/Gradio link or demo video (≤3 mins)
- [ ] **Setup Instructions** — How to run locally, install dependencies, etc.
- [ ] **Known Limitations Document** — What the chatbot can/cannot do
- [ ] **Test Results Report** — Golden dataset scores, adversarial test results
- [ ] **Audit Log Sample** — Example of logged Q&As with refusals
- [ ] **Source Metadata** — JSON/CSV with URL, scheme, topic, last-updated for each source

---

## 13. SUCCESS DEFINITION

**This chatbot is successful when:**

1. ✅ All 15-25 sources are official (AMC, SEBI, AMFI) + currently live
2. ✅ Chatbot answers 10 sample factual questions with 90%+ accuracy
3. ✅ Every answer includes exactly 1 clear, valid citation link
4. ✅ All answers are ≤3 sentences and factually correct
5. ✅ 100% of investment advice requests are politely refused
6. ✅ 100% of PII attempts are detected + refused
7. ✅ 0% incorrect or misleading information in any answer
8. ✅ Disclaimer visible on UI: "Facts-only. No investment advice."
9. ✅ Response time <2 seconds per question
10. ✅ Audit trail logs all questions + answers + sources + refusals
11. ✅ User can verify every answer by clicking the source link
12. ✅ Support team can copy-paste answers to customers reliably

---

## 14. INTEGRATION WITH OTHER MILESTONES (Future)

### **M2 Integration (Weekly Product Pulse):**
- **Future Enhancement:** If M2 identifies "Exit Load Confusion" as theme, link to M1 FAQ
- **Example:** M2 Weekly Pulse → "Link to FAQ: What is exit load?"
- **Priority:** P2 (future phase)

### **M3 Integration (Voice Agent):**
- **Future Enhancement:** If investor asks "What to prepare?" for SIP consultation, link to M1 FAQ
- **Example:** Voice agent → "See our FAQ on SIP: [M1 link]"
- **Priority:** P2 (future phase)

### **Smart-Sync FAQ (M1+M2 Unified):**
- **Future Enhancement:** Merge M1 FAQ with M2 pulse context
- **Example:** Investor asks "What is exit load AND why was I charged?" → M1 fact + M2 context
- **Priority:** P0 (core to capstone project)

---

## 15. SAMPLE OUTPUTS

### **Sample Q&A 1: Factual Question**
```
Q: "What is the expense ratio of SBI Bluechip Equity Fund?"
A: "The expense ratio of SBI Bluechip Equity Fund is 0.65% per annum. 
   This is charged as a percentage of your investment. 
   See the official factsheet for details.
   
   Source: SBI Bluechip Equity Fund - Scheme Information Document (SID)
   [https://www.sbimf.com/en/investor/documents/bluechip-sid]
   
   Last updated from sources: Mar 7, 2024"
```

### **Sample Q&A 2: Refusal (Advice)**
```
Q: "Should I buy the SBI ELSS fund?"
A: "I can only provide factual information about schemes, not investment recommendations.
   Please consult with a qualified financial advisor based on your goals and risk profile.
   
   Learn more about choosing funds:
   • SEBI Investor Guide: How to Choose a Mutual Fund
     [https://www.sebi.gov.in/investor-education/mutual-funds]
   • AMFI: Investor Education
     [https://www.amfiindia.com/investor-education]
   
   What factual questions do you have about mutual fund schemes?"
```

### **Sample Q&A 3: Refusal (PII)**
```
Q: "My account number is 123456789"
A: "Please don't share your account number, PAN, Aadhaar, OTP, or other personal information here.
   This chatbot doesn't handle account-specific queries or sensitive data.
   
   For account-related questions, please contact:
   • INDMoney Support: [support link]
   
   I can help with general factual questions about mutual fund schemes!"
```

### **Sample Source List (CSV)**
```
URL,Title,Scheme,Topic,LastUpdated
https://www.sbimf.com/en/investor/documents/bluechip-sid,SBI Bluechip ELSS SID,Bluechip ELSS,Fund Details,2024-03-01
https://www.sebi.gov.in/investor-education/mutual-funds,SEBI Mutual Funds Guide,General,Investor Education,2024-02-28
https://www.amfiindia.com/exit-load,AMFI Exit Load Explainer,General,Exit Load,2024-03-05
...
```

---

## 16. ARCHITECTURE DIAGRAM

```
┌─────────────────────────────────────────────────────────────┐
│        RAG-BASED MUTUAL FUND FAQ CHATBOT (M1)               │
│            (Facts-Only, Citation-Backed)                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Source Corpus Layer (15-25 Official URLs)           │  │
│  │  • AMC: SBI MF Factsheets, SIDs, KIM                 │  │
│  │  • Regulatory: SEBI pages, AMFI pages               │  │
│  │  • Official: Scheme FAQs, fee guides, tax guides    │  │
│  │  • Status: Verified daily + updated                 │  │
│  └──────────────────────────────────────────────────────┘  │
│                          ↓                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Vector Database & Embeddings                        │  │
│  │  • Convert each source doc to embeddings             │  │
│  │  • Index semantically (Pinecone/Weaviate/local)     │  │
│  │  • Store metadata: URL, scheme, topic, date         │  │
│  └──────────────────────────────────────────────────────┘  │
│                          ↓                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Question Router Layer                               │  │
│  │  • Classify: Factual vs. Advice vs. PII?            │  │
│  │  • If factual → go to RAG pipeline                   │  │
│  │  • If advice/PII → go to refusal flow                │  │
│  └──────────────────────────────────────────────────────┘  │
│                          ↓                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  RAG Pipeline (Factual Path)                         │  │
│  │                                                      │  │
│  │  [RETRIEVE]                                          │  │
│  │  • Embed user question                               │  │
│  │  • Search corpus semantically                        │  │
│  │  • Retrieve top 3 matching chunks + URLs             │  │
│  │                                                      │  │
│  │  [GENERATE]                                          │  │
│  │  • Pass chunks + question to LLM                     │  │
│  │  • Prompt: "Use ONLY chunks, ≤3 sentences, cite"   │  │
│  │  • LLM generates factual answer                      │  │
│  │                                                      │  │
│  │  [CITE]                                              │  │
│  │  • Extract source URL from chunk                     │  │
│  │  • Verify URL is valid + official                    │  │
│  │  • Format citation                                   │  │
│  └──────────────────────────────────────────────────────┘  │
│                          ↓                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Refusal Flow (Advice/PII Path)                      │  │
│  │  • Detect: Investment advice? PII? Prediction?       │  │
│  │  • Generate polite refusal message                   │  │
│  │  • Include educational link or support contact      │  │
│  │  • Log refusal in audit trail                        │  │
│  └──────────────────────────────────────────────────────┘  │
│                          ↓                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Output & Audit Layer                                │  │
│  │  • Format response (≤3 sentences + citation)         │  │
│  │  • Add "Last updated: {date}"                        │  │
│  │  • Log: question, answer, source, timestamp, user    │  │
│  │  • Return to user via UI                             │  │
│  └──────────────────────────────────────────────────────┘  │
│                          ↓                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  User Interface (Minimal, Clean)                     │  │
│  │  • Welcome + disclaimer                              │  │
│  │  • 3 example questions                               │  │
│  │  • Input box: "Ask your question"                    │  │
│  │  • Output: Answer + clickable citation link          │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                             │
└─────────────────────────────────────────────────────────────┘

Safety Layer (Always Active):
  • PII detector (PAN, Aadhaar, account #, email, phone)
  • Advice detector (should I buy?, which is best?, etc.)
  • Refusal responder (polite + educational)
  • Audit logger (all activity logged)
```

---

## 17. RESOURCE REQUIREMENTS

| Resource | Type | Provider | Notes |
|----------|------|----------|-------|
| **LLM** | Core AI | OpenAI/Claude/Gemini/Ollama | For RAG generation |
| **Embeddings** | Semantic Search | OpenAI/Cohere/HuggingFace | For corpus indexing |
| **Vector DB** | Storage | Pinecone/Weaviate/Local | For retrieval |
| **UI Framework** | Frontend | Streamlit/Gradio | For chatbot interface |
| **PII Detector** | Utility | Regex/spaCy/Presidio | For data safety |
| **Source Validator** | Utility | requests library | Check URL validity |
| **Logging** | Infrastructure | Python logging/ELK | Audit trail |
| **Hosting** | Deployment | Streamlit Cloud/HF Spaces | Make app accessible |

---

## 18. SUCCESS CRITERIA SUMMARY

| Criterion | Status | Evidence |
|-----------|--------|----------|
| ✅ 15-25 official sources curated | TODO | Source list CSV + validation |
| ✅ Sources are live + current | TODO | URL checker results |
| ✅ Chatbot answers 10 factual Q&As | TODO | Golden dataset results |
| ✅ All answers ≤3 sentences | TODO | Word count validation |
| ✅ All answers cite official source | TODO | Citation check |
| ✅ 100% advice refusals | TODO | Adversarial test results |
| ✅ 100% PII refusals | TODO | Adversarial test results |
| ✅ UI has disclaimer + examples | TODO | UI screenshot |
| ✅ Response time <2 seconds | TODO | Performance test |
| ✅ Audit trail functional | TODO | Sample log file |

---

## 19. EXAMPLE SUPPORTED QUESTIONS

The chatbot should answer questions like:

✅ **Scheme Details:**
- "What is the expense ratio of SBI Bluechip?"
- "What is the minimum SIP for SBI ELSS?"
- "What is the lock-in period for ELSS?"
- "What benchmark does SBI Bluechip track?"

✅ **Fees & Charges:**
- "What is exit load?"
- "When do I pay exit load?"
- "Is there a maintenance charge?"
- "How is STT (securities transaction tax) calculated?"

✅ **Transactions & Statements:**
- "How do I download my statement?"
- "How do I download capital gains certificate?"
- "How do I calculate cost basis?"

✅ **Risk & Return:**
- "What is riskometer?"
- "What are the risk ratings?"
- "How is return calculated?"

❌ **Should NOT Answer (Refusal):**
- "Should I buy this fund?"
- "Which fund will beat the market?"
- "Will this fund give 20% returns?"
- "What are your credentials?" (PII)

---

## CONCLUSION

The **RAG-Based Mutual Fund FAQ Chatbot (M1)** solves the critical problem of scattered, unreliable financial information by creating a single source of truth. By combining:

✅ **Curated corpus** (15-25 official sources only)  
✅ **RAG pipeline** (accurate retrieval + grounded generation)  
✅ **Citation backing** (every answer linked to official source)  
✅ **Strict safety** (0% advice, 0% PII, 100% compliance)  

The chatbot enables:
- **Retail investors** to make informed decisions with verified facts
- **Support teams** to scale consistently with 50%+ time savings
- **Compliance teams** to maintain full audit trail and zero risk

**This is the foundation of the entire capstone suite—everything else (M2, M3) depends on accurate, trustworthy facts.**

