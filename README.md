
# Multimodal RAG with ColQwen2.5, Weaviate, and Qwen2.5-VL

This project demonstrates a cutting-edge Multimodal Retrieval-Augmented Generation (RAG) system over PDF documents. It tackles the challenge of retrieving highly specific information from dense, multimodal documents (containing both text and images) and leveraging Vision Language Models (VLMs) to provide insightful answers. The core problem addressed is the limitations of traditional RAG systems in handling complex visual and textual data simultaneously within documents, ensuring precise and contextually rich responses.

## Key Features

-   **ColQwen2.5 for Multi-Vector Embeddings**: Utilizes the ColQwen2.5 multimodal late-interaction model to generate fine-grained multi-vector embeddings for both individual PDF pages (as images) and user queries. This approach preserves minute details like table cells, labels, and numbers that would be lost in single-vector embeddings, leading to significantly more accurate retrieval. During retrieval, each query piece is compared to all page pieces (MaxSim), enhancing precision.

-   **Weaviate Vector Database**: Employs Weaviate, a high-performance vector database, with its multi-vector feature to efficiently index and store the ColQwen2.5 embeddings. This enables rapid approximate nearest-neighbor search for relevant PDF pages based on multimodal queries.

-   **Qwen2.5-VL-7B-Instruct for Multimodal RAG**: Integrates the powerful Qwen/Qwen2.5-VL-7B-Instruct Vision Language Model (VLM) for generating comprehensive answers. Retrieved PDF pages (as visual context) are fed alongside the original user query to the VLM, which then synthesizes a human-like response.

-   **Interactive Gradio Application**: A user-friendly Gradio web interface simplifies interaction with the RAG system, making it accessible for demonstrations and testing.
    -   **Visual References**: The Gradio app displays the actual retrieved PDF pages, providing users with transparent visual context for the generated answers.
    -   **Suggested Questions**: Enhances user experience by offering relevant benchmark questions, guiding users to explore the system's capabilities.
    -   **LLM Judge for Live Benchmarking**: Includes an innovative 'LLM Judge' feature that evaluates the VLM's responses against ground truth answers for benchmark questions, offering real-time critique and accuracy judgments (Correct, Partially Correct, Incorrect).

## Setup Instructions

To run this project, you will need:

1.  **Python 3.13**: Ensure you have Python version `3.13` installed.

2.  **Hardware**: A machine capable of running neural networks, typically requiring 5-10 GB of GPU memory. Recommended options:
    -   Google Colab (free-tier T4 GPU for Qwen2.5-3b-VL-Instruct).
    -   Google Colab Pro+ (A100 GPU for Qwen2.5-7b-VL-Instruct).
    -   Locally (e.g., tested on an M3 Studio 256GB Mac).

3.  **Install Dependencies**: Install the required Python libraries using pip.
    ```bash
    !apt-get install poppler-utils
    %pip install colpali_engine weaviate-client qwen_vl_utils pdf2image
    %pip install flash-attn --no-build-isolation # For A100 optimization
    %pip install -q -U "colpali-engine[interpretability]>=0.3.2,<0.4.0"
    %pip install gradio
    ```

4.  **Weaviate Vector Database Connection**: You need a running Weaviate instance (version `>=1.29.0`). Choose one of the following options:
    -   **Option 1: Weaviate Cloud (WCD)**: Obtain a free 14-day sandbox from [Weaviate Cloud](https://console.weaviate.cloud/).
        -   **Secrets Required**: Set `WEAVIATE_URL` (your cluster URL/REST ENDPOINT) and `WEAVIATE_API_KEY` (your Admin API Key) in your environment variables or Colab secrets.
    -   **Option 2: Embedded Weaviate**: For a local, no-setup experience (uncomment in the notebook).
    -   **Option 3: Local Deployment**: Via Docker or Kubernetes (uncomment in the notebook, e.g., `!docker run --detach -p 8080:8080 -p 50051:50051 cr.weaviate.io/semitechnologies/weaviate:1.29.0`).

5.  **Hugging Face User Access Token**: A `HF_TOKEN` is required for accessing models from Hugging Face. Set this as an environment variable or Colab secret.

## How to Use the Gradio Application

Once all dependencies are installed and configurations are set (follow the notebook cells), run the Gradio application cell. An interactive interface will launch in your browser (or within Colab).

1.  **Input Queries**: Type your question into the textbox labeled "Enter your query..." and click "Submit" or press Enter.
2.  **Utilize Suggested Questions**: Below the input box, you'll find "Suggested Questions." Click on these buttons to quickly query the system with predefined benchmark questions.
3.  **Observe Visual References**: The "Retrieved Relevant Pages" gallery will display images of the PDF pages that the system deemed most relevant to your query. This provides crucial visual context for the generated answer.
4.  **Interpret LLM Judge Output**: If you ask a benchmark question (either by typing it or clicking a suggested question), the "Live Benchmark Evaluation" accordion will expand to show a critique and judgment (e.g., "Correct", "Partially Correct", "Incorrect") from an independent LLM judge, comparing the system's answer against the ground truth.
5.  **Chat History**: The chatbot window on the left will keep a history of your conversation, including both your queries and the system's responses.

