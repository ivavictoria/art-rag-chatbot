# Testing the pipeline (From prelim ChatGPT discussion)

This doc contains snippets that I pasted over from my preliminary planning session with ChatGPT. I saved  the most interesting/relevant bits and put them in my notes so that I could refer to them while coding, in case they could be a source of inspiration.

---

### Example Questions and Expected Responses

1. **"Tell me about Impressionism."**
    - **Expected Response**: The pipeline should retrieve relevant data about Impressionism from the *wikiart-subjects* dataset and generate a response that includes core details, such as its defining characteristics, notable artists, and historical context. The answer could look like:
        
        > "Impressionism was an art movement that originated in France in the late 19th century, characterized by loose brushwork and a focus on capturing light and movement. Notable artists include Claude Monet and Pierre-Auguste Renoir."
        > 
2. **"Who was Vincent van Gogh?"**
    - **Expected Response**: The retriever would pull any entries related to Vincent van Gogh, and the generator would format these into a concise summary:
        
        > "Vincent van Gogh was a Dutch painter known for his expressive use of color and emotional depth. He created iconic works like Starry Night and Sunflowers and is considered one of the most influential figures in Western art."
        > 
3. **"What is Cubism?"**
    - **Expected Response**: For this question, the chatbot should return a brief description of Cubism and mention key figures and techniques.
        
        > "Cubism, pioneered by Pablo Picasso and Georges Braque, was an early 20th-century movement that broke subjects into geometric shapes, offering multiple perspectives within a single plane."
        > 
4. **"What are the characteristics of Baroque art?"**
    - **Expected Response**: This query would prompt the system to retrieve and summarize characteristics typical of Baroque art.
        
        > "Baroque art, which flourished in the 17th century, is known for its dramatic lighting, intense emotion, and dynamic compositions. Artists like Caravaggio and Rembrandt were known for their use of chiaroscuro to create depth and realism."
        > 
5. **"Can you tell me about abstract expressionism?"**
    - **Expected Response**: The system should pull relevant data on Abstract Expressionism and provide an engaging response.
        
        > "Abstract Expressionism was a post-World War II art movement in the U.S., emphasizing spontaneous, abstract forms. Notable artists include Jackson Pollock and Mark Rothko, who explored emotion and color through non-representational techniques."
        > 

---

### Testing Your MVP

To ensure everything is working, you’d want to:

1. **Check Retrieval Accuracy**: Make sure the right data is being retrieved for each topic.
2. **Validate Response Coherence**: Confirm that responses are coherent, informative, and formatted naturally.
3. **Run Variations**: Try rephrasing questions (e.g., “What’s Impressionism?”) to ensure that the model is flexible and handles different phrasings.

## Testing to ensure it’s using the embedded data:

---

### 1. **Test with Dataset-Specific Queries**

- **Pick Unique Dataset Facts**: Choose specific, less-common information that you know exists in the *wikiart-subjects* dataset but isn’t likely to be in the base model’s training data. For instance, if there’s a unique detail about a lesser-known artist or art style in the dataset, ask the chatbot about it.
- **Expected Outcome**: If the response includes this specific detail, it confirms that the retrieval mechanism is working as intended.

### 2. **Inspect Retrieved Context Before Generation**

- In your Jupyter notebook, log or print the retrieved data right before it’s passed to the generator model. This gives you transparency into exactly what information the retrieval step pulls in response to each query.
- **How to Do This**: Add a code cell that outputs the retrieved dataset entries alongside the chatbot’s generated answer. This lets you compare what was retrieved and confirm that the answer incorporates it.

### 3. **Limit Model’s Background Knowledge (If Possible)**

- Temporarily restrict the prompt to only include retrieved information without additional context from the base language model. This helps verify if the retrieved content alone provides enough substance for a coherent answer.
- This won’t work for all models but can be useful if you have the option to control prompting finely.

### 4. **Experiment with Non-Art Queries**

- Ask questions unrelated to art (e.g., “What’s the capital of France?” or “Tell me about climate change”). A well-constructed RAG pipeline should produce responses that reveal it doesn’t have this information in the embedded dataset, making it obvious that the pipeline relies on retrieval rather than generic knowledge.
- **Expected Outcome**: Ideally, the model should either indicate that it doesn’t know the answer (if it’s strictly using the dataset) or provide a generic response, highlighting its reliance on dataset content for relevant responses.

### 5. **Compare Responses with and Without Retrieval**

- Run a few queries where you disable retrieval, forcing the model to rely on its base knowledge. Compare these results to responses with retrieval enabled to see if there’s a clear difference in accuracy, relevance, and specificity.

These steps help ensure the chatbot relies on the dataset for its responses, showing that the RAG pipeline is functioning as intended and not just defaulting to the language model’s general knowledge.