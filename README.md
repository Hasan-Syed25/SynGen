# Legal QA Generator

![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)

This project is a Python script designed to generate question-answer (QA) pairs from a dataset containing text about Pakistani law, specifically focusing on the Constitution of Pakistan and the Pakistan Penal Code. It leverages a large language model (LLM) via the OpenAI API to create detailed, legally accurate QA pairs, which are then saved in both JSON and Excel formats. The script is highly customizable and integrates with Hugging Face datasets for input data.

## Features
- **Dataset Integration**: Loads datasets from Hugging Face (default: `Syed-Hasan-8503/FYP-PPC-pretrain-corpus`).
- **QA Pair Generation**: Generates up to 25 QA pairs per text chunk, customizable via command-line arguments.
- **Legal Focus**: Questions and answers are tailored to Pakistani law, using precise legal terminology.
- **Output Formats**: Saves results in JSON (for programmatic use) and Excel (for human-readable review).
- **Error Handling**: Robust parsing with fallback mechanisms for JSON and regex-based QA extraction.
- **Progress Tracking**: Uses `tqdm` for progress bars and saves intermediate results to prevent data loss.
- **Customizable**: Supports configurable dataset splits, target question counts, and chunk sizes.

## Prerequisites
- Python 3.8+
- A local vLLM API server running at `http://localhost:8000/v1` (or modify `setup_openai_client` for another endpoint).
- Access to a Hugging Face dataset (default or custom).

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/pakistani-legal-qa-generator.git
   cd pakistani-legal-qa-generator
   ```

2. **Install Dependencies**:
   Install the required Python packages using `pip`:
   ```bash
   pip install -r requirements.txt
   ```
   If no `requirements.txt` exists yet, install the following:
   ```bash
   pip install pandas tqdm datasets openai openpyxl
   ```

3. **Set Up vLLM Server**:
   Ensure a vLLM server is running locally with the specified model (`lambdalabs/Llama-3.3-70B-Instruct-AWQ-4bit`). Refer to the [vLLM documentation](https://vllm.readthedocs.io/en/latest/) for setup instructions.

## Usage

Run the script with default settings:
```bash
python generate_qa.py
```

### Command-Line Arguments
Customize the script's behavior with the following options:

| Argument            | Description                                      | Default Value                           |
|---------------------|--------------------------------------------------|-----------------------------------------|
| `--dataset`         | Hugging Face dataset ID                         | `Syed-Hasan-8503/FYP-PPC-pretrain-corpus` |
| `--split`           | Dataset split to use                            | `train`                                 |
| `--json-output`     | Output JSON file path                           | `pakistan_legal_qa.json`                |
| `--excel-output`    | Output Excel file path                          | `pakistan_legal_qa.xlsx`                |
| `--target-questions`| Target number of questions to generate          | `2000`                                  |
| `--start-idx`       | Starting index in the dataset                   | `0`                                     |
| `--end-idx`         | Ending index in the dataset (exclusive)         | `None` (process all)                    |
| `--chunk-size`      | Number of rows to process before saving         | `1`                                     |

Example with custom parameters:
```bash
python generate_qa.py --dataset "your/dataset" --target-questions 500 --json-output "output.json" --excel-output "output.xlsx"
```

### Output
- **JSON**: A file (e.g., `pakistan_legal_qa.json`) containing an array of objects with `chunk_id`, `original_text`, and `qa_pairs`.
- **Excel**: A file (e.g., `pakistan_legal_qa.xlsx`) with a `QA Pairs` sheet containing two columns: `Question` and `Answer`.

## How It Works
1. **Dataset Loading**: Loads a specified dataset split from Hugging Face.
2. **Client Setup**: Configures an OpenAI client to interact with a local vLLM server.
3. **QA Generation**: Processes text chunks, generating QA pairs using a legal-specific prompt.
4. **Parsing**: Extracts QA pairs from the model's response using JSON or regex, with a fallback to raw text.
5. **Saving**: Saves results incrementally to Excel and JSON, with intermediate checkpoints.

## Example QA Pair
```json
{
  "qa_pairs": [
    {
      "question": "What is Article 25 of the Constitution of Pakistan?",
      "answer": "Article 25 guarantees equality before law and equal protection of law to all citizens."
    },
    {
      "question": "What penalty does Section 302 of Pakistan Penal Code prescribe?",
      "answer": "Section 302 of Pakistan Penal Code prescribes death or imprisonment for life as punishment for murder."
    }
  ]
}
```

## Acknowledgments
- Built with support from [Hugging Face](https://huggingface.co/) for datasets and [vLLM](https://vllm.ai/) for model inference.
- Special thanks to the open-source community for tools like `pandas`, `tqdm`, and `openpyxl`.
