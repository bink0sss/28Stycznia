# Automatic File Monitoring

### Description:
Write a program that monitors changes in a directory.

### Requirements:
- **Class `DirectoryMonitor`**:
  - Searches for files in the directory.
  - Checks if new files have been added.
  - Logs changes in files.
  - Uses a loop to continuously monitor the directory.

### Example of Features:
- Monitor files in a given directory for new files.
- Log all detected changes, including new files.

### Sample Code:

```python
import os
import time
import logging

class DirectoryMonitor:
    def __init__(self, directory):
        self.directory = directory
        self.files_in_directory = set()
        self.logger = logging.getLogger(__name__)
        self.logger.setLevel(logging.INFO)
        
        handler = logging.FileHandler('directory_monitor.log')
        handler.setLevel(logging.INFO)
        formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
        handler.setFormatter(formatter)
        self.logger.addHandler(handler)

    def get_files(self):
        return set(os.listdir(self.directory))

    def check_for_changes(self):
        current_files = self.get_files()
        new_files = current_files - self.files_in_directory
        if new_files:
            for new_file in new_files:
                self.logger.info(f"New file detected: {new_file}")
                print(f"New file detected: {new_file}")
        self.files_in_directory = current_files

    def monitor(self):
        print(f"Monitoring changes in directory: {self.directory}")
        while True:
            self.check_for_changes()
            time.sleep(1)

# Menu for interacting with the user
def menu():
    print("File Monitoring Program")
    print("1. Start monitoring directory")
    print("2. Exit")
    
    choice = input("Enter your choice: ")
    
    if choice == '1':
        directory = input("Enter the directory path to monitor: ")
        monitor = DirectoryMonitor(directory)
        monitor.monitor()
    elif choice == '2':
        print("Exiting the program.")
        return
    else:
        print("Invalid choice, try again.")
        menu()

if __name__ == "__main__":
    menu()
