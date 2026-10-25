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

the pyhon code is:-

import json
import os
import hashlib
import hmac
import secrets
from pathlib import Path

import requests

# ---------- Configuration ----------
ACCOUNTS_FILE = Path("accounts.json")
API_URL = "https://api.langsearch.com/v1/web-search"
API_KEY = os.getenv("LANGSEARCH_API_KEY")


# ---------- Account System ----------
def load_accounts():
    if not ACCOUNTS_FILE.exists():
        return {}

    try:
        with ACCOUNTS_FILE.open("r", encoding="utf-8") as file:
            accounts = json.load(file)
            return accounts if isinstance(accounts, dict) else {}
    except (json.JSONDecodeError, OSError):
        print("Could not read accounts.json.")
        return {}


def save_accounts(accounts):
    with ACCOUNTS_FILE.open("w", encoding="utf-8") as file:
        json.dump(accounts, file, indent=2)


def hash_password(password, salt=None):
    if salt is None:
        salt = secrets.token_hex(16)

    hashed = hashlib.pbkdf2_hmac(
        "sha256",
        password.encode("utf-8"),
        bytes.fromhex(salt),
        200_000
    ).hex()

    return {"salt": salt, "hash": hashed}


def verify_password(password, stored):
    # Support accounts created by this version.
    if isinstance(stored, dict):
        candidate = hash_password(password, stored["salt"])["hash"]
        return hmac.compare_digest(candidate, stored["hash"])

    # Compatibility with your original plain-text accounts.json.
    # Successful login upgrades the password to a hash.
    return isinstance(stored, str) and hmac.compare_digest(
        stored, password
    )


