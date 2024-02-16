# Access Keys in Google Colab Notebook

Accessing secret keys in our usual desktop file based development environments is just storing the secret keys in config / txt files and calling them in directly. 
But there is no direct file system that a Google Colab notebook refers to, unless you ask them to.

### Dont' Do This
The first few search results when you try to find a way to import API keys prompt you to directly paste them into the notebook / python file. Pasting in keys in your scripts are risky and should be avoided.

```python
# DO NOT do this
from openai import OpenAI
client = OpenAI(
    api_key = "xxxxxx" # paste your api key here
)
```

### Do This
To provide the Colab notebook a file system, you have to first mount Google Drive to the Colab Notebook.

Mounting Google Drive to a Colab notebook allows you to access files stored in your Google Drive directly from within the notebook environment. 

It essentially establishes a connection between your Google Drive storage and the Colab notebook, enabling seamless file operations such as reading, writing, and modifying files.

When you mount Google Drive to a Colab notebook, you'll be prompted to authorize access to your Google Drive account. Once authorized, you'll have access to your Google Drive files and directories directly from your notebook.

```python
# Mounting Google Drive to a Colab notebook
from google.colab import drive
drive.mount('/content/drive')
```

This is particularly useful when you need to work with datasets, files, or other resources stored in your Google Drive, as it eliminates the need to manually upload or download files each time you want to work with them in Colab.

You can then read the API key and use it.

```python
# Read and copy Key from the key file
with open('/content/drive/My Drive/path_to_directory_containing_api_key_txt_file/openai_api_key.txt', 'r') as file:
    api_key = file.read().strip()

# Use the api_key to define the openai client
from openai import OpenAI
client = OpenAI(
    api_key = openai_api_key
)
```
