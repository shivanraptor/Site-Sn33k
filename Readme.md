# Website Scraper, And PDF Chunker/Vectorizer for Pinecone DB

This Python repository contains a set of scripts that allow you to scrape a website, clean the data, organize it, chunk it, and then vectorize it. The resulting vectors can be used for various machine learning tasks such as similarity search or clustering. The script can consume PDFs and add them to the training data too.

## Files
**THESE FUNCTIONS CONSUME THE FILES THEY PROCESS (only in the `websites/` and `pdfs/` directories)**

- `cleaner.py`: This script downloads a website using `wget`, reads and cleans the HTML files using Beautiful Soup, and saves the resulting text files in a specified directory.
- `chunker.py`: This script splits the text files into smaller chunks, using a recursive character-based text splitter. The resulting chunks are saved in a JSONL file.
- `vectorizor.py`: This script loads the JSONL file, creates embeddings using OpenAI's `text-embedding-ada-002` model, and indexes the embeddings using Pinecone. This is the one script you need API keys to run.
- `pdf-muncher.py`: This will consume every PDF you put in the `pdfs/` folder, and add it to the vectorized `train.jsonl`.

## Requirements

- Python 3.x
- OpenAI API key
- Pinecone API key
- `bs4` Python library
- `jsonlines` Python library
- `tqdm` Python library
- `tiktoken` Python library
- `pinecone-client` Python library

## Workflow

1. Clone the repository and navigate to the project directory.

2. Install the required Python libraries using `pip install -r requirements.txt`.

3. Set up your OpenAI and Pinecone API keys.

4: Download the website - copy and run the wget command: 
  `wget -r -np -nd -A.html,.txt,.tmp -P websites https://www.linkedin.com/in/sean-stobo/`

5. Run `python cleaner.py` to download and clean the website data. This will break down the directory structure into a list of HTML docs.

6. Run `python chunker.py` to split the text files into smaller chunks. This outputs `train.jsonl` in the root folder.

6.5. (Optional) Run `python pdf-muncher.py` to convert the contents of the `/pdfs/` folder to the serialized `train.jsonl` file in the root folder. 

7. Run `python vectorizor.py` to create embeddings and index them using Pinecone. This will vectorize `train.jsonl` to the Pinecone index. The index with 1536 dimensions will be created during the `vectorizer.py`.


## Usage (an example)

We choose a site to rip:
![Step 1](data/pdf-1.png)

We see our `websites/` folder filling up with files:
![Step 2](data/pdf-2.png)

We run the cleaner script (`cleaner.py`):
![Step 3](data/pdf-3.png)

Files are normalized and cleaned up:
![Step 4](data/pdf-4.png)

Now we run the chunker (`chunker.py`), and we can see our website files are now chunked and vectorized:
![Step 5](data/pdf-5.png)

Now we run our PDF muncher, which will consume the Dungeons and Dragons monster manual PDF in our `pdfs/` folder:
![Step 6](data/pdf-muncher.png)

Finally, we can see our vectorized training data contains the DnD content too:
![Step 7](data/pdf-6.png)

Now we run the vectorizer (`vectorizer.py`) and our Pinecone DB will get updated.


## Notes

- The fork supports the Pinecone SDK 5.x and fixes some bugs, especially the `train.jsonl` formatting and Pinecone Index creation.
