# Heritage-a-Voice-First-Citation-Grounded-AI-for-Capturing-Family-Business-Knowledge

1. Chosen Problem and Why

Most small and family businesses run on tacit knowledge: which supplier to trust, how to judge quality by touch or smell, why a customer gets special terms, and what a past mistake taught. When a founder retires or becomes unavailable, this knowledge disappears, and the next generation inherits the shop but not the judgment behind it. Existing tools (Notion, wikis, SOP templates) assume the expert will type structured documents in English. Our target users, such as a 62-year-old sweet-shop owner in Pune, will not.

2. Core Technical Difficulty

The challenge is not transcription but faithful transformation of rambling, code-mixed speech into reliable knowledge. It has four parts:

Low-resource, code-mixed speech. Speakers mix Marathi, Hindi, and English with trade-specific vocabulary, which standard ASR handles poorly.
Tacit knowledge elicitation. Experts don’t know what they know. The system must ask good follow-up questions and detect what is missing, not just record answers.
Hallucination risk. If an LLM summarizes or answers “what would Dad do?”, it may invent plausible but false guidance. In a business context that is worse than no answer.
Reasoning, not just facts. The valuable part is why a decision was made, which is harder to extract than names and numbers.

3. Proposed Approach

Pipeline:
Voice capture: Short 5 to 10 minute sessions via WhatsApp or a lightweight web app on low-end phones.

Speech-to-text: Whisper large-v3 and AI4Bharat’s Indic-fine-tuned models, with a custom glossary of business terms. We keep timestamps for every segment.

Adaptive AI interviewer: An LLM, guided by a question framework based on critical-incident technique and “five whys,” chooses the next question from what was just said. A topic-coverage tracker (suppliers, pricing, quality, customers, crises, finance, people) drives a gap detector that raises uncovered areas.
Alternative public dataset can also be used. 

Structured extraction: The LLM converts transcripts into typed records, such as process, rule of thumb, contact/relationship, decision + rationale, and lesson from failure, using JSON-schema-constrained output. Each record stores pointers to source audio timestamps.

Retrieval and answering: Multilingual embeddings (BGE-M3 or multilingual-e5) with pgvector or FAISS. Answers are generated only from retrieved passages, must quote or link the original recording, and the system abstains when evidence is weak.

Teaching layer: Converts verified records into short lessons, scenario questions (“The main supplier raised prices 15%. What did your predecessor do?”), and quizzes for successors. Also Graphs, flowcharts or step-wise approaches can be explained.  

4. Stretch Goals
   
1.Fully adaptive interviewer with live gap detection across sessions
2.Auto-generated training modules and spaced-repetition quizzes for successors
3.Video and photo input (walk-throughs of tools, shelves, and ledgers) for multimodal context
4.Offline or on-device ASR for rural users, using quantized models (efficiency angle)

5. Novelty and Differentiation

Existing tools are either generic note-takers (transcribe and summarize) or enterprise knowledge-management systems built for typed documents in English. Our differentiators:

1. Active elicitation, not passive recording: the AI interviews and detects gaps.
2. Capturing rationale, not only facts: decisions are stored with their reasons.
3. Evidence-bound answers: every output links to the speaker’s own recording, with abstention when unsure.
4. Elder-in-the-loop correction as a core design feature.
5. Designed for code-mixed regional speech and family-business context, rather than corporate English.

6. Evaluation Plan

1. Speech Recognition Testing: Measure transcription accuracy for Marathi, Hindi and code-mixed speech.
2. Knowledge Extraction Testing: Check whether important facts, processes and decisions are extracted correctly.
3. Rationale Accuracy: Verify whether the system captures the actual reasons behind decisions.
4. Follow-up Question Evaluation: Check whether AI identifies missing information and asks relevant questions.
5. Citation Verification: Ensure every answer links to the correct audio recording and timestamp.
6. Hallucination Testing: Test whether the system avoids unsupported answers and abstains when evidence is insufficient.
7. Multilingual Retrieval Testing: Evaluate search accuracy across different languages.
8. User Acceptance Testing: Collect feedback from business owners and successors.
9. Knowledge Transfer Testing: Check whether successors can learn and answer business-related questions using stored knowledge.

7. Risk

> Speech Recognition Errors: Difficulty understanding Marathi-Hindi-English code-mixed speech and trade-specific terms.
> Knowledge Misinterpretation: AI may misunderstand the expert's intended meaning or reasoning.
> Hallucinations: AI may generate unsupported business advice or answers.
> Incomplete Knowledge Capture: Important details may remain undiscovered during interviews.
> Data Privacy: Sensitive family and business information may be exposed.
> Elderly User Adoption: Older business owners may find the interface difficult to use.
> Language Limitations: Regional accents and low-resource languages may reduce accuracy.
> Source Reliability: Transcripts or extracted knowledge may not accurately reflect the original recording.
> Technical Dependency: Speech and AI services may require internet access and incur costs.
