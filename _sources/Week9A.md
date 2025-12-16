# Week 9a Working with Online Data Stores and APIs in Python

## Introduction

In today's world, data doesn't live in isolation on a single computer. Weather forecasts, social media posts, financial transactions, and countless other types of information are stored on servers around the world. As a programmer, you need to know how to access and work with this data. This reading will introduce you to two fundamental concepts: **Application Programming Interfaces** (aka APIs) and online data stores.

## What Are APIs and Why Do We Need Them?

Imagine you're at a restaurant. You don't walk into the kitchen and make your own food—instead, you tell a waiter what you want, and they bring it to you. An **API** works the same way: it's an intermediary that lets your program request data or services from another system without needing to know how that system works internally.

### Real-World Examples of APIs

APIs are everywhere in modern applications:

- **ABSOLUTELY EVERYWHERE** Below is a list of some ones you might personally use early on in your careers, but the first question anyone asks about your new software project is "Where is your API?" and "How can I get API access?"
- **Weather apps** use APIs from meteorological services to get current conditions and forecasts
- **Social media integrations** (like "Sign in with Google") use APIs to authenticate users
- **E-commerce sites** use payment APIs (like Stripe or PayPal) to process transactions
- **Map applications** use APIs to get directions, traffic data, and location information
- **News aggregators** use APIs to pull articles from multiple sources

Without APIs, every developer would need to recreate these services from scratch, which would be inefficient and often impossible.

## Understanding REST APIs and HTTP

Most modern web APIs follow a pattern called **REST** (Representational State Transfer). REST APIs use the same protocol that web browsers use to load websites: **HTTP** (Hypertext Transfer Protocol).

### HTTP Methods: The Verbs of the Web

When you interact with a REST API, you use different HTTP methods to specify what action you want to perform:

- **GET**: Retrieve data (like viewing a webpage)
- **POST**: Create new data (like submitting a form)
- **PUT**: Update existing data completely
- **PATCH**: Update part of existing data
- **DELETE**: Remove data

For most data retrieval tasks—which is what we'll focus on—you'll use **GET** requests.

### Anatomy of an API Request

An API request typically includes several components:

1. **Base URL**: The address of the API server (e.g., `https://api.weather.com`)
2. **Endpoint**: The specific resource you want (e.g., `/current`)
3. **Parameters**: Additional information about your request (e.g., `?city=Boston`)
4. **Headers**: Metadata about your request (e.g., authentication tokens)

Put together, a complete API request might look like:
```
https://api.weather.com/current?city=Boston&units=metric
```

## How to Learn About a Specific API: Reading Documentation

Before you can use any API, you need to understand what it offers and how to use it. Every API has **documentation**—a guide that explains its endpoints, parameters, authentication requirements, and response formats. Learning to read API documentation is one of the most important skills for working with APIs.

### What API Documentation Tells You

Good API documentation typically includes:

1. A FULLY WORKING EXAMPLE. For reasons that are difficult to understand, many API providers do not give you a fully working reference example, which just adds time and ambiguity. If/when you write your own API, don't be that person

2. **Base URL**: Where the API is hosted (e.g., `https://api.openweathermap.org/data/2.5`)

3. **Available Endpoints**: The specific paths you can access (e.g., `/weather`, `/forecast`, `/users`)

4. **HTTP Methods**: Which methods each endpoint accepts (GET, POST, etc.)

5. **Parameters**: What information you can or must include in your request:
   - **Required parameters**: Must be included or the request fails
   - **Optional parameters**: Customize the response but aren't necessary
   - **Parameter types**: String, number, boolean, etc.
   - **Parameter location**: Query string, URL path, headers, or request body

6. **Authentication**: How to prove you're authorized to use the API (API keys, OAuth, etc.)

7. **Response Format**: What data structure you'll receive back, usually with examples

8. **Error Codes**: What different status codes mean for that specific API

9. **Rate Limits**: How many requests you can make per minute/hour/day

### Example: Reading OpenWeatherMap Documentation

Let's walk through a real example. If you visit OpenWeatherMap's API documentation, you'll find information like this:

**Endpoint**: Current Weather Data  
**URL**: `https://api.openweathermap.org/data/2.5/weather`  
**Method**: GET

**Required Parameters**:
- `appid` (string): Your API key
- One of:
  - `q` (string): City name (e.g., "Boston" or "Boston,US")
  - `lat` and `lon` (numbers): Geographic coordinates
  - `zip` (string): ZIP code

**Optional Parameters**:
- `units` (string): "standard", "metric", or "imperial" (default: "standard")
- `lang` (string): Language for weather descriptions (default: "en")

