import requests
import hashlib

def check_password(password):
    # Hash the password using SHA1
    sha1_password = hashlib.sha1(password.encode('utf-8')).hexdigest().upper()
    prefix = sha1_password[:5]   # First 5 characters
    suffix = sha1_password[5:]   # Remaining characters

    # Send the first 5 characters of the hash to the API
    url = f"https://api.pwnedpasswords.com/range/{prefix}"
    response = requests.get(url)

    # Check if the remaining hash exists in the leaked database
    for line in response.text.splitlines():
        pwned_suffix, count = line.split(':')

        if pwned_suffix == suffix:
            print("❌ Your password has been found in data breaches.")
            return

    print("✅ Your password was NOT found in any known breaches.")

# User input
password = input("Enter your password: ")

# Function call
check_password(password)
