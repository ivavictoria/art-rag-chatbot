# MVP Roadmap (From prelim ChatGPT discussion)

The rough MVP plan below was co-written by ChatGPT and I after back-and-forth deliberation (via my iPad on a Friday night).

My primary concerns were getting started with new tools *and* an AI process I’ve never worked with before, and wanted to get *something* working while learning in the process. I went into the discussion very concerned about the large dataset size and wanting to restrict how heavily I use the API, to avoid excessive costs while experimenting.

I wanted to walk away with a plan that would remind me of the order in which I can work, so that I don’t get too hung up on any particular step in the limited time allowed.

---

### Preliminary Setup

- **Install Jupyter and Required Libraries**
    - Install Jupyter Notebook and set up the notebook file in the repo.
    - Install `langchain`, `datasets`, and `boto3` dependencies.
- **Create GitHub Repository**
    - Initialize a new repository with a `README.md` and `.gitignore`.
    - Add setup instructions to the README.
- **Configure AWS Credentials**
    - Set up credentials securely to access Bedrock, avoiding committing sensitive data.
- **Verify Environment**
    - Test that all imports work and AWS connection is successful.
- **Initial Commit to GitHub**
    - Commit README, `.gitignore`, and initial notebook skeleton to GitHub.

### Build the Pipeline

1. **Set Up the Dataset and Embeddings**
    - Load the dataset and create embeddings to use for retrieval.
    - **Batch Embeddings**: Use batching to process the dataset embeddings in manageable chunks to avoid long runtimes and high API costs.
2. **Implement the Retrieval Function**
    - Develop the retrieval mechanism and log retrieved data for verification.
    - **Cache Frequently Retrieved Data**: Implement a simple caching mechanism (e.g., dictionary or file-based) to store results for common queries or repeat requests, reducing redundant processing.
3. **Set Up the Generation Step**
    - Pass retrieved data to the language model and generate responses.
4. **Add the Verification Step to Check Dataset Dependency**
    - Test and log retrieved data to confirm it is used in responses.
5. **Test with a Range of Questions**
    - Run various queries to verify pipeline accuracy and relevance.
6. **Document Future Directions**
    - Outline ideas for usability, scalability, advanced caching, or hosted access for non-technical users.