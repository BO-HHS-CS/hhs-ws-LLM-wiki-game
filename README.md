# HHS CS Workshop: Wikipedia Game with LLMs
Learning prompt engineering through a program that plays the Wikipedia game. Built off previous [workshop](https://github.com/BaroqueObama/hhs-ws-wiki-game).  
The [Wikipedia game](https://en.wikipedia.org/wiki/Wikipedia:Wiki_Game) involves trying to navigate from a "start" Wikipedia page to a "goal" Wikipedia page by traversing pages through clicking wikilinks. Fastest wins.  
The code prompts the user for the starting and ending Wikipedia page. It then attempts to find a path between those two pages using BFS.  
## Code
### WikiGame Class
- Begins with start link/topic and goal link/topic.
- Iterate BFS:
  - Use BS4 to get a list of wikilink topics from Wikipedia page.
  - If the goal topic isn't found:
    - Prompts LLM to pick top `n` topics that relate closest to the goal topic.
    - Add those topics to the queue to search through.
  - If goal topic is found:
    - Return path from start to goal topic.
### LLM Prompting
Uses LangChain to run `Replicate` models. LLM used is the Llama 34 billion parameters.  
The LLM takes in a list of topics scraped from a Wikipedia page and should return the top `n` topics that relate closest to the goal topic.  
Sample prompt that is to be improved upon:
```py
f"""
From the following list of topics [{list_of_topics}], pick up to {num_of_topics} that relate most closely about {goal_topic}.
Your response should include no explanation. The list of topics MUST be formatted in square brackets [] separated by commas.
DO NOT respond in bullet point lists or numbered lists.
The wording of the topics (including the underscores) must be exactly as given.
"""
```