**Example Response**:
```json
{
  "weather": [{"main": "Clear", "description": "clear sky"}],
  "main": {
    "temp": 72.5,
    "feels_like": 70.2,
    "humidity": 45
  },
  "name": "Boston"
}
```

From this documentation, you know exactly how to construct your request!

### Where to Find API Documentation

Most APIs host their documentation on their website. Common places to look:

- **Company website**: Often at `https://company.com/api` or `https://developers.company.com`
- **Documentation subdomains**: `https://docs.company.com` or `https://api.company.com/docs`
- **README files**: For APIs hosted on GitHub or similar platforms
- **API reference pages**: Searchable lists of all endpoints


### Tips for Reading API Documentation

1. **Start with the "Getting Started" or "Quickstart" guide**: Don't dive into the full reference immediately
2. **Look for code examples**: Good documentation includes sample code in multiple languages
3. **Check the version**: APIs evolve, so make sure you're reading docs for the current version
4. **Note the date format**: Different APIs use different date/time formats (ISO 8601, Unix timestamps, etc.)
5. **Understand the authentication flow**: This is often the most confusing part, so read it carefully
6. **Look for SDKs or libraries**: Many APIs provide official Python libraries that simplify usage

### What If Documentation Is Poor or Missing?

Not all APIs have great documentation. If you encounter this:

- **Look for community tutorials**: Other developers may have written guides
- **Check Stack Overflow**: Search for questions about the specific API
- **Ask Chat**: GenAI can be very useful, but be careful because GenAI's understanding of the API can be behind by several versions
- **Experiment carefully**: Make simple test requests and examine the responses
- **Use browser developer tools**: If there's a web interface, watch the network requests it makes
- **Contact support**: Many API providers have developer support channels

## Making API Requests with Python's `requests` Library

Python makes working with APIs straightforward using the `requests` library. This library handles all the complexity of HTTP communication for you.

### Basic GET Request

Here's a simple example using a public API that provides random cat facts:

```python
import requests

# Make a GET request
response = requests.get('https://catfact.ninja/fact')

# Check if the request was successful
if response.status_code == 200:
    # Parse the JSON data
    data = response.json()
    print(data['fact'])
else:
    print(f"Error: {response.status_code}")
```

The `status_code` tells you whether the request succeeded. A code of 200 means success; codes in the 400s indicate client errors (like bad requests), and codes in the 500s indicate server errors.

### Working with JSON Data

Most APIs return data in **JSON** (JavaScript Object Notation) format. JSON is text-based and uses a structure similar to Python dictionaries and lists. Understanding how to work with JSON is crucial because it's the lingua franca of web APIs—almost every modern API uses it.

Here's an expanded introduction with more detail:

---

**JSON Structure**

JSON (JavaScript Object Notation) is a lightweight, text-based format for storing and exchanging data. Originally derived from JavaScript, it has become the dominant format for web APIs because of its simplicity, human readability, and language independence. Nearly every modern programming language includes native or library support for parsing and generating JSON.

**Basic Structure**

JSON data is built from two fundamental structures:

*Objects* are unordered collections of key-value pairs enclosed in curly braces `{}`. Each key must be a string enclosed in double quotes, followed by a colon, then the value. Multiple pairs are separated by commas:

```json
{
  "name": "Alice",
  "age": 28,
  "email": "alice@example.com",
  "active": true
}
```

*Arrays* are ordered lists of values enclosed in square brackets `[]`. Values are separated by commas and can be of any type:

```json
["apple", "banana", "cherry", "date"]
```

```json
[1, 2, 3, 5, 8, 13]
```

**Data Types**

JSON supports exactly six data types:

- **String**: Text enclosed in double quotes. Special characters are escaped with backslashes (`"Hello\nWorld"` for a newline).
- **Number**: Integers or floating-point values without quotes (`42`, `3.14`, `-17`, `2.5e10`). JSON doesn't distinguish between integers and floats.
- **Boolean**: Either `true` or `false` (lowercase, no quotes).
- **Null**: Represents the absence of a value, written as `null`.
- **Object**: A collection of key-value pairs as described above.
- **Array**: An ordered list of values as described above.

**Nested Structures**

The power of JSON comes from nesting objects and arrays to represent complex, hierarchical data:

```json
{
  "user": {
    "id": 12345,
    "name": "Alice Johnson",
    "email": "alice@example.com",
    "preferences": {
      "theme": "dark",
      "notifications": true
    }
  },
  "orders": [
    {
      "id": 101,
      "date": "2024-01-15",
      "items": ["laptop", "mouse"],
      "total": 1299.99
    },
    {
      "id": 102,
      "date": "2024-02-03",
      "items": ["keyboard"],
      "total": 89.50
    }
  ],
  "orderCount": 2
}
```

This example shows objects nested within objects, arrays containing objects, and arrays containing strings—all valid JSON structures.

