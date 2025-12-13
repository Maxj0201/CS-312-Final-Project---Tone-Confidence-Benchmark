# CS-312-Final-Project---Tone-Confidence-Benchmark

This repository implements the Tone–Confidence Benchmark, a systematic evaluation of how large language models adjust their reported confidence when identical factual questions are framed in different tones. The project includes complete experimental code, analysis tools, and reproducibility instructions.

1. Project Overview
   
The benchmark compares:
Llama-3.1-8B-Instant (via Groq API)
GPT-4o (via OpenAI API)

Both models answer the same 50 factual questions under seven conversational tones:
deferential
neutral
polite
praising
rude
skeptical
urgent

For every (question, tone) pair, the system extracts:
the model’s predicted answer (answer)
a numerical confidence score (confidence)
whether the answer is correct (is_correct)
the tone-relative confidence shift (conf_shift)
the full model output (raw_response)

The notebook then computes:
Accuracy by tone
Mean confidence by tone
Confidence shift per tone
Tone Sensitivity Index (TSI)
Expected Calibration Error (ECE)
Calibration curves
Cross-model comparisons

2. File Structure
   
Tone_Confidence_Project.ipynb- Main notebook containing the full experiment. 

questions.csv
contains the 50 factual questions and ground-truth labels.

results_tone_confidence_groq.csv contains Llama benchmark outputs.

results_tone_confidence_gpt4o.csv contains GPT-4o benchmark outputs.


The notebook is organized into 7 sections:

Experimental Setup

Benchmark Questions Dataset (50 Items)

Llama-3.1-8B Tone–Confidence Benchmark

GPT-4o Tone–Confidence Benchmark

Comparative Analysis of the Models

Summary of Key Metrics

Statistical Validation: P-values of 3 hypotheses



3. Running the Project
   
Option A — Use the existing results (recommended)
If questions.csv and both results CSVs are already included:
You do NOT need to rerun the model query loops.
All plots, metrics, calibration curves, and summary tables will load directly.
You may begin at Section 3.2 of the notebook (Llama) and Section 4.3 (GPT-4o), or at Section 5 for combined analysis.

Option B — Reproduce all results from scratch
To generate fresh outputs:
Install dependencies:
pip install pandas numpy matplotlib seaborn tqdm groq openai python-dotenv
Create a .env file containing:
GROQ_API_KEY=...
OPENAI_API_KEY=...
Run the notebook from top to bottom, including:
Section 3.1 (Llama query loop)
Section 4.2 (GPT-4o query loop)
New CSVs will be saved automatically.

4. Expected Outputs
   
Running the notebook produces:
CSV outputs for each model
Accuracy and confidence bar plots
Confidence-shift scatterplots
Tone Sensitivity Index (TSI) values
Expected Calibration Error (ECE) for each model
Calibration curves
Full comparative figures
Summary metrics tables
All figures appear inline and require no manual configuration.

Sample Output (One Row)

This table illustrates the format of a typical row produced by the Tone–Confidence Benchmark.


<h3>Sample Output (One Row)</h3>

<table>
  <tr>
    <th>Column</th>
    <th>Example Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>id</td>
    <td>1</td>
    <td>Numerical ID for the question</td>
  </tr>
  <tr>
    <td>question</td>
    <td>Is the Earth round?</td>
    <td>The original factual question</td>
  </tr>
  <tr>
    <td>tone</td>
    <td>neutral</td>
    <td>One of the 7 tone conditions</td>
  </tr>
  <tr>
    <td>answer</td>
    <td>Yes, the Earth is round, but it is not a perfect sphere...</td>
    <td>Parsed model answer</td>
  </tr>
  <tr>
    <td>confidence</td>
    <td>100</td>
    <td>Model-reported confidence (0–100)</td>
  </tr>
  <tr>
    <td>correct_answer</td>
    <td>Yes</td>
    <td>Ground-truth label</td>
  </tr>
  <tr>
    <td>raw_response</td>
    <td>Answer: Yes, the Earth is round, but it is not...</td>
    <td>Full unprocessed model output</td>
  </tr>
  <tr>
    <td>model</td>
    <td>gpt-4o</td>
    <td>Name of the model that generated the response</td>
  </tr>
</table>



Each row contains:

id: numerical question ID

question: the original factual statement

tone: one of 7 tone conditions

answer: the model’s extracted answer

confidence: parsed numerical confidence (0–100)

correct_answer: the ground-truth label

raw_response: the full model output before parsing

model: either gpt-4o or llama-3.1-8b-instant

Full Outputs

The complete outputs for all 50 questions × 7 tones × 2 models are saved in:

results_tone_confidence_gpt4o.csv
results_tone_confidence_groq.csv

These CSVs contain everything needed to reproduce all figures and statistical analyses in the notebook without rerunning the model loops.

5. Notes on Reproducibility
   
Llama-3.1-8B (Groq)
Random seed has been set
Fully deterministic with temperature=0.
Re-running the loop will always yield identical results.

GPT-4o
Uses temperature=0, but OpenAI models do not allow setting a numerical random seed.
Therefore, confidence values may vary slightly across runs, even with identical prompts.
These variations are typically small but should be expected when regenerating the CSV.
Static CSVs

If the included CSVs are used, all numerical results will match the plots and analyses exactly.

6. Recommended Workflow for Readers
   
If the goal is analysis or replication without contacting the APIs:
Keep the provided results_tone_confidence_*.csv files.
Run the notebook starting at the analysis portions:
Section 3.2 onward (Llama)
Section 4.3 onward (GPT-4o)
Section 5 for comparisons
Skip all model-query cells to avoid API cost and speed constraints.
If the goal is to re-evaluate using updated models:
Rerun both query loops in Sections 3.1 and 4.2.

7. Troubleshooting
   
Issue: Notebook errors due to missing confidence values
Fix: Regenerate CSVs using the full loops.

Issue: API authentication errors
Fix: Ensure .env file exists and kernel is restarted.

Issue: Calibration bins contain too few samples
Fix: This is expected with a small dataset and does not affect overall results.

/* THIS CODE IS MY OWN WORK . IT WAS WRITTEN WITHOUT
CO NS UL TI NG CODE WRITTEN BY OTHER STUDENTS OR RELYING
SOLELY ON LARGE LANGUAGE MODELS SUCH AS CHATGPT .
[ Max Jiang ] */
