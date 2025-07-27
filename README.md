# rag
# RAGAs Integration for LLM Log Evaluation in Google Colab

[![Open in Colab](https://colab.research.google.com/drive/1fkPAId_i9ZBnrR__pQ7XbGSrG71TX5xv?usp=sharing)

## Overview

This project provides a Google Colab notebook (`RAG_precision.ipynb`) for integrating RAGAs (Retrieval Augmented Generation Assessment) metrics to evaluate LLM (Large Language Model) logs. It computes key metrics like **faithfulness**, **answer_relevancy**, and **context_precision** on log data without requiring an OpenAI API key. Instead, it uses a local open-source model (e.g., GPT-2 or Phi-2 from Hugging Face) for inference, making it accessible on free Colab resources.

The code processes a JSON log file containing system prompts (as contexts), user queries, and expected outputs (as answers/ground truths). It prepares a dataset, evaluates metrics using RAGAs, formats the results, and saves them to `ragas_output.json`.

### Key Features
- **No API Key Required**: Runs entirely locally using Hugging Face Transformers for metric computation.
- **Metrics Computed**:
  - **Faithfulness**: Measures how factually accurate the answer is relative to the context.
  - **Answer Relevancy**: Assesses how well the answer addresses the question.
  - **Context Precision**: Evaluates the relevance of the context to the ground truth.
- **Sample Data Included**: Generates a sample `llm_log.json` for testing.
- **Visual Outputs**: Displays results in tables and summaries using Pandas.
- **Download Support**: Easily download the output JSON from Colab.
- **Error Handling**: Includes progress indicators, error messages, and fallbacks.

This is ideal for developers, researchers, or students evaluating RAG systems without API costs or quotas.

## Approach

1. **Data Loading**: Parse the JSON log file to extract questions (user prompts), answers (expected outputs), contexts (system prompts), and ground truths (mirroring answers).
2. **Dataset Preparation**: Convert data into a Hugging Face `Dataset` format required by RAGAs.
3. **Local LLM Setup**: Use a small model like GPT-2 for judging metrics (wrapped via Langchain for RAGAs compatibility).
4. **Metric Computation**: Evaluate the dataset using RAGAs, handling computations locally.
5. **Output Formatting**: Generate per-item scores rounded to 2 decimals and save as JSON.
6. **Visualization**: Display results with Pandas for easy inspection.

**Assumptions**:
- System prompts are treated as "retrieved contexts."
- Expected outputs serve as both answers and ground truths (common in simulated RAG evaluations).
- Local models may yield less accurate scores than proprietary ones (e.g., OpenAI); use for prototyping.

## Setup Instructions

### Prerequisites
- A Google Colab account (free tier works, but enable GPU for faster computation: Runtime > Change runtime type > T4 GPU).
- No external API keys needed!

### Installation
No manual installation required—all packages are installed via the notebook's first cell. It uses:
- `ragas==0.1.20` (pinned for compatibility).
- `datasets`, `pandas`, `langchain`, `langchain_huggingface`, `transformers`, `torch`.

### Running the Notebook
1. Open the notebook in Google Colab (use the badge above or upload `RAG_precision.ipynb`).
2. Run cells sequentially:
   - **Cell 1**: Install packages and imports.
   - **Cell 2**: Define the `RAGAsIntegrator` class with local LLM setup.
   - **Cell 3**: Create or load sample input data (`llm_log.json`).
   - **Cell 4**: Run the integration to compute and save metrics.
   - **Cell 5**: Display results in tables and stats.
   - **Cell 6**: Download `ragas_output.json`.
3. (Optional) Replace the sample data in Cell 3 with your own JSON log file.

**Expected Runtime**: 1-5 minutes for 3 items on CPU; faster on GPU.

## Usage Example

After running the notebook with the sample data, you'll get an output like `ragas_output.json`:

[
{
"id": "item-001",
"faithfulness": 0.85,
"answer_relevancy": 0.90,
"context_precision": 0.88
},
{
"id": "item-002",
"faithfulness": 0.78,
"answer_relevancy": 0.82,
"context_precision": 0.80
},
{
"id": "item-003",
"faithfulness": 0.92,
"answer_relevancy": 0.95,
"context_precision": 0.91
}
]



(Note: Actual scores depend on the local model's judgments and may vary.)

The notebook also prints a summary:

📊 RAGAs Evaluation Results:
text
  id  faithfulness  answer_relevancy  context_precision
item-001 0.85 0.90 0.88
item-002 0.78 0.82 0.80
item-003 0.92 0.95 0.91

📈 Summary Statistics:
Average Faithfulness: 0.850
Average Answer Relevancy: 0.890
Average Context Precision: 0.863


## Code Structure

- **Class `RAGAsIntegrator`**:
  - `__init__`: Sets up metrics and local LLM (e.g., GPT-2).
  - `load_json_log`: Loads the input JSON.
  - `prepare_dataset`: Builds the Dataset with required columns.
  - `compute_ragas_metrics`: Evaluates using RAGAs.
  - `format_output`: Formats scores per item.
  - `save_results`: Saves to JSON.

- **Notebook Cells**: Modular for easy debugging—each handles a specific step.

## Troubleshooting

- **Import Errors**: Ensure the pinned RAGAs version is used. Restart the runtime if issues persist.
- **Memory Issues**: If using a larger model (e.g., Phi-2), enable GPU or reduce `max_new_tokens` in the pipeline.
- **Low Scores or Zeros**: Local models like GPT-2 are small; switch to `microsoft/phi-2` for better results (uncomment in Cell 2).
- **Rate Limits/API Errors**: This version avoids them entirely— if you see old errors, clear the runtime.
- **Custom Data**: Ensure your `llm_log.json` matches the sample structure (with `items`, `input` roles, and `expected_output`).

For more details, check the RAGAs docs: [https://docs.ragas.io/](https://docs.ragas.io/).

## Limitations
- **Model Accuracy**: Local models may not match OpenAI's quality—scores are approximate.
- **Speed**: Computation is slower than API-based (but free!).
- **Scalability**: Best for small datasets; large logs may require more resources.

## Contributing
Fork the repo, make changes, and submit a pull request. Issues and suggestions welcome!

## License
MIT License. See [LICENSE](LICENSE) for details.

---
Exact answer is not showing due to extented billing in an openai api key.

Created by [ipshitakarmakar]. Last updated: July 27, 2025.
Related
How can I include a clear overview of the code's purpose and structure in my README
What key sections should I add to explain setup, usage, and contribution guidelines
How do I best document dependencies and environment setup for reproducibility
What examples or screenshots can I add to help users understand the code's functionality
How can I write a brief explanation of the main functions and their roles in the project
RAG_precision.ipynb


