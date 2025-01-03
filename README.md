# Running Llama 3.1 8B with Ollama on Free Google Colab

<div style="display: flex; justify-content: center; gap: 10px;">
    <img src="https://github.com/coder-akram-khan/Run-Llama3.1-On-Colab/raw/refs/heads/main/Llama3-1.webp" alt="Llama 3.1" width="500">
    <img src="https://github.com/coder-akram-khan/Run-Llama3.1-On-Colab/raw/refs/heads/main/collab.webp" alt="Collab" width="500">
</div>

## Description
This repository provides step-by-step instructions to run the Llama 3.1 8B model using Ollama API on a free Google Colab environment. It is designed for anyone interested in leveraging advanced language models for tasks like Q&A, data analysis, or natural language processing, without the need for high-end local hardware.

---

## Features
- **Use of Ollama API**: Simplifies model deployment.
- **LightRAG Integration**: Leverage LightRAG for efficient interaction with Llama 3.1.
- **Free Setup**: Works seamlessly on Google Colab with minimal setup.
- **Custom QA Component**: Example for building a simple question-answering system.

---

## How to Use

1. **Install Required Libraries**
    ```bash
    sudo apt-get install -y pciutils
    curl -fsSL https://ollama.com/install.sh | sh
    pip install -U lightrag[ollama]
    ```

2. **Set Up Ollama API**
    ```python
    import os
    import threading
    import subprocess

    def ollama():
        os.environ['OLLAMA_HOST'] = '0.0.0.0:11434'
        os.environ['OLLAMA_ORIGINS'] = '*'
        subprocess.Popen(["ollama", "serve"])

    ollama_thread = threading.Thread(target=ollama)
    ollama_thread.start()
    ```

3. **Download the Model**
    ```bash
    ollama pull llama3.1:8b
    ```

4. **Integrate LightRAG**
    ```python
    from lightrag.core.generator import Generator
    from lightrag.core.component import Component
    from lightrag.components.model_client import OllamaClient

    qa_template = r"""<SYS>
    You are a helpful assistant.
    </SYS>
    User: {{input_str}}
    You:"""

    class SimpleQA(Component):
        def __init__(self, model_client, model_kwargs):
            super().__init__()
            self.generator = Generator(
                model_client=model_client,
                model_kwargs=model_kwargs,
                template=qa_template,
            )

        def call(self, input: dict) -> str:
            return self.generator.call({"input_str": str(input)})

    model = {
        "model_client": OllamaClient(),
        "model_kwargs": {"model": "llama3.1:8b"}
    }
    qa = SimpleQA(**model)
    output = qa.call("What is Standard Deviation?")
    print(output)
    ```

5. **Output Example**
    ```plaintext
    Answer: A key concept in statistics!
    Standard Deviation (SD) is a measure of the amount of variation or dispersion from the average value in a set of data...
    ```


---

## Contributions
Contributions are welcome! Feel free to open an issue or submit a pull request to improve this repository.

---

## License
[MIT License](LICENSE)

Happy Coding! 🚀
