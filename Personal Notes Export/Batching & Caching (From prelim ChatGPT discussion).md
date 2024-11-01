# Batching & Caching (From prelim ChatGPT discussion)

This doc contains snippets that I pasted over from my preliminary planning session with ChatGPT. I saved  the most interesting/relevant bits and put them in my notes so that I could refer to them while coding, in case they could be a source of inspiration.

---

### Indicators of Long Runtime:

- **Excessive API Calls**: If the pipeline repeatedly calls the language model or embedding API, it could quickly rack up costs and extend runtime.
- **Large or Inefficient Data Retrieval**: Retrieving large datasets without filtering or narrowing down results can slow down processing, especially if embeddings aren’t optimized.
- **High Latency in Embedding or Generation**: Both embedding and generation steps involve external models that can add latency, particularly if they’re handling large data inputs or complex queries.

### Parts of the Pipeline that Could Cause Long Runtime:

1. **Embedding Step**:
    - **Potential Issue**: If you’re embedding large amounts of data without batching or caching, it can slow down the process significantly.
    - **Optimization**: Try batching embeddings or using in-memory storage for smaller, reusable chunks.
2. **Retrieval Step**:
    - **Potential Issue**: Inefficient retrieval, especially from large datasets, can add unnecessary overhead.
    - **Optimization**: Index embeddings in a vector store, which speeds up similarity searches, or use filtering to reduce the search space.
3. **Generation Step**:
    - **Potential Issue**: Each generation request to the language model incurs time and cost, especially if running on multiple queries or in multiple iterations.
    - **Optimization**: Limit unnecessary calls by caching responses or batching requests where possible. You can also simplify the prompt to reduce token usage if that’s an option.
4. **Data Logging and Testing**:
    - **Potential Issue**: Logging or testing with overly large datasets or repetitive requests could contribute to slowdowns.
    - **Optimization**: For testing, use a smaller, representative subset of the dataset rather than running exhaustive tests on the entire data.

If you hit a long-running issue, check these parts first. Optimizing them can ensure a fast, cost-effective MVP that meets the requirements without excessive processing.

---

For this project, **batching** and **caching** can help keep the runtime efficient, and you can definitely incorporate these techniques. Here’s how each might work in your pipeline:

### 1. **Batching**

- **Embedding Batching**: If you need to embed a large dataset, batching requests prevents you from sending one request per item, which would take significantly longer. Instead, you group items into batches and send each batch as a single request.
- **How to Implement**: Most APIs, including AWS Bedrock via Boto3, support batching in requests. For example, when embedding, you could batch items in groups of, say, 10–20 entries, depending on API limitations and response speed. In Python, you could:
    - Use a `for` loop with `range` or libraries like `itertools` to create batches.
    - Send each batch as a single request to the embedding model, then store or save the results before moving to the next batch.

### 2. **Caching**

- **What to Cache**: If certain parts of the data are used repeatedly (like specific embeddings or retrieval responses), caching can save you from redundant processing.
- **Implementing a Simple Cache**:
    - **Local Variable Cache**: For quick caching, you could store frequently used results in a dictionary, mapping queries or embeddings to responses.
    - **File-Based Cache**: For larger data, consider saving embeddings or responses to a local file (like JSON or pickle files) so you can reload them instead of recalculating.
    - **In-Memory Cache (Using Libraries)**: For advanced caching, libraries like `cachetools` offer options for caching in-memory with policies like time-based expiration.

### How Boto3 Can Help

While **Boto3 doesn’t directly manage batching and caching**, you can use it flexibly within your own batching and caching logic:

- **Batch API Calls**: Boto3 works well with looping or batching, so you can control how many items you send per call by structuring your loops and requests.
- **Efficient Resource Use**: Because Boto3 sessions are stateless, you can batch requests under one session to optimize API calls while respecting rate limits.

**Summary**:

- **Batching** reduces the number of individual API requests, making embedding and generation steps more efficient.
- **Caching** prevents redundant calculations, saving time on repeated data use.

Together, these techniques will help you avoid long-running jobs and stay within your allotted runtime and cost limits.

---

Starting with a subset of your data is an excellent approach for initial development, and the **Hugging Face `datasets` library** makes this super easy without needing to hand-craft anything. You can pull a smaller portion of the *wikiart-subjects* dataset to work with, which will keep things efficient as you develop and test.

Using a subset from the `datasets` library is both quick and memory-efficient, and it lets you scale up gradually. Once your pipeline works well with the small sample, you can expand to the full dataset or larger subsets when you’re ready to test performance.

This approach will keep your development focused and minimize processing time until you’re ready for the full dataset.

### How to Use a Subset with the `datasets` Library

You can load a random sample or a specific slice of the dataset directly using `datasets`. Here’s how:

- **Load a Random Sample**:
    - You can load a random subset using the `train_test_split` method, which allows you to split off, say, 5–10% of the data.
    - Example:This approach gives you a small, manageable sample for initial testing.
        
        ```python
        dataset = load_dataset("jlbaker361/wikiart-subjects")
        small_dataset = dataset["train"].train_test_split(test_size=0.1)["test"
        ```
        
- **Load a Fixed Number of Entries**:
    - If you want an exact number of entries (e.g., the first 100), you can use slicing.
    - Example:
        
        ```python
        small_dataset = dataset["train"].select(range(100))
        ```
        
- **Load by Specific Criteria** (Optional):
    - If the dataset has metadata (e.g., art style or artist name), you could filter by specific criteria to work with certain types of data.
    - Example:
        
        ```python
        filtered_dataset = dataset["train"].filter(lambda x: x["style"] == "Impressionism")
        ```
        

---