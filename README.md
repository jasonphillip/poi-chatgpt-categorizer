# ChatGPT inside my code, May 2023

In May 2023 I put the OpenAI chat API (`gpt-3.5-turbo`) *inside* a Python data
pipeline, not beside it as a chat window. The job: collapse 2,531 distinct Google
Places `primary_category` strings, from about 94,000 Atlanta businesses, into the
96 standard Google Places types, so a real-estate model could use them. This was
my capstone at NYC Data Science Academy (sponsor: Haystacks.ai), two months after
the ChatGPT API was released (2023-03-01).

`Catergories_POI_Chatgpt.ipynb` (original filename, typo included):

- loads the business table from S3 with `boto3`
- splits the 2,532 category strings into five batches of 500 to fit the 4k-token context
- sends each batch with a system prompt and a fixed output format, `'<business> - <category>'`
- parses the reply into a dict, with an `undefined` bucket for misses
- checks for unexpected and missing keys, then re-runs the leftovers
- second pass with a stricter prompt and `temperature=0.01`
- pickles the mapping to `category_dict.pickle` (included; 2023-05-03 17:08 ET)

The 82 MB Google Places CSV is not included.

## About the dates

This repository was created in October 2026 from my 2023 files. Each commit's
**author date** is the original file's last-modified time on my Mac, preserved
by the backup. The **committer date** is the 2026 import. The only edit to the
2023 code is one line: the hard-coded OpenAI API key was replaced with
`os.environ["OPENAI_API_KEY"]`.

| Commit | Author date (America/New_York) | Source file |
|---|---|---|
| 1 | 2023-05-03 17:18 | Jupyter autosave `.ipynb_checkpoints/Catergories_POI_Chatgpt-checkpoint.ipynb` + `category_dict.pickle` (17:08) |
| 2 | 2023-05-04 14:37 | `Catergories_POI_Chatgpt.ipynb`, final save |

Corroboration outside this repository:

- The capstone's public repository, with my commits from 2023-04-18 to 2023-06-15,
  including one at 17:28 ET on 2023-05-03: https://github.com/schmeeed/capstone
- Enrollment receipt for "ChatGPT Prompt Engineering for Developers"
  (DeepLearning.AI + OpenAI), 2023-04-28, the week the course launched.
- A text I sent at 15:32 ET on 2023-05-03: "I figured out how to incorporate
  chatGPT into my code to put businesses into predefined categories."
- The project write-up, published 2023-06-17:
  https://nycdatascience.com/blog/student-works/demographic-based-real-estate-investing/

## Running it

The notebook uses the 2023 `openai.ChatCompletion` interface (`openai<1.0`).

```
pip install "openai<1.0" pandas boto3 python-dotenv
export OPENAI_API_KEY=...
```
