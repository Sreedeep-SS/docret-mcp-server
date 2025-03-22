# Documentation Retrieval MCP Server (DOCRET)

This project implements a Model Context Protocol (MCP) server that enables AI assistants to access up-to-date documentation for various Python libraries, including LangChain, LlamaIndex, and OpenAI. By leveraging this server, AI assistants can dynamically fetch and provide relevant information from official documentation sources. The goal is to ensure that AI applications always have access to the latest official documentation.

## What is an MCP Server?

The Model Context Protocol is an open standard that enables developers to build secure, two-way connections between their data sources and AI-powered tools. The architecture is straightforward: developers can either expose their data through MCP servers or build AI applications (MCP clients) that connect to these servers.

## Features

- **Dynamic Documentation Retrieval**: Fetches the latest documentation content for specified Python libraries.
- **Asynchronous Web Searches**: Utilizes the **SERPER API** to perform efficient web searches within targeted documentation sites.
- **HTML Parsing**: Employs BeautifulSoup to extract readable text from HTML content.
- **Extensible Design**: Easily add support for additional libraries by updating the configuration.

## Prerequisites

- Python 3.8 or higher
- UV for Python Package Management (or pip if you're a pleb)
- A Serper API key (for Google searches or "SERP"s)
- Claude Desktop or Claude Code (for testing)

## Installation

### 1. Clone the Repository

```env
git clone https://github.com/Sreedeep-SS/docret-mcp-server.git
cd docret-mcp-server
```

### 2. Create and Activate a Virtual Environment

- **On macOS/Linux**:

  ```env
  python3 -m venv env
  source env/bin/activate
  ```

- **On Windows**:

  ```env
  python -m venv env
  .\env\Scripts\activate
  ```

### 3. Install Dependencies

With the virtual environment activated, install the required dependencies:

```env
pip install -r requirements.txt
```

or if you are using uv:

```env
uv sync
```

## Set Up Environment Variables

Before running the application, configure the required environment variables. This project uses the SERPER API for searching documentation and requires an API key.

1. Create a `.env` file in the root directory of the project.
2. Add the following environment variable:

   ```env
   SERPER_API_KEY=your_serper_api_key_here
   ```

Replace `your_serper_api_key_here` with your actual API key.

## Running the MCP Server

Once the dependencies are installed and environment variables are set up, you can start the MCP server.

```bash
python main.py
```

This will launch the server and make it ready to handle requests.

## Usage

The MCP server provides an API to fetch documentation content from supported libraries. It works by querying the SERPER API for relevant documentation links and scraping the page content.

### Searching Documentation

To search for documentation on a specific topic within a library, use the `get_docs` function. This function takes two parameters:

- `query`: The topic to search for (e.g., "Chroma DB")
- `library`: The name of the library (e.g., "langchain")

Example usage:

```python
from main import get_docs

result = await get_docs("memory management", "openai")
print(result)
```

This will return the extracted text from the relevant OpenAI documentation pages.

## Integrating with AI Assistants

You can integrate this MCP server with AI assistants like Claude or custom-built AI models. To configure the assistant to interact with the server, use the following configuration:

```json
{
  "servers": [
    {
      "name": "Documentation Retrieval Server",
      "command": "python /path/to/main.py"
    }
  ]
}
```

Ensure that the correct path to `main.py` is specified.

## Extending the MCP Server

The server currently supports the following libraries:

- **LangChain**
- **LlamaIndex**
- **OpenAI**

To add support for additional libraries, update the `docs_urls` dictionary in `main.py` with the library name and its documentation URL:

```python
docs_urls = {
    "langchain": "python.langchain.com/docs",
    "llama-index": "docs.llamaindex.ai/en/stable",
    "openai": "platform.openai.com/docs",
    "new-library": "new-library-docs-url.com",
}
```

📌 Roadmap

Surely this is really exciting for me and I'm looking forward to build more on this and stay updated with the latest news and ideas that can be implemented

This is what I have on my mind:

1. #### **Add support for more libraries (e.g., Hugging Face, PyTorch)**
   
   - Expand the `docs_urls` dictionary with additional libraries.
   - Modify the get_docs function to handle different formats of documentation pages.
   - Use regex-based or AI-powered parsing to better extract meaningful content.
   - Provide an API endpoint to dynamically add new libraries.
 

2. #### **Implement caching to reduce redundant API calls**

    - Use Redis or an in-memory caching mechanism like `functools.lru_cache`
    - Implement time-based cache invalidation.
    - Cache results per library and per search term.

    
3. #### **Optimize web scraping with AI-powered summarization**

    - Use `GPT-4`, `BART`, or `T5` for summarizing scraped documentation.
    - There are also `Claude 3 Haiku`, `Gemini 1.5 Pro`, `GPT-4-mini`, `Open-mistral-nemo`, `Hugging Face Models` and many more that can be used. All of which are subject to debate. 
    - Let users choose between raw documentation text and a summarized version.
 

4. #### **Introduce a REST API for external integrations**

    - Use FastAPI to expose API endpoints. (Just because)
    - Build a simple frontend dashboard for API interaction. (Why not?)

 

5. #### **Add unit tests for better reliabilityReferences**

    - Use pytest and unittest for API and scraping reliability tests. (Last thing we want is this thing turning into a nuclear bomb)
    - Implement CI/CD workflows to automatically run tests on every push. (The bread and butter of course)


6. #### **More MCP tools that can be useful during development**

    - Database Integrations
    - Google Docs/Sheets/Drive Integration
    - File System Operations
    - Git Integration
    - Integrating Communication Platforms to convert ideas into product
    - Docker and Kubernetes management



## References

For more details on MCP servers and their implementation, refer to the guide:

- [Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol)
- [https://modelcontextprotocol.io/](https://modelcontextprotocol.io/)
- [Adding MCP to your python project](https://github.com/modelcontextprotocol/python-sdk?tab=readme-ov-file#adding-mcp-to-your-python-project)


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

