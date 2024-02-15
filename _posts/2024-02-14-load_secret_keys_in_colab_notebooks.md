# Access Keys in Google Colab Notebook

### What not to do:

```python
# DO NOT do this - most useful recommendation when googled as of Feb 2024
from openai import OpenAI
client = OpenAI(
    api_key = "xxxxxx" # paste your api key here
)
```

### What to do:

```python
# Mounting Google Drive to a Colab notebook
from google.colab import drive
drive.mount('/content/drive')

# Read and copy Key from the key file
with open('/content/drive/My Drive/path_to_directory_containing_api_key_txt_file/openai_api_key.txt', 'r') as file:
    api_key = file.read().strip()
    
# Use the api_key to define the openai client
from openai import OpenAI
client = OpenAI(
    api_key = openai_api_key
)
```

<script src="https://gist.github.com/vamsiuppala/e4783d2e7133456901bbb12e0bc8203c.js"></script>
