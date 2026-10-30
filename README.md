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

Active Knowledge Elicitation: AI actively interviews business owners, asks follow-up questions, and identifies missing knowledge instead of simply recording conversations.

Rationale-Based Knowledge Capture: Preserves not only business facts and decisions but also the reasoning behind them.

Evidence-Grounded Answers: Links AI-generated answers to original audio recordings and timestamps, with the ability to abstain when evidence is insufficient.

Elder-in-the-Loop Correction: Allows experienced business owners to review, correct, and verify extracted knowledge.

Regional Language Support: Designed for Marathi, Hindi, English, and code-mixed speech, focusing on family-business needs rather than corporate English.


6. Evaluation Plan

Speech Recognition Testing: Evaluate transcription accuracy for Marathi, Hindi, and code-mixed speech.

Knowledge Extraction Testing: Verify the accuracy of extracted facts, processes, and decisions.

Rationale Accuracy: Check whether the system correctly captures the reasoning behind business decisions.

Follow-up Question Evaluation: Assess whether AI identifies knowledge gaps and asks relevant questions.

Citation Verification: Ensure answers link to the correct audio recordings and timestamps.

Hallucination Testing: Verify that AI avoids unsupported answers and abstains when evidence is insufficient.

Multilingual Retrieval Testing: Evaluate search accuracy across different languages.

User Acceptance Testing: Collect feedback from business owners and successors.

Knowledge Transfer Testing: Evaluate how effectively successors learn and retrieve business knowledge.


7. Risk

Speech Recognition Errors: Difficulty understanding code-mixed speech, regional accents, and trade-specific terms.

Knowledge Misinterpretation: AI may misunderstand the expert's intended meaning or reasoning.

Hallucinations: AI may generate unsupported business advice or incorrect answers.

Incomplete Knowledge Capture: Important details may remain undiscovered during interviews.

Data Privacy: Sensitive family and business information may be exposed.

Elderly User Adoption: Older business owners may find the interface difficult to use.

Language Limitations: Low-resource languages and regional dialects may reduce accuracy.

Source Reliability: Extracted knowledge may not accurately represent the original recording.

Technical Dependency: External AI and speech services may require internet access and incur additional costs.
