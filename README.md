# python-AI-user-interface
🔍 Secure Account-Based Web Search Application

A Python command-line application that combines a secure user account system with powerful web search capabilities using the LangSearch API. Users can create accounts, log in securely, and perform customizable web searches with filters for result count and content freshness.

✨ Features

Secure User Authentication

Account creation and login system
Password hashing using PBKDF2-HMAC-SHA256
Random salt generation for enhanced security
Protection against timing attacks using hmac.compare_digest

Account Management

Stores user accounts in a local JSON database
Automatic account loading and saving
Supports upgrading old plain-text passwords to secure hashed passwords

Web Search Integration

Connects to the LangSearch Web Search API
Search for any topic directly from the terminal
Select the number of search results (1-10)
Filter results by freshness:
Any Date
Past Day
Past Week
Past Month
Past Year

Result Display

Search result title
Website URL
Publication date (when available)
Content snippet/summary

Error Handling

Invalid login protection
API key validation
Network and timeout handling
HTTP error management
Invalid response handling
🛠 Technologies Used
Python 3
Requests
JSON
hashlib
hmac
secrets
pathlib
LangSearch API
🚀 How It Works
User creates an account or logs in.
Passwords are securely hashed and stored.
After login, the user enters a search query.
The application sends a request to the LangSearch API.
Search results are displayed with detailed information.
🔒 Security Features
PBKDF2-HMAC-SHA256 password hashing
200,000 hash iterations
Unique salt per user
Secure password comparison
Environment variable API key storage
📂 Project Structure
Plain Text
project/
│
├── main.py
├── accounts.json
└── README.md
Environment Variable

Before running the application, set your LangSearch API key:

Shell
export LANGSEARCH_API_KEY="your_api_key_here"

This project is ideal for learning Python authentication systems, password security, API integration, and command-line application development.