**Syntax Rules**

JSON is strict about formatting:
- Keys must always be strings in double quotes (not single quotes)
- No trailing commas after the last item in an object or array
- No comments are allowed in standard JSON
- All strings must use double quotes, not single quotes
- Values must be one of the six data types (no functions, dates, or undefined values)

**Why JSON for APIs and Data Stores**

JSON's structure maps naturally to data structures in most programming languages. Objects become dictionaries, hash maps, or objects; arrays become lists or arrays. This makes JSON trivial to parse and generate programmatically.

When a REST API returns data, it typically sends JSON that applications can immediately deserialize into native data structures. A Python application converts JSON to dictionaries and lists; a JavaScript application converts it to objects and arrays. This seamless transformation eliminates the need for custom parsing logic.

For data stores, JSON and JSON-like formats (like BSON in MongoDB) offer flexibility. Unlike rigid table schemas in relational databases, JSON documents can have varying structures, making them ideal for evolving data models. Yet the format remains human-readable, simplifying debugging, logging, and configuration files.

**Common Use Cases**

- API responses and requests (RESTful services almost universally use JSON)
- Configuration files (package.json, settings files)
- NoSQL databases (MongoDB stores data as BSON, a binary form of JSON)
- Data interchange between microservices
- Log files and structured logging
- Web storage (localStorage and sessionStorage in browsers)

---


#### Converting JSON to Python

Python's `requests` library automatically converts JSON into Python data structures when you call `.json()` on a response:

```python
import requests

response = requests.get('https://api.example.com/weather')
data = response.json()  # Converts JSON to Python dict

# Now you can access data like any Python dictionary
print(data['city'])              # Output: Boston
print(data['temperature'])       # Output: 72
print(data['forecast'][0])       # Output: sunny
print(data['details']['wind_speed'])  # Output: 12
```

#### Accessing Nested Data

Real-world API responses often have deeply nested structures. Here's how to navigate them:

```python
# Sample API response converted to Python
weather_data = {
    "location": {
        "name": "Boston",
        "coordinates": {
            "latitude": 42.36,
            "longitude": -71.06
        }
    },
    "current": {
        "temperature": 72,
        "conditions": ["sunny", "windy"]
    }
}

# Accessing nested values
city_name = weather_data['location']['name']
latitude = weather_data['location']['coordinates']['latitude']
first_condition = weather_data['current']['conditions'][0]

print(f"{city_name} is at latitude {latitude}")
print(f"Current condition: {first_condition}")
```

#### Handling Missing Data Safely

Sometimes API responses don't include all fields, or they might be `None`. Use the `.get()` method to avoid errors:

```python
# Unsafe way - will crash if 'humidity' doesn't exist
humidity = data['humidity']

# Safe way - returns None if key doesn't exist
humidity = data.get('humidity')

# Safe way with a default value
humidity = data.get('humidity', 0)

# For nested data
wind_speed = data.get('details', {}).get('wind_speed', 0)
```

#### Working with JSON Arrays

When an API returns a list of items, you'll need to iterate through them:

```python
import requests

# Get a list of users from an API
response = requests.get('https://jsonplaceholder.typicode.com/users')
users = response.json()  # This is a list of dictionaries

# Process each user
for user in users:
    name = user['name']
    email = user['email']
    city = user['address']['city']
    print(f"{name} from {city}: {email}")

# Or use list comprehension to extract specific data
all_emails = [user['email'] for user in users]
print(all_emails)
```

#### Pretty Printing JSON

When you're exploring a new API, it helps to see the structure clearly:

```python
import requests
import json

response = requests.get('https://api.example.com/data')
data = response.json()

# Pretty print with indentation
print(json.dumps(data, indent=2))
```

This will display the JSON in a readable format:
```
{
  "city": "Boston",
  "temperature": 72,
  "details": {
    "wind_speed": 12
  }
}
```

#### Converting Python to JSON

Sometimes you need to send data to an API. The `requests` library handles this automatically:

```python
import requests

# Python dictionary you want to send
new_post = {
    "title": "My First Post",
    "body": "This is the content",
    "userId": 1
}

# requests automatically converts dict to JSON
response = requests.post(
    'https://jsonplaceholder.typicode.com/posts',
    json=new_post  # Note: use 'json=' parameter
)

print(response.json())  # See the API's response
```

## API Authentication: Keeping Data Secure

Many APIs require authentication to prevent abuse and track usage. Think of authentication as showing your ID before accessing a service.

### API Keys

The most common authentication method is an **API key**—a unique string that identifies you to the API. To read more about obtaining and using API keys, check out the appendix to this reading.



## Database as a Service (DBaaS): Structured Data in the Cloud

