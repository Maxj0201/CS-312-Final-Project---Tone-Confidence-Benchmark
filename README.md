# CS-312-Final-Project---Tone-Confidence-Benchmark
/* THIS CODE IS MY OWN WORK . IT WAS WRITTEN WITHOUT
CO NS UL TI NG CODE WRITTEN BY OTHER STUDENTS OR RELYING
SOLELY ON LARGE LANGUAGE MODELS SUCH AS CHATGPT .
[ Max Jiang ] */

Tone–Confidence Benchmark
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
Tone_Confidence_Project.ipynb
Main notebook containing the full experiment. The notebook is organized into 6 sections:
Experimental Setup
Benchmark Questions Dataset (50 Items)
Llama-3.1-8B Tone–Confidence Benchmark
GPT-4o Tone–Confidence Benchmark
Comparative Analysis of the Models
Summary of Key Metrics

questions.csv
Contains the 50 factual questions and ground-truth labels.
results_tone_confidence_groq.csv
Llama benchmark outputs.
results_tone_confidence_gpt4o.csv
GPT-4o benchmark outputs.

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
