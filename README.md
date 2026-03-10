**Multilingual Counter-Narrative Generation using DSPy**

Author: Preethi Gajawada
Institution: Northeastern University
Year: 2026

This project implements a DSPy-based multilingual counter-narrative generation system designed to respond to hateful or offensive speech with polite, respectful, and constructive counter narratives.
The system uses Large Language Models, prompt optimization, and reward-based evaluation metrics to generate high-quality counter speech.
The framework supports English and Tamil counter-narrative generation and evaluates outputs using both LLM-based judges and reward-based evaluation models.

**Overview**
Online platforms increasingly struggle with harmful or extremist content. Instead of removing content alone, counter-narratives provide constructive responses that challenge harmful claims while maintaining respectful dialogue.

This project builds a modular DSPy pipeline that:
Generates counter narratives
Optimizes prompts using COPRO
Evaluates outputs using multiple reward functions
Supports multilingual counter-speech generation
The goal is to produce responses that are:
Polite and respectful
Contextually relevant
Empathetic and non-confrontational
Constructive rather than argumentative

**System Architecture**

The system pipeline consists of the following components:
Hate Speech Input
        │
        ▼
DSPy Counter Narrative Program
        │
        ▼
Large Language Model (GPT-4o)
        │
        ▼
Prompt Optimization (COPRO)
        │
        ▼
Generated Counter Narrative
        │
        ▼
Evaluation Pipeline
        │
        ├── LLM Judge (PRS, CCNC, QS)
        └── Reward Functions
                │
                ├ Safety
                ├ Empathy
                ├ Semantic Grounding
                ├ Non-Confrontational Tone
                ├ MNLI Contradiction
                └ Ground-Truth Alignment

**Repository Structure**
counterNarrativeGeneration
│
└── dspy_cn
    │
    ├── base_llm.py
    ├── dspy_program.py
    ├── dspy_optimize.py
    ├── dspy_metric.py
    ├── evaluator.py
    ├── generate_predictions.py
    │
    ├── rewards
    │   ├── base.py
    │   ├── r1_safety.py
    │   ├── r2_empathy.py
    │   ├── r3_grounding.py
    │   ├── r4_non_confrontational.py
    │   ├── r8_mnli_contradiction.py
    │   ├── r12_cosine_gt.py
    │   ├── distinct2.py
    │   └── bertscore_metric.py
    │
    ├── data
    ├── outputs
    └── logs


**Base Language Model**
The system uses GPT-4o as the base LLM configured through DSPy. 

base_llm
Example configuration:

def configure_gpt4o(model="gpt-4o-mini"):
    lm = dspy.LM(
        model=model,
        response_format={"type": "json_object"}
    )

**Counter-Narrative Generation**

The DSPy program defines structured signatures for generating counter narratives.

Key constraints:
2–3 sentences
polite and respectful tone
directly address the harmful claim
avoid lecturing or moralizing


**Example program structure: **

dspy_program
class TamilCNProgram(dspy.Module):
    def __init__(self):
        self.generate = dspy.Predict(TamilCNSignature)


**The system supports:**
English counter narratives
Tamil counter narratives

**Prompt Optimization**
The project uses COPRO (Candidate Optimization for Prompt Refinement) to optimize prompt instructions.

Optimization settings include:
Candidate prompts per step: 4
Iterations: 5
Metric: Reward-based evaluation

Optimization is implemented in:
dspy_optimize.py 

**dspy_optimize**
The optimizer searches for instructions that improve counter-narrative quality across the training set.

**Evaluation Methods**
The system evaluates generated counter narratives using two approaches.

**1. LLM-as-Judge**
A GPT-4o model scores the output based on three criteria:
PRS – Politeness and Respectfulness Score
CCNC – Claim-Centered Counter Narrative Coherence
QS – Overall Quality Score
This evaluation returns a normalized score between 0 and 1. 

**dspy_metric**
**2. Reward-Based Evaluation**
The reward evaluator combines multiple reward functions to compute composite metrics. 

**evaluator**
Reward Functions
Reward	Purpose
R1	Safety and non-toxicity
R2	Empathy detection
R3	Semantic grounding
R4	Non-confrontational tone
R8	MNLI contradiction (challenging harmful claim)
R12	Ground truth alignment

These rewards are aggregated into:
PRS = politeness score
CCNC = contextual coherence
QS = overall response quality

**Additional Evaluation Metrics**
The system also computes reference-based metrics.
Distinct-2
Measures lexical diversity of generated counter narratives. 
distinct2

Higher values indicate more diverse language usage.
BERTScore
Measures semantic similarity between generated counter narratives and ground truth references. 
bertscore_metric

**Generating Predictions**
The script generate_predictions.py generates counter narratives for test datasets and evaluates them using both scoring methods. 
generate_predictions

Output includes:
Generated counter narrative
LLM judge scores
Reward scores
Distinct-2
BERTScore

Example output file:
outputs/predictions_en.csv
outputs/predictions_ta.csv


**Installation**

Clone the repository:
git clone https://github.com/gajawp/counterNarrativeGeneration.git
cd counterNarrativeGeneration

Create a virtual environment:
python -m venv venv
source venv/bin/activate

Install dependencies:
pip install -r requirements.txt

Set your OpenAI API key:
export OPENAI_API_KEY=your_key

**Running Optimization**
To run prompt optimization:
python dspy_cn/dspy_optimize.py

This will generate optimized prompts and store them in:

dspy_cn/outputs/
dspy_cn/logs/

**Generating Counter Narratives**
To generate predictions and evaluate results:
python dspy_cn/generate_predictions.py

Outputs will be saved in:
dspy_cn/outputs/

Example
Input:

"Hate speech example"
Output:
Generated counter narrative:
"Everyone deserves dignity and respect. It helps to consider that people from different backgrounds contribute positively to society. Encouraging understanding leads to stronger communities."


**Applications**
Online moderation tools
Counter-speech generation
Responsible AI research
Hate speech mitigation systems