While cloud storage is great for files, **Database as a Service** platforms specialize in storing structured data—information organized in tables, documents, or other formats optimized for querying and analysis.

### Types of Cloud Databases

Cloud databases come in different flavors:

- **Relational databases** (like PostgreSQL, MySQL): Data organized in tables with relationships
- **Document databases** (like MongoDB): Data stored as JSON-like documents
- **Key-value stores** (like Redis): Simple pairs of keys and values for fast access

### Why Use Database as a Service?

Setting up and maintaining your own database server requires significant expertise and resources. DBaaS providers handle:

- **Server management**: No need to configure or maintain hardware
- **Automatic backups**: Your data is regularly saved
- **Scaling**: The database grows with your needs
- **Security**: Professionals manage security patches and updates

### Accessing Cloud Databases

Most cloud databases can be accessed using Python libraries specific to the database type. Here's a simplified example using a hypothetical API:

```python
import requests

# Query a cloud database via its API
response = requests.get(
    'https://api.database-service.com/query',
    headers={'Authorization': f'Bearer {api_key}'},
    params={'table': 'users', 'limit': 10}
)

users = response.json()
for user in users['data']:
    print(user['name'], user['email'])
```

## Best Practices for Working with APIs and Cloud Services

As you start working with online data stores and APIs, keep these principles in mind:

1. **Read the documentation**: Every API is different. Always check the official docs for endpoints, parameters, and authentication methods.

2. **Handle errors gracefully**: Networks fail, servers go down, and rate limits get exceeded. Always check status codes and handle potential errors.

3. **Respect rate limits**: Many APIs limit how many requests you can make per hour or day. Exceeding these limits can get you temporarily blocked.

4. **Cache when possible**: If data doesn't change frequently, store it locally instead of requesting it repeatedly.

5. **Keep credentials secure**: Never hardcode API keys in your source code or commit them to version control.

6. **Test with small datasets first**: When working with new APIs, start with small requests to understand the response format before processing large amounts of data.

## Example APIs You Can Use Right Now

To help you get started, here are some beginner-friendly APIs you can experiment with. These examples range from completely free with no authentication to those requiring simple (free) API keys.

### 1. JSONPlaceholder - Practice API (No Authentication)

**What it does**: Provides fake data for testing and prototyping (posts, comments, users, photos, etc.)

**Base URL**: `https://jsonplaceholder.typicode.com`

**Example usage**:
```python
import requests

# Get a list of users
response = requests.get('https://jsonplaceholder.typicode.com/users')
users = response.json()

for user in users[:3]:  # Show first 3 users
    print(f"{user['name']} - {user['email']}")

# Get a specific post
response = requests.get('https://jsonplaceholder.typicode.com/posts/1')
post = response.json()
print(f"\nPost title: {post['title']}")
```

### 2. Open-Meteo - Weather Data (No Authentication)

**What it does**: Provides weather forecasts and historical weather data

**Base URL**: `https://api.open-meteo.com/v1`

**Example usage**:
```python
import requests

# Get weather for a location (latitude, longitude)
params = {
    'latitude': 42.36,
    'longitude': -71.06,  # Boston coordinates
    'current_weather': 'true'
}

response = requests.get(
    'https://api.open-meteo.com/v1/forecast',
    params=params
)

weather = response.json()
current = weather['current_weather']
print(f"Temperature: {current['temperature']}°C")
print(f"Wind speed: {current['windspeed']} km/h")
```

### 3. REST Countries - Country Information (No Authentication)

**What it does**: Provides data about countries (population, languages, currencies, flags, etc.)

**Base URL**: `https://restcountries.com/v3.1`

**Example usage**:
```python
import requests

# Get information about a specific country
response = requests.get('https://restcountries.com/v3.1/name/japan')
countries = response.json()

japan = countries[0]
print(f"Capital: {japan['capital'][0]}")
print(f"Population: {japan['population']:,}")
print(f"Region: {japan['region']}")
```

## Conclusion

APIs and online data stores are fundamental to modern software development. They allow your programs to access vast amounts of data and powerful services without building everything from scratch. As you practice working with these technologies, you'll discover that most of the programming challenges you'll face in the real world involve integrating different systems and services—and now you have the foundational knowledge to do exactly that.


## Key Terms to Remember

- **API**: Application Programming Interface—a way for programs to communicate
- **REST**: A common pattern for web APIs using HTTP
- **HTTP methods**: GET, POST, PUT, PATCH, DELETE
- **JSON**: JavaScript Object Notation—a text format for data exchange
- **API key**: A unique identifier for authentication
- **Cloud storage**: File storage on remote servers accessible via the internet
- **DBaaS**: Database as a Service—managed database hosting
- **Status code**: A number indicating the result of an HTTP request (200 = success)
