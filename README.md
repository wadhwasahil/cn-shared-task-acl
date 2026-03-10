Multilingual Counter-Narrative Generation using DSPy

Author: Preethi Gajawada
Institution: Northeastern University
Year: 2026

1. Introduction

Online platforms increasingly face challenges related to harmful, hateful, or extremist content. Traditional moderation strategies such as removing content or banning users are often insufficient for promoting constructive dialogue.

An alternative approach is counter-narrative generation, where harmful statements are addressed through respectful responses that challenge the underlying claim and encourage empathy and reflection.

This project implements a DSPy-based multilingual counter-narrative generation system capable of producing polite, contextual, and constructive responses to hateful speech.

The system leverages:

• Large Language Models (LLMs)
• Prompt optimization techniques
• Reward-based evaluation metrics

The framework supports English and Tamil counter-narrative generation and evaluates responses using both LLM-based judging and automated reward functions.

2. Project Objectives

The main objectives of this project are:

• Generate respectful counter narratives for harmful speech
• Optimize prompts to improve generation quality
• Evaluate generated responses using multiple evaluation metrics
• Support multilingual counter-speech generation
• Build a modular and extensible DSPy pipeline

The generated responses aim to be:

• Polite and respectful
• Contextually relevant
• Empathetic and non-confrontational
• Constructive rather than argumentative

3. System Architecture

The system follows a modular pipeline architecture consisting of multiple stages.

Pipeline Flow

Hate Speech Input
↓
DSPy Counter Narrative Program
↓
Large Language Model (GPT-4o)
↓
Prompt Optimization using COPRO
↓
Generated Counter Narrative
↓
Evaluation Pipeline

Evaluation includes:

• LLM-based judge scoring
• Reward-based evaluation functions
• Reference-based metrics

4. Repository Structure

The repository is organized into multiple modules that support generation, optimization, and evaluation.

counterNarrativeGeneration
    dspy_cn
        base_llm.py
        dspy_program.py
        dspy_optimize.py
        dspy_metric.py
        evaluator.py
        generate_predictions.py

        rewards
            base.py
            r1_safety.py
            r2_empathy.py
            r3_grounding.py
            r4_non_confrontational.py
            r8_mnli_contradiction.py
            r12_cosine_gt.py
            distinct2.py
            bertscore_metric.py

        data
        outputs
        logs

5. Base Language Model

The system uses GPT-4o as the base language model configured through the DSPy framework.

The model is initialized using a configuration function that enables structured JSON responses, ensuring compatibility with DSPy modules.

This configuration ensures consistent interaction between the DSPy pipeline and the underlying language model.

6. Counter-Narrative Generation

The DSPy program defines structured generation signatures used by the model to produce counter narratives.

Each generated response follows strict guidelines:

• Exactly 2–3 sentences
• Maintain a polite and respectful tone
• Address the harmful claim directly
• Avoid lecturing, preaching, or moralizing
• Avoid repeating the original hate speech

The system supports two generation pipelines:

• English counter-narrative generation
• Tamil counter-narrative generation

Both pipelines use DSPy modules to ensure structured outputs.

7. Prompt Optimization

The project uses COPRO (Candidate Optimization for Prompt Refinement) to improve the quality of prompt instructions used by the language model.

Optimization parameters include:

Optimizer: COPRO
Candidate prompts per step: 4
Number of optimization iterations: 5
Evaluation metric: Reward-based scoring

During optimization, the system generates multiple prompt candidates and selects the prompt configuration that produces the highest evaluation score.

The optimized prompts are stored and reused during inference.

8. Evaluation Methods

The system evaluates generated counter narratives using two complementary approaches.

8.1 LLM-as-Judge Evaluation

A GPT-4o model is used as an automated evaluator to assess the quality of generated responses.

The evaluation uses three criteria:

PRS – Politeness and Respectfulness Score

Measures whether the response maintains a respectful and polite tone.

CCNC – Claim-Centered Counter Narrative Coherence

Measures whether the response directly addresses the harmful statement.

QS – Quality Score

Measures clarity, grammar, coherence, and content richness.

Scores from these criteria are normalized to produce a final evaluation score.

8.2 Reward-Based Evaluation

The system also evaluates responses using multiple reward functions that capture different aspects of counter-narrative quality.

Reward functions include:

R1 – Safety and Non-Toxicity

Ensures the generated response does not contain toxic language.

R2 – Empathy Reward

Measures empathetic language and emotional tone.

R3 – Semantic Grounding

Measures semantic similarity between the hate speech and the counter narrative.

R4 – Non-Confrontational Tone

Ensures the response avoids aggressive language.

R8 – MNLI Contradiction

Evaluates how effectively the counter narrative challenges the harmful claim.

R12 – Ground-Truth Alignment

Measures similarity between the generated response and the reference counter narrative.

Composite scores are calculated as:

PRS = politeness score
CCNC = contextual coherence score
QS = response quality score

Final combined score:

combined score = (PRS + CCNC + QS) / 3

9. Additional Evaluation Metrics

The system also includes reference-based evaluation metrics.

Distinct-2

Measures lexical diversity in generated counter narratives.
Higher scores indicate more varied language usage.

BERTScore

Measures semantic similarity between generated counter narratives and reference responses using contextual embeddings.

10. Installation

Clone the repository:

git clone https://github.com/gajawp/counterNarrativeGeneration.git

cd counterNarrativeGeneration

Create a virtual environment:

python -m venv venv
source venv/bin/activate

Install required dependencies:

pip install -r requirements.txt

Set the OpenAI API key:

export OPENAI_API_KEY=your_api_key

11. Running Prompt Optimization

To run prompt optimization using the COPRO optimizer:

python dspy_cn/dspy_optimize.py

Optimized prompts and logs will be stored in:

dspy_cn/outputs
dspy_cn/logs

12. Generating Counter Narratives

To generate counter narratives for the test datasets and evaluate them:

python dspy_cn/generate_predictions.py

Generated outputs will be stored in:

dspy_cn/outputs/predictions_en.csv
dspy_cn/outputs/predictions_ta.csv

Submission files will also be generated automatically.

13. Example

Input Hate Speech

“Immigrants are ruining our country.”

Generated Counter Narrative

“Many immigrants contribute significantly to economic growth and innovation.
Recognizing these contributions can help foster understanding and stronger communities.”

14. Applications

This system can be applied in several domains including:

• Online moderation systems
• Counter-speech generation tools
• Responsible AI research
• Hate speech mitigation platforms

15. Author

Preethi Gajawada
MS Computer Science
Northeastern University
