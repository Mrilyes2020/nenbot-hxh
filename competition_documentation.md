# Hunter x Hunter RAG Chatbot System Documentation

## 1. System Capabilities
The NenBot is an intelligent, context-aware chatbot designed specifically around the Hunter x Hunter universe. Its key capabilities include:
- **Domain-Specific Knowledge**: The system strictly responds to topics related to Hunter x Hunter. Any queries outside this domain are politely refused or redirected back to the core subject.
- **Team Information**: The chatbot is programmed to recognize and provide specific details about the development team members, including their full names, academic levels, ages, university names, and fields of study.
- **Conversational Memory**: The system retains the context of the conversation by remembering the last 5 to 10 interactions, allowing for natural, multi-turn conversations.
- **Speech-to-Text (Speech Recognition)**: Users can interact with the chatbot using voice commands, which are converted to text using the browser's Web Speech API.
- **Text-to-Speech (Voice Output)**: The chatbot can read its responses aloud using a synthesized Arabic voice, creating an immersive user experience.
- **(Optional) Image Recognition**: The system can be extended to accept image inputs, utilizing vision models to identify Hunter x Hunter characters.

## 2. Technologies Used
- **Frontend / User Interface**: 
  - HTML5, CSS3, and Vanilla JavaScript.
  - **Web Speech API**: For implementing both Speech Recognition and Text-to-Speech capabilities natively in the browser.
- **Backend / Orchestration**: 
  - **n8n**: Used as the primary workflow automation tool to handle incoming webhook requests, process the logic, and route data to the LLM and vector database.
- **LLM (Large Language Model)**: 
  - Advanced models (e.g., OpenAI GPT-4o-mini or Claude 3.5) for generating responses based on the retrieved context.
- **Vector Database**: 
  - Used to store and perform similarity searches on embedded Hunter x Hunter lore and wiki data.
- **Embeddings**: 
  - To convert textual lore into vector representations.

## 3. RAG Architecture Implemented
The Retrieval-Augmented Generation (RAG) architecture follows a standard pipeline tailored for this specific use case:

1. **Data Ingestion & Indexing**:
   - A dataset of Hunter x Hunter lore (characters, nen types, story arcs) is chunked into smaller text segments.
   - These chunks are passed through an embedding model and stored in the Vector Database.

2. **User Query Processing (Retrieval)**:
   - The user submits a question via text or voice (frontend).
   - The query is sent to the backend webhook in n8n.
   - The system embeds the user's query and performs a similarity search against the Vector Database to retrieve the most relevant Hunter x Hunter lore.

3. **Context Construction & Prompt Engineering**:
   - The retrieved information, the user's query, and the recent conversation history (last 5-10 messages) are injected into a strict system prompt.
   - **System Prompt Instructions**: *"You are an expert on Hunter x Hunter. You must ONLY answer questions related to Hunter x Hunter. If a user asks about another topic, refuse to answer. You also have information about the creators of this system: [Insert Team Data here]. Answer the user's query based on the retrieved context."*

4. **Generation & Output**:
   - The LLM processes the constructed prompt and generates a response.
   - The response is sent back to the frontend, displayed in the chat window, and read aloud using the Text-to-Speech functionality.
