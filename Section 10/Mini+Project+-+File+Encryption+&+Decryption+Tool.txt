#  

from cryptography.fernet import Fernet

def generate_key(file_path = 'encryption_key.key'):
    key = Fernet.generate_key()
    with open(file_path, 'wb') as key_file:
      key_file.write(key)
    print(f"Encryption key saved to {file_path}")

def load_key(file_path = 'encryption_key.key'):
  with open(file_path, 'rb') as key_file:
    key= key_file.read()
  return key

def encrypt_file(input_file,output_file,key):
  fernet = Fernet(key)
  with open(input_file, 'rb') as file:
    original = file.read()
  encrypted = fernet.encrypt(original)
  with open(output_file, 'wb') as file:
    file.write(encrypted)
  print(f"File encrypted and saved to {output_file}")


def decrypt_file(input_file,output_file,key):
  fernet = Fernet(key)
  with open(input_file, 'rb') as file:
    encrypted = file.read()
  decrypted = fernet.decrypt(encrypted)
  with open(output_file, 'wb') as file:
    file.write(decrypted)
  print(f"File {input_file} decrypted and saved to {output_file}")

def main():
  print("Welcome to the Encryption/Decryption Tool")
  while True:
    print("1. Generate an encryption key")
    print("2. Encrypt a file")
    print("3. Decrypt a file")
    print("4. Exit the program")

    option = input("What option would you like to go with?")

    if option == '1':
      file_path = input("Enter the file path to save the key (default: 'encryption_key.key'): ") or 'encryption_key.key'
      generate_key(file_path)

    elif option == '2':
      input_file = input("Enter the path of the file to encrypt:")
      output_file = input("Enter the name of the output file:")
      key_path = input("Enter the path of the encyption key otherwise the default will be used") or 'encryption_key.key'
      try:
        key = load_key(key_path)
        encrypt_file(input_file, output_file,key)
      except Exception as e:
        print(f"Error during encryption: {e}")

    elif option == '3':
      input_file = input("Enter the path of the file to decrypt:")
      output_file = input("Enter the name of the output file:")
      key_path = input("Enter the path of the encyption key otherwise the default will be used") or 'encryption_key.key'
      try:
        key = load_key(key_path)
        decrypt_file(input_file, output_file,key)
      except Exception as e:
        print(f"Error during decryption: {e}")

    elif option == '4':
      print("Exiting the program")
      break

    else:
      print("Please choose a valid option")

main()


