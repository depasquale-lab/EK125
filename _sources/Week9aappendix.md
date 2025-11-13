# Week 9a Appendix How to Get an API Key

You don't automatically have an API key—you need to obtain one from the API provider. The typical process looks like this:

1. **Create an account**: Visit the API provider's website and sign up (usually free for basic usage)
2. **Navigate to the developer section**: Look for links like "API", "Developers", "Dashboard", or "Console"
3. **Generate or view your key**: There's usually a button like "Create API Key", "Generate Key", or "Show API Key"
4. **Copy and save your key**: This is typically a long string of random characters like `a3f5k9m2p7q1r8s4t6v9w2x5y8z1b4c7`
5. **Read the terms**: Note any rate limits or usage restrictions

**Example: Getting an OpenWeatherMap API Key**
1. Go to openweathermap.org
2. Click "Sign Up" or "API Keys"
3. Create a free account
4. Navigate to your account dashboard
5. Find the "API Keys" tab
6. Copy your default key (or generate a new one)

#### Important Notes About API Keys

- **Keep them secret**: API keys are like passwords. Never share them publicly or commit them to GitHub
- **One key per service**: Each API provider gives you a different key
- **Free tier limitations**: Free API keys often have usage limits (e.g., 1,000 requests per day)
- **Key activation time**: Some services take a few minutes to activate new keys
- **Multiple keys**: Some services let you create multiple keys for different projects

#### Using API Keys in Python

Here's how you typically use an API key once you have it:

```python
import requests

# Your unique API key (never share this publicly!)
api_key = "a3f5k9m2p7q1r8s4t6v9w2x5y8z1b4c7"

# Include the key in your request
response = requests.get(
    'https://api.example.com/data',
    params={'api_key': api_key}
)
```

Some APIs expect the key in the URL parameters (as shown above), while others want it in the request headers:

```python
headers = {'Authorization': f'Bearer {api_key}'}
response = requests.get(
    'https://api.example.com/data',
    headers=headers
)
```

The API documentation will tell you exactly where to put your key.

#### Storing API Keys Safely

**Never do this**:
```python
# DON'T hardcode your actual key in your code!
api_key = "a3f5k9m2p7q1r8s4t6v9w2x5y8z1b4c7"
```

**Better approaches**:

1. **Use environment variables**:
```python
import os

api_key = os.environ.get('OPENWEATHER_API_KEY')
if not api_key:
    print("Error: API key not found in environment variables")
```

2. **Use a configuration file** (add this file to `.gitignore`):
```python
# In config.py (don't commit this file!)
OPENWEATHER_API_KEY = "your_key_here"

# In your main program
from config import OPENWEATHER_API_KEY
api_key = OPENWEATHER_API_KEY
```

3. **For this class**: Your instructor may provide specific guidance on how to store keys for coursework

## Cloud Storage: Your Files in the Sky

**Cloud storage** refers to saving files on remote servers that you can access from anywhere with an internet connection. Services like Google Drive, Dropbox, and Amazon S3 provide cloud storage with APIs that let your programs upload, download, and manage files programmatically.

### Why Use Cloud Storage?

Cloud storage offers several advantages over storing files locally:

- **Accessibility**: Access your files from any device, anywhere
- **Collaboration**: Multiple people can work with the same files
- **Backup**: Your data is protected against local hardware failures
- **Scalability**: Store as much as you need without buying new hard drives
- **Integration**: Easy to connect with other online services

### Example: Accessing Files from Cloud Storage

Most cloud storage services provide Python libraries. Here's a conceptual example of how you might download a file:

```python
# This is pseudocode - actual implementation varies by service
import cloud_storage_library

# Authenticate with your credentials
client = cloud_storage_library.connect(api_key='your_key')

# List available files
files = client.list_files()
print(files)

# Download a specific file
client.download_file('data.csv', local_path='./data.csv')

```

