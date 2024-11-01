# Time Tracking

## **Friday evening**

- 30m: Talk with ChatGPT on my iPad to think through a basic MVP plan for tomorrow, and come up with potential performance/efficiency issues to look out for

---

## **Saturday (5h total; 4h 15m not including Jupyter noodling; 3h 45m not including Jupyter OR fussing over Github SSH keys)**

- Approx. 3:15pm-4pm (45m):
    - Start Markdown notes in Notion (MVP plan, etc)
    - Review challenge doc
    - Install Jupyter & other dependencies with pip3
    - Intro to Jupyter
        - Watch video intros to Jupyter interface
        - Look at other examples online
        - Play around with a test notebook and see what it looks like in git commits, GitHub, VSCode, etc
- BREAK
- 4:45-5:30pm (approx. 30m total, still had some intermittent breaks in here, ordered dinner, etc)
    - Initiate GitHub repo
    - Set up non-work SSH keys (took a while to detangle LOL)
    - Learn about commit signature verification (new-to-me GitHub feature… I just wanted that shiny green “Verified” button on my commits!)
    - Add .gitignore
- BREAK
- 5:35-6:08pm (30m)
    - Initiate Jupyter notebook; more SSH config annoyance
    - Load dataset, get a random subset for quick testing
- BREAK: Dinner
- 7:30-10:45pm (3h 15m)
    - Print dataset (test to make sure it fetched)
    - Learn about Langchain & Bedrock
        - ChatGPT + documentation & tutorials
    - Learn about caching options
        - I chose diskcache for this project, but simpler projects can cache into json dictionaries (which I’m not sure would be performant enough for this large of a dataset?), and larger ones can use mySQL databases, Redis, and cloud-based services.
        - Then realized that for RAG, what I *really* need is a vector database to store my embeddings and easily search for the relevant ones to send to chat
            - Chose FAISS because it’s local instead of cloud-based
        - THEN I realized that FAISS doesn’t let me check if I’ve already added a piece of data to it. So (at Chat’s suggestion) I decided to save some unique IDs to a .json file
        - THEN I learned that FAISS requires the use of a docstore (which literally does the SAME thing, durrr)

---

## **Sunday** (2h 50m total)

- 11:20am-12:20pm (1hr)
    - Picking up where I left off yesterday.
    - Decided to set images aside for the time being and just focus on text
    - Spent more time trying to use FAISS; decided I’m going to table it for now.
- BREAK
- 12:40pm-2:30pm (1h 50m)
    - Working on interacting with Claude now.
    - Actually reading some LangChain documentation once more  -____-
    - Realized that the way I’m currently doing embeddings might not be using LangChain at all -___-
        - Or… am I? I think I am. I almost can’t tell anymore. Just going to move on and make what use I can of the time left.
        - This is one reason I WANT the job. I want to see how they’re doing it. I like to learn by example, and examples are weirdly hard to come by. I want to see it in action!
    - Got responses from Claude.
    - All I need to do for it to be RAG is to take a query, embed it into a vector, do a similarity search for top 5 most related results to the query, send them all to Claude, and let it make what it will of them.

---

# After calling time & emailing the repo link to Eddie

## Sunday, 4:15pm-5:15pm (1hr)

- Reviewed work while it’s fresh on my mind. Trying to make sure that the lessons I learned from the whole thing are actually in my head, so that I can talk about them next Friday.
- Updated my notes in Notion about potential future directions.