# Future Directions

This doc was written by me. ChatGPT informed me of some of the detailed options I mention about vector databases, but I wrote these notes as a brainstorm for interview prep.

I started the doc while I was coding, noting features that I wanted to build but did not have the time to dig into. I fleshed it out at the end, right before submitting my code, and a little more over the next day or two as I reflected on the challenge.

While it’s just a brainstorm, I feel like this could be fleshed out/refined for clarity and shared with others in some way, even just on an internal team.

---

## **Experiment with the multimodal embedding model**

- I didn’t explore which embedding model would be best, just out of time restrictions. I was more focused on making sure I could get embeddings at all.
- The multimodal might have been best for the images *and* text, rather than just the images. It was beyond my beginner knowledge, so I chose to leave that as a future step.
    - What are the pros and cons to embedding the images and text *together*, versus embedding each separately?
    - What are the strengths of the text-only embedding models, versus multimodal models?

## **Batch processing for generating embeds for a large dataset**

- When embedding a large dataset, batching requests prevents you from sending one request per item, which would take longer. Instead, you can group items into batches and send each batch as a single request.

## **Cloud vector databases**

- Instead of using FAISS on my local machine, I could use a cloud-based service like Pinecone or WeAviate.
- Or possibly just host Redis or a MySQL database on AWS.

## **Enhanced vector database search indexes and methods**

FAISS alone provides many options that could be experimented with.

- I was initially going to start with a “flat L2 index” (uncompressed vector DB that does brute-force search & comparison of vectors by the Euclidean distance between them) and perform a nearest-neighbor search, where I just find the top X embeddings in my DB that are most similar to the query vector.
- For example, I could instead try a range search, where I gather all embeddings that are relevant up to a certain predetermined threshold.
    - Maybe for one query, 2 embeddings are relevant, and for another query, 15 embeddings are relevant. This is more flexible than always choosing the “top 5”.
- FAISS also supports k-means clustering, which would group similar embeddings into clusters.
    - Clustering could enhance UX by grouping artworks into thematic categories (e.g., styles or genres). If users frequently query for styles like "Impressionism" or "Surrealism," clustering would help create ready-made groups of similar artworks.
- I can also use weights to decide which datapoints are most important in my searches.
- I could also try more advanced FAISS indices to boost search speed or reduce memory usage when scaling to even larger datasets.
    - Hash-based index; tree-based index; IVF index (inverted file indexing for larger datasets); PQ index (product quantization for memory-efficient approximate search); etc….
    - [https://thedataquarry.com/posts/vector-db-3/](https://thedataquarry.com/posts/vector-db-3/)

## **Hosting the Jupyter notebook** so that non-developers could interact with it, if desirable.

- Or making a simple web UI for users to interact with chat, without needing the notebook at all. Really depends on our needs and goals.