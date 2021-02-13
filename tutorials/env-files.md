# Handling Project Secrets (Python)

Often you'd be having secrets on your project (like database connection details, API access keys etc) that you'd not want to expose to the public by committing it to version control in your source code. 
This can be easily avoided by making use of `.env` files. This short article will guide you through the process of using one to keep your project secrets safe.

## Step 1: Make the `.env` file.
In your project folder, make a file simply called `.env` and fill in your secrets as shown in the example.\ 
_Be careful about the file names, it should **NOT** be `.env.txt`_
![Example `.env` file](/images/env.png)

## Step 2: Install the [`dotenv`](https://pypi.org/project/python-dotenv/) module
This module helps us in accessing the variables declared in our `.env` file.\
Installation can be done with `pip`
```
pip install -U python-dotenv
```

## Step 3: Use the secrets in your code
Now to access the secret in your code, you'd first load the `.env`. After this we can access the secrets from the [`os.environ`](https://docs.python.org/3/library/os.html#os.environ) dictionary, which contains the environment variables.

```py
import dotenv   # this is the module we installed earlier
import os

dotenv.load_dotenv(dotenv.find_dotenv())   

PROJECT_SECRET = os.environ.get("SECRET")
```
[Related Link: Github Secrets](/tutorials/gh-secrets.md)\
[Back to Homepage](https://anand2312.github.io)
