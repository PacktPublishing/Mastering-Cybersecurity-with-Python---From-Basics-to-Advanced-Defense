import re # import for regular expressions
import hashlib # import for hashing

from datetime import datetime # import for date and time

# Function to hash passwords

def hash_password(password):
  return hashlib.sha256(password.encode()).hexdigest()

# Password Checker Function

def is_strong_password(password):
  if len(password) < 8:
    return False, "Password must be at least 8 characters long."

  if not re.search(r'[A-Z]', password):
    return False, "Password must include at least one uppercase letter."

  if not re.search(r'[0-9]', password):
    return False, "Password must include at least one number."

  if not re.search(r'[!@#$%^&*(),.?":{}|<>]', password):
    return False, "Password must include at least one special symbol"

  return True, "Password is strong"


# Function to log user activities

def log_event(event):
  timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
  with open("audit_log.txt", "a") as log_file:
    log_file.write(f"[{timestamp}] {event}\n")


# Registration Function

def register_user():
  username = input("Enter a Username:")
  password = input("Enter a password:")

  is_valid, feedback = is_strong_password(password)
  if not is_valid:
    print("Password is not strong enough")
    print(feedback)
    log_event(f"Registration failed for user {username}")
    return

  # Hash the password

  hashed_password = hash_password(password)

  with open("users.txt", "a") as file:
    file.write(f"{username}:{hashed_password}\n")
  print("User Registration was Successful")
  log_event(f"{username} successfully registered")

# Login Function

def login_user():
  username = input("Enter your Username:")
  password = input("Enter your Password:")

  with open("users.txt", "r") as file:
    users = file.readlines()

  for user in users:
    stored_username, stored_password = user.strip().split(":")
    if username == stored_username and hash_password(password) == stored_password:
      print("Login Successful")
      log_event(f"{username} successfully logged in")
      post_login_menu(username)
      return
      
  print("Invalid username or password")
  log_event(f"Failed login attempt for user {username}")


# Function to display Post Login Menu

def post_login_menu(username):
  while True:
    print("\nPost-Login Menu")
    print("1. View my Logs")
    print("2. Logout")
    choice = input("What would you like to do?")

    if choice == "1":
      view_logs(username)
    elif choice == "2":
      log_event(f"User '{username}' logged out")
      print("Logged out successfully")
      break
    else:
      print("Invalid choice. Please try again")

# Function to view logs

def view_logs(username):
  print(f"\nLogs for user '{username}'")
  with open("audit_log.txt", "r") as log_file:
    logs = log_file.readlines()

  user_logs = [log.strip() for log in logs if username in log]

  if user_logs:
    for log in user_logs:
      print(log)

  else:
    print("No logs were found in your account")


def main():
  while True:
    print("Welcome to the User Registration System")
    print("1. Register")
    print("2. Login")
    print("3. Exit")
    choice = input("What would you like to do?")

    if choice == "1":
      register_user()

    elif choice == "2":
      login_user()

    elif choice == "3":
      log_event("System exit")
      print("Exiting the system")
      break
    else:
      print("Invalid choice. Please try again.")

main()