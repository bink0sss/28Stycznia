# Text Analysis Program

### Description:
Create a program that analyzes a text and logs the results into a log file.

### Requirements:
1. **Module `text_analysis`** with functions:
   - `word_count`: Counts the number of words in the provided text.
   - `unique_words`: Returns a list of unique words from the text.

2. **Use classes** to manage input data and log the results.

### Example of Features:
- Count the number of words in a given text.
- Extract and display all unique words.

### Sample Code:

```python
import re
import logging

# Word count function
def word_count(text):
    words = re.findall(r'\b\w+\b', text.lower())
    return len(words)

# Unique words function
def unique_words(text):
    words = re.findall(r'\b\w+\b', text.lower())
    return list(set(words))

# Logger setup and analysis
class TextAnalyzer:
    def __init__(self, text):
        self.text = text
        self.logger = logging.getLogger(__name__)
        self.logger.setLevel(logging.INFO)
        handler = logging.FileHandler('analysis.log')
        handler.setLevel(logging.INFO)
        formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
        handler.setFormatter(formatter)
        self.logger.addHandler(handler)

    def analyze(self):
        word_count_result = word_count(self.text)
        unique_words_result = unique_words(self.text)
        self.logger.info(f"Word count: {word_count_result}")
        self.logger.info(f"Unique words: {', '.join(unique_words_result)}")
        return word_count_result, unique_words_result

# Menu for interacting with the user
def menu():
    print("Text Analysis Program")
    print("1. Analyze Text")
    print("2. Exit")
    
    choice = input("Enter your choice: ")
    
    if choice == '1':
        text = input("Enter the text for analysis: ")
        analyzer = TextAnalyzer(text)
        word_count_result, unique_words_result = analyzer.analyze()
        print(f"Word count: {word_count_result}")
        print(f"Unique words: {', '.join(unique_words_result)}")
    elif choice == '2':
        print("Exiting the program.")
        return
    else:
        print("Invalid choice, try again.")
        menu()

if __name__ == "__main__":
    menu()