# ---------- LangSearch ----------
def search_web(topic):
    if not API_KEY:
        print(
            "\nAPI key missing. Set the LANGSEARCH_API_KEY "
            "environment variable first."
        )
        return

    count_text = input("Number of results (1-10, default 5): ").strip()
    try:
        count = int(count_text) if count_text else 5
        if not 1 <= count <= 10:
            raise ValueError
    except ValueError:
        print("Please enter a number between 1 and 10.")
        return

    print("\nFreshness options:")
    print("1. Any date")
    print("2. Past day")
    print("3. Past week")
    print("4. Past month")
    print("5. Past year")

    freshness_options = {
        "1": "noLimit",
        "2": "oneDay",
        "3": "oneWeek",
        "4": "oneMonth",
        "5": "oneYear",
    }

    choice = input("Choose freshness (default 1): ").strip() or "1"
    if choice not in freshness_options:
        print("Invalid choice; using any date.")
        choice = "1"

    payload = {
        "query": topic,
        "count": count,
        "freshness": freshness_options[choice],
        "contents": {
            "text": {
                "max_characters": 2000
            }
        }
    }

    headers = {
        "Authorization": f"Bearer {API_KEY}",
        "Content-Type": "application/json"
    }

    try:
        response = requests.post(
            API_URL,"
import json
import os
import hashlib
import hmac
import secrets
from pathlib import Path

import requests

# ---------- Configuration ----------
ACCOUNTS_FILE = Path("accounts.json")
API_URL = "https://api.langsearch.com/v1/web-search"
API_KEY = os.getenv("LANGSEARCH_API_KEY")


# ---------- Account System ----------
def load_accounts():
    if not ACCOUNTS_FILE.exists():
        return {}

    try:
        with ACCOUNTS_FILE.open("r", encoding="utf-8") as file:
            accounts = json.load(file)
            return accounts if isinstance(accounts, dict) else {}
    except (json.JSONDecodeError, OSError):
        print("Could not read accounts.json.")
        return {}


def save_accounts(accounts):
    with ACCOUNTS_FILE.open("w", encoding="utf-8") as file:
        json.dump(accounts, file, indent=2)


def hash_password(password, salt=None):
    if salt is None:
        salt = secrets.token_hex(16)

    hashed = hashlib.pbkdf2_hmac(
        "sha256",
        password.encode("utf-8"),
        bytes.fromhex(salt),
        200_000
    ).hex()

    return {"salt": salt, "hash": hashed}


def verify_password(password, stored):
    # Support accounts created by this version.
    if isinstance(stored, dict):
        candidate = hash_password(password, stored["salt"])["hash"]
        return hmac.compare_digest(candidate, stored["hash"])

    # Compatibility with your original plain-text accounts.json.
    # Successful login upgrades the password to a hash.
    return isinstance(stored, str) and hmac.compare_digest(
        stored, password
    )


# ---------- LangSearch ----------
def search_web(topic):
    if not API_KEY:
        print(
            "\nAPI key missing. Set the LANGSEARCH_API_KEY "
            "environment variable first."
        )
        return

    count_text = input("Number of results (1-10, default 5): ").strip()
    try:
        count = int(count_text) if count_text else 5
        if not 1 <= count <= 10:
            raise ValueError
    except ValueError:
        print("Please enter a number between 1 and 10.")
        return

    print("\nFreshness options:")
    print("1. Any date")
    print("2. Past day")
    print("3. Past week")
    print("4. Past month")
    print("5. Past year")

    freshness_options = {
        "1": "noLimit",
        "2": "oneDay",
        "3": "oneWeek",
        "4": "oneMonth",
        "5": "oneYear",
    }

    choice = input("Choose freshness (default 1): ").strip() or "1"
    if choice not in freshness_options:
        print("Invalid choice; using any date.")
        choice = "1"

    payload = {
        "query": topic,
        "count": count,
        "freshness": freshness_options[choice],
        "contents": {
            "text": {
                "max_characters": 2000
            }
        }
    }

    headers = {
        "Authorization": f"Bearer {API_KEY}",
        "Content-Type": "application/json"
    }

    try:
        response = requests.post(
            API_URL,"
import json
import os
import hashlib
import hmac
import secrets
from pathlib import Path

import requests

# ---------- Configuration ----------
ACCOUNTS_FILE = Path("accounts.json")
API_URL = "https://api.langsearch.com/v1/web-search"
API_KEY = os.getenv("LANGSEARCH_API_KEY")


# ---------- Account System ----------
def load_accounts():
    if not ACCOUNTS_FILE.exists():
        return {}

    try:
        with ACCOUNTS_FILE.open("r", encoding="utf-8") as file:
            accounts = json.load(file)
            return accounts if isinstance(accounts, dict) else {}
    except (json.JSONDecodeError, OSError):
        print("Could not read accounts.json.")
        return {}


def save_accounts(accounts):
    with ACCOUNTS_FILE.open("w", encoding="utf-8") as file:
        json.dump(accounts, file, indent=2)


def hash_password(password, salt=None):
    if salt is None:
        salt = secrets.token_hex(16)

    hashed = hashlib.pbkdf2_hmac(
        "sha256",
        password.encode("utf-8"),
        bytes.fromhex(salt),
        200_000
    ).hex()

    return {"salt": salt, "hash": hashed}


def verify_password(password, stored):
    # Support accounts created by this version.
    if isinstance(stored, dict):
        candidate = hash_password(password, stored["salt"])["hash"]
        return hmac.compare_digest(candidate, stored["hash"])

    # Compatibility with your original plain-text accounts.json.
    # Successful login upgrades the password to a hash.
    return isinstance(stored, str) and hmac.compare_digest(
        stored, password
    )


# ---------- LangSearch ----------
def search_web(topic):
    if not API_KEY:
        print(
            "\nAPI key missing. Set the LANGSEARCH_API_KEY "
            "environment variable first."
        )
        return

    count_text = input("Number of results (1-10, default 5): ").strip()
    try:
        count = int(count_text) if count_text else 5
        if not 1 <= count <= 10:
            raise ValueError
    except ValueError:
        print("Please enter a number between 1 and 10.")
        return

    print("\nFreshness options:")
    print("1. Any date")
    print("2. Past day")
    print("3. Past week")
    print("4. Past month")
    print("5. Past year")

    freshness_options = {
        "1": "noLimit",
        "2": "oneDay",
        "3": "oneWeek",
        "4": "oneMonth",
        "5": "oneYear",
    }

    choice = input("Choose freshness (default 1): ").strip() or "1"
    if choice not in freshness_options:
        print("Invalid choice; using any date.")
        choice = "1"

    payload = {
        "query": topic,
        "count": count,
        "freshness": freshness_options[choice],
        "contents": {
            "text": {
                "max_characters": 2000
            }
        }
    }

    headers = {
        "Authorization": f"Bearer {API_KEY}",
        "Content-Type": "application/json"
    }

    try:
        response = requests.post(
            API_URL,
            headers=headers,
            json=payload,
            timeout=30
        )
        response.raise_for_status()
        data = response.json()

        if str(data.get("code")) != "200":
            print("Search failed:", data.get("message", "Unknown error"))
            return

        pages = data.get("data", {}).get(
            "webPages", {}
        ).get("value", [])

        if not pages:
            print("No search results found.")
            return

        print(f"\nSearch results for: {topic}\n")

        for index, page in enumerate(pages, start=1):
            title = page.get("name", "Untitled")
            url = page.get("url", "")
            content = page.get("text") or page.get(
                "snippet", "No snippet available."
            )
            date = page.get("datePublished", "")

            print(f"{index}. {title}")
            print(f"   URL: {url}")
            if date:
                print(f"   Published: {date}")
            print(f"   Details: {content}\n")
            print("-" * 70)

    except requests.exceptions.Timeout:
        print("The search request timed out. Please try again.")
    except requests.exceptions.HTTPError as error:
        print("HTTP error:", error)
        if response.status_code in (401, 403):
            print("Check your LangSearch API key and permissions.")
        elif response.status_code == 429:
            print("Rate or usage limit reached. Try again later.")
    except requests.exceptions.RequestException as error:
        print("Network error:", error)
    except (ValueError, KeyError, TypeError) as error:
        print("Could not process the search response:", error)


# ---------- Main Program ----------
def main():
    accounts = load_accounts()

    print("Welcome!")
    action = input(
        "Type 'yes' to log in or 'create' to create an account: "
    ).strip().lower()

    if action == "create":
        user_name = input("Type user name: ").strip()

        if not user_name:
            print("User name cannot be empty.")
            return

        if user_name in accounts:
            print("That username already exists.")
            return

        password = input("Type your password: ")
        if len(password) < 8:
            print("Please use a password with at least 8 characters.")
            return

        accounts[user_name] = hash_password(password)
        save_accounts(accounts)
        print("Your account was created successfully.")

    elif action == "yes":
        user_name = input("Type your user name: ").strip()
        password = input("Type your password: ")

        stored = accounts.get(user_name)

        if stored is None or not verify_password(password, stored):
            print("Your username or password is incorrect.")
            return

        # Upgrade an old plain-text password after successful login.
        if isinstance(stored, str):
            accounts[user_name] = hash_password(password)
            save_accounts(accounts)

        print("Login successful!")

        if not API_KEY:
            print(
                "Set your LangSearch API key before searching."
            )
            return

        topic = input("What do you want to search? ").strip()
        if topic:
            search_web(topic)
        else:
            print("Search topic cannot be empty.")

    else:
        print("Please type 'yes' or 'create'.")


if __name__ == "__main__":
    main()
            headers=headers,
            json=payload,
            timeout=30
        )
        response.raise_for_status()
        data = response.json()

        if str(data.get("code")) != "200":
            print("Search failed:", data.get("message", "Unknown error"))
            return

        pages = data.get("data", {}).get(
            "webPages", {}
        ).get("value", [])

        if not pages:
            print("No search results found.")
            return

        print(f"\nSearch results for: {topic}\n")

        for index, page in enumerate(pages, start=1):
            title = page.get("name", "Untitled")
            url = page.get("url", "")
            content = page.get("text") or page.get(
                "snippet", "No snippet available."
            )
            date = page.get("datePublished", "")

            print(f"{index}. {title}")
            print(f"   URL: {url}")
            if date:
                print(f"   Published: {date}")
            print(f"   Details: {content}\n")
            print("-" * 70)

    except requests.exceptions.Timeout:
        print("The search request timed out. Please try again.")
    except requests.exceptions.HTTPError as error:
        print("HTTP error:", error)
        if response.status_code in (401, 403):
            print("Check your LangSearch API key and permissions.")
        elif response.status_code == 429:
            print("Rate or usage limit reached. Try again later.")
    except requests.exceptions.RequestException as error:
        print("Network error:", error)
    except (ValueError, KeyError, TypeError) as error:
        print("Could not process the search response:", error)


# ---------- Main Program ----------
def main():
    accounts = load_accounts()

    print("Welcome!")
    action = input(
        "Type 'yes' to log in or 'create' to create an account: "
    ).strip().lower()

    if action == "create":
        user_name = input("Type user name: ").strip()

        if not user_name:
            print("User name cannot be empty.")
            return

        if user_name in accounts:
            print("That username already exists.")
            return

        password = input("Type your password: ")
        if len(password) < 8:
            print("Please use a password with at least 8 characters.")
            return

        accounts[user_name] = hash_password(password)
        save_accounts(accounts)
        print("Your account was created successfully.")

    elif action == "yes":
        user_name = input("Type your user name: ").strip()
        password = input("Type your password: ")

        stored = accounts.get(user_name)

        if stored is None or not verify_password(password, stored):
            print("Your username or password is incorrect.")
            return

        # Upgrade an old plain-text password after successful login.
        if isinstance(stored, str):
            accounts[user_name] = hash_password(password)
            save_accounts(accounts)

        print("Login successful!")

        if not API_KEY:
            print(
                "Set your LangSearch API key before searching."
            )
            return

        topic = input("What do you want to search? ").strip()
        if topic:
            search_web(topic)
        else:
            print("Search topic cannot be empty.")

    else:
        print("Please type 'yes' or 'create'.")


if __name__ == "__main__":
    main()
            headers=headers,
            json=payload,
            timeout=30
        )
        response.raise_for_status()
        data = response.json()

        if str(data.get("code")) != "200":
            print("Search failed:", data.get("message", "Unknown error"))
            return

        pages = data.get("data", {}).get(
            "webPages", {}
        ).get("value", [])

        if not pages:
            print("No search results found.")
            return

        print(f"\nSearch results for: {topic}\n")

        for index, page in enumerate(pages, start=1):
            title = page.get("name", "Untitled")
            url = page.get("url", "")
            content = page.get("text") or page.get(
                "snippet", "No snippet available."
            )
            date = page.get("datePublished", "")

            print(f"{index}. {title}")
            print(f"   URL: {url}")
            if date:
                print(f"   Published: {date}")
            print(f"   Details: {content}\n")
            print("-" * 70)

    except requests.exceptions.Timeout:
        print("The search request timed out. Please try again.")
    except requests.exceptions.HTTPError as error:
        print("HTTP error:", error)
        if response.status_code in (401, 403):
            print("Check your LangSearch API key and permissions.")
        elif response.status_code == 429:
            print("Rate or usage limit reached. Try again later.")
    except requests.exceptions.RequestException as error:
        print("Network error:", error)
    except (ValueError, KeyError, TypeError) as error:
        print("Could not process the search response:", error)


