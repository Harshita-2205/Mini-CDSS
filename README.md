# CDSS

The Clinical Decision Support System (CDSS) leverages the CrewAI platform to perform four distinct tasks: NER validation using a Hugging Face model to extract and validate key clinical entities, preliminary report generation for suggested diagnoses, best practices retrieval through Tavily web scraping for evidence-based management insights, and a final report writing agent to compile all outputs into a structured, actionable clinical report. This system streamlines decision-making for healthcare professionals by automating complex data processing and enhancing the accuracy and efficiency of diagnostic workflows.

---

## 📂 Project Folder Structure

The project is organized as follows:

```
crew/
├── config
│   ├── agents.yaml
│   └── tasks.yaml
├── src
│   ├── crew
│   │   ├── agents_and_tasks.py
│   ├── hugging_face_ner.py
│   ├── output_pydantic.py
│   ├── .env
│   ├── project_flow.html
│   ├── sample_report.md
│   ├── flow.py
│   └── requirements.txt
```

# Project Architecture: Clinical Decision Support System (CDSS)

The Clinical Decision Support System (CDSS) is designed as a modular pipeline leveraging the CrewAI platform to perform distinct tasks through a combination of agents and a search tool. Each component is implemented with `type safety using Pydantic output classes` to ensure reliable data exchange between tasks. The system processes input clinical text and generates a comprehensive, human-readable report in Markdown format.

## Architecture Overview

### 1. NER Validation Agent

**Purpose**: Validates and refines the output of a domain-expert medical Named Entity Recognition (NER) model.

**Inputs**:

- **NER Output**: The raw tagged entities generated by a domain-specific medical NER model.
- **Sample Input Text**: The original clinical text provided as input.

**Outputs**:

- A validated and refined list of medical entities with context extracted from the sample text.

**Key Role**:

- Ensures the accuracy and relevance of extracted clinical entities to minimize downstream errors.

---

### 2. Preliminary Report Agent

**Purpose**: Generates an initial diagnostic report based on the validated NER output.

**Inputs**:

- **NER Validation Output**: The refined entities from the NER Validation Agent.

**Outputs**:

- **Diagnosis**: A list of potential diseases or conditions identified.
- **Reasoning**: Logical explanations supporting each diagnosis.
- **Recommendations**: Preliminary management or follow-up actions.

**Key Role**:

- Provides a structured and actionable starting point for clinical decision-making.

---

### 3. Search Tool (Tavily Integration)

**Purpose**: Crawls the web to retrieve evidence-based best practices for the diagnoses identified in the preliminary report.

**Inputs**:

- **Diagnoses**: The list of diseases generated by the Preliminary Report Agent.

**Process**:

- Queries the web with the format: `"Best Practise {diagnosis}"`.
- Filters results based on a score metric, considering only items with a score > 0.5 for relevance.

**Outputs**:

- A curated list of best practices for each diagnosis, including title, URL, and a brief summary.

**Key Role**:

- Enhances the preliminary report with verified, actionable guidelines.

---

### 4. Report Writing Agent

**Purpose**: Combines outputs from all previous tasks into a final, human-readable report.

**Inputs**:

- **NER Validation Output**: Extracted and validated entities.
- **Preliminary Report Output**: Diagnoses, reasoning, and recommendations.
- **Best Practices Output**: Evidence-based management insights from the search tool.

**Process**:

- Waits for all tasks to complete.
- Accesses outputs stored in the shared `state` variable.
- Drafts a structured clinical report in Markdown format.

**Outputs**:

- A final comprehensive report exported as a `.md` file.

**Key Role**:

- Serves as the end-point of the pipeline, delivering actionable insights in a polished format.

---

## Workflow Summary

1. **NER Validation Agent** processes the input clinical text and NER model output.
2. **Preliminary Report Agent** generates an initial report using the validated entities.
3. **Search Tool** crawls the web for best practices relevant to the diagnoses.
4. **Report Writing Agent** compiles the outputs of all tasks into a final human-readable report.

---

### Output Format

The final output is a comprehensive clinical report exported as a Markdown (`.md`) file, ensuring easy readability and portability.

## How to Use the Project

Follow these steps to set up and run the project:

1. **Set Up the Environment**  
   Ensure your device has Conda installed for easy environment management (Python environments work too).

   ```bash
   conda create --name cdss_env python=3.10 -y
   conda activate cdss_env

   ```

2. **Install Dependencies**
   Install the required packages using requirements.txt:

   ```bash
   pip install -r requirements.txt
   ```

3. **Run the Python Script in the Activated Conda-Env**

   ```bash
   python flow.py
   ```
