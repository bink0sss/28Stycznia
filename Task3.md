# To-Do Application for Task Management

### Description:
Write a To-Do application that allows the user to manage a list of tasks.

### Requirements:
- **The application should allow**:
  - Adding tasks with a title and priority (high, medium, low).
  - Displaying all tasks sorted by priority (high -> medium -> low).
  - Marking tasks as completed.
  - Displaying only completed or incomplete tasks.
  - Logging all user actions to a text file (`todo.log`).

### Example of Features:
- Allow the user to add tasks and set priorities.
- Mark tasks as completed and log all changes.

### Sample Code:

```python
import logging

class Task:
    def __init__(self, title, priority):
        self.title = title
        self.priority = priority
        self.completed = False
    
    def mark_completed(self):
        self.completed = True

    def __str__(self):
        status = "Completed" if self.completed else "Not Completed"
        return f"{self.title} - Priority: {self.priority} - Status: {status}"

class ToDoApp:
    def __init__(self):
        self.tasks = []
        self.logger = logging.getLogger(__name__)
        self.logger.setLevel(logging.INFO)
        
        handler = logging.FileHandler('todo.log')
        handler.setLevel(logging.INFO)
        formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
        handler.setFormatter(formatter)
        self.logger.addHandler(handler)

    def add_task(self, title, priority):
        if priority not in ["high", "medium", "low"]:
            print("Invalid priority.")
            return
        task = Task(title, priority)
        self.tasks.append(task)
        self.logger.info(f"Added task: {task.title} with priority {task.priority}")
    
    def display_tasks(self):
        for task in sorted(self.tasks, key=lambda x: x.priority):
            print(task)

    def mark_completed(self, title):
        for task in self.tasks:
            if task.title == title:
                task.mark_completed()
                self.logger.info(f"Task completed: {title}")
                return
        print("Task not found.")

    def display_completed(self):
        for task in self.tasks:
            if task.completed:
                print(task)

    def display_incomplete(self):
        for task in self.tasks:
            if not task.completed:
                print(task)

# Menu for interacting with the user
def menu():
    print("To-Do Application")
    print("1. Add Task")
    print("2. Display All Tasks")
    print("3. Mark Task as Completed")
    print("4. Display Completed Tasks")
    print("5. Display Incomplete Tasks")
    print("6. Exit")
    
    choice = input("Enter your choice: ")
    
    todo_app = ToDoApp()
    
    if choice == '1':
        title = input("Enter task title: ")
        priority = input("Enter task priority (high, medium, low): ")
        todo_app.add_task(title, priority)
    elif choice == '2':
        todo_app.display_tasks()
    elif choice == '3':
        title = input("Enter the task title to mark as completed: ")
        todo_app.mark_completed(title)
    elif choice == '4':
        todo_app.display_completed()
    elif choice == '5':
        todo_app.display_incomplete()
    elif choice == '6':
        print("Exiting the program.")
        return
    else:
        print("Invalid choice, try again.")
        menu()

if __name__ == "__main__":
    menu()