# ---------- Main Program ----------
def main():
    accounts = load_accounts()

    print("Welcome!")
    action = input(
        "Type 'yes' to log in or 'create' to create an account: "
    ).strip().lower()

    if action == "create":
        user_name = input("Type user name: ").strip()

        if not user_name:
            print("User name cannot be empty.")
            return

        if user_name in accounts:
            print("That username already exists.")
            return

        password = input("Type your password: ")
        if len(password) < 8:
            print("Please use a password with at least 8 characters.")
            return

        accounts[user_name] = hash_password(password)
        save_accounts(accounts)
        print("Your account was created successfully.")

    elif action == "yes":
        user_name = input("Type your user name: ").strip()
        password = input("Type your password: ")

        stored = accounts.get(user_name)

        if stored is None or not verify_password(password, stored):
            print("Your username or password is incorrect.")
            return

        # Upgrade an old plain-text password after successful login.
        if isinstance(stored, str):
            accounts[user_name] = hash_password(password)
            save_accounts(accounts)

        print("Login successful!")

        if not API_KEY:
            print(
                "Set your LangSearch API key before searching."
            )
            return

        topic = input("What do you want to search? ").strip()
        if topic:
            search_web(topic)
        else:
            print("Search topic cannot be empty.")

    else:
        print("Please type 'yes' or 'create'.")


if __name__ == "__main__":
    main()




