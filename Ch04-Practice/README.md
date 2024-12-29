# Python Todo Application Development Practice Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Project Overview](#project-overview)
3. [Learning Objectives](#learning-objectives)
4. [Project Versions](#project-versions)
5. [Technical Requirements](#technical-requirements)
6. [Development Guidelines](#development-guidelines)
7. [Testing Strategy](#testing-strategy)
8. [Extension Ideas](#extension-ideas)
9. [Review & Assessment](#review-and-assessment)

## Introduction

This practice guide outlines a structured approach to building a TODO application in Python, progressing through three versions of increasing complexity. Each version builds upon the previous one, introducing new concepts and best practices.

## Project Overview

### Problem Statement
Create a TODO application that allows users to manage tasks, demonstrating progression from basic functionality to a well-architected, thoroughly tested application.

### Core Features
- Task management (CRUD operations)
- Data persistence using JSON
- Command-line interface
- Error handling and validation

### Project Structure
```
todo_app/
├── VERSION_1/
│   ├── todo_app.py
│   └── todos.json
├── VERSION_2/
│   ├── models/
│   │   ├── __init__.py
│   │   ├── todo.py
│   │   └── todo_model.py
│   ├── views/
│   │   ├── __init__.py
│   │   └── todo_view.py
│   ├── controllers/
│   │   ├── __init__.py
│   │   └── todo_controller.py
│   └── data/
│       └── todos.json
└── VERSION_3/
    ├── models/
    ├── views/
    ├── controllers/
    ├── tests/
    │   ├── __init__.py
    │   ├── conftest.py
    │   ├── test_model.py
    │   ├── test_view.py
    │   ├── test_controller.py
    │   └── test_integration.py
    └── data/
        └── todos.json
```

## Learning Objectives

### Version 1: Basic Implementation
- Python fundamentals
- File I/O operations
- JSON data handling
- Basic error handling
- User input processing

### Version 2: MVC Architecture
- Software architecture patterns
- Object-oriented programming
- Python type hints
- Dataclasses
- Clean code principles

### Version 3: Testing
- Unit testing with pytest
- Test fixtures and mocking
- Integration testing
- Test coverage analysis
- Test-driven development

## Project Versions

### Version 1: Basic TODO Application

#### Components
```python
class Todo:
    def __init__(self, title: str, description: str, due_date: date):
        self.title = title
        self.description = description
        self.due_date = due_date
        self.is_complete = False
        self.created_at = datetime.now()

class TodoApp:
    def __init__(self, file_path: str):
        self.file_path = file_path
        self.todos = {}
```

#### Required Functions
1. TODO Management
   - add_todo(title, description, due_date)
   - complete_todo(title)
   - delete_todo(title)
   - list_todos()

2. File Operations
   - save_to_file()
   - load_from_file()

#### Success Criteria
- [ ] Basic CRUD operations working
- [ ] Data persists between sessions
- [ ] Basic error handling implemented
- [ ] Input validation working

### Version 2: MVC Implementation

#### Model
```python
@dataclass
class Todo:
    title: str
    description: str
    due_date: date
    is_complete: bool = False
    created_at: datetime = field(default_factory=datetime.now)
    updated_at: datetime = None

    @property
    def is_overdue(self) -> bool:
        return self.due_date < date.today() and not self.is_complete

class TodoModel:
    def __init__(self, file_path: str):
        self.file_path = file_path
        self.todos: Dict[str, Todo] = {}

    @property
    def completed_todos(self) -> List[Todo]:
        return [todo for todo in self.todos.values() if todo.is_complete]
```

#### View
```python
class TodoView:
    @staticmethod
    def show_menu() -> None:
        print("\n=== Todo Manager ===")
        print("1. Add Todo")
        print("2. Complete Todo")
        print("3. Delete Todo")
        print("4. List Todos")
        print("5. Exit")

    @staticmethod
    def get_todo_input() -> TodoData:
        # Implementation
        pass
```

#### Controller
```python
class TodoController:
    def __init__(self, model: TodoModel, view: TodoView):
        self.model = model
        self.view = view

    def run(self) -> None:
        while True:
            self.view.show_menu()
            choice = self.view.get_input("Choose an option: ")
            # Implementation
```

### Version 3: Testing Implementation

#### Test Fixtures
```python
@pytest.fixture
def mock_todos() -> Dict[str, Todo]:
    return {
        "test_todo": Todo(
            title="Test Todo",
            description="Test Description",
            due_date=date.today()
        )
    }

@pytest.fixture
def task_model(tmp_path) -> TodoModel:
    model = TodoModel(tmp_path / "test_todos.json")
    return model
```

#### Model Tests
```python
class TestTodoModel:
    def test_add_todo(self, task_model, mock_todos):
        todo = mock_todos["test_todo"]
        task_model.add_todo(todo)
        assert "test_todo" in task_model.todos

    def test_complete_todo(self, task_model, mock_todos):
        todo = mock_todos["test_todo"]
        task_model.add_todo(todo)
        task_model.complete_todo("test_todo")
        assert task_model.todos["test_todo"].is_complete
```

#### View Tests
```python
class TestTodoView:
    def test_display_todo(self, capsys, mock_todos):
        view = TodoView()
        view.display_todo(mock_todos["test_todo"])
        captured = capsys.readouterr()
        assert "Test Todo" in captured.out
```

#### Integration Tests
```python
class TestIntegration:
    def test_complete_workflow(self, task_model, mock_todos):
        # Test full workflow
        title = "test_todo"
        todo = mock_todos[title]
        
        # Add
        task_model.add_todo(todo)
        assert title in task_model.todos
        
        # Complete
        task_model.complete_todo(title)
        assert task_model.todos[title].is_complete
        
        # Delete
        task_model.delete_todo(title)
        assert title not in task_model.todos
```

## Technical Requirements

### Version 1
1. Data Storage
   - JSON file format
   - Basic error handling
   - Data validation

2. User Interface
   - Command-line menu
   - Basic input validation
   - Simple output formatting

### Version 2
1. Model Requirements
   - Data validation
   - Business logic
   - Error handling
   - File operations

2. View Requirements
   - User input handling
   - Data display formatting
   - Error message display
   - Menu system

3. Controller Requirements
   - Business logic
   - Error handling
   - State management
   - User input processing

### Version 3
1. Testing Requirements
   - Unit tests for all components
   - Integration tests
   - Edge case coverage
   - Mocking framework usage

## Development Guidelines

### Coding Standards
1. Follow PEP 8
2. Use type hints
3. Write docstrings
4. Handle errors appropriately

### Best Practices
1. Single Responsibility Principle
2. DRY (Don't Repeat Yourself)
3. KISS (Keep It Simple, Stupid)
4. Write testable code

### Version Control
1. Commit regularly
2. Write meaningful commit messages
3. Use feature branches
4. Tag versions

## Testing Strategy

### Unit Testing
1. Test individual components
2. Mock dependencies
3. Test edge cases
4. Verify error handling

### Integration Testing
1. Test component interactions
2. Verify data flow
3. Test complete workflows
4. Verify file operations

### Test Coverage
1. Aim for >90% coverage
2. Test all code paths
3. Include edge cases
4. Document untested code

## Extension Ideas

### Feature Extensions
1. Categories for todos
2. Priority levels
3. Due date reminders
4. Recurring todos
5. Task dependencies

### Technical Extensions
1. Database storage
2. REST API
3. Web interface
4. Multi-user support
5. Task sharing

## Review & Assessment

### Version 1 Checklist
- [ ] Basic operations working
- [ ] File operations working
- [ ] Error handling implemented
- [ ] Input validation working

### Version 2 Checklist
- [ ] MVC architecture implemented
- [ ] Advanced Python features used
- [ ] Clean code principles followed
- [ ] Documentation complete

### Version 3 Checklist
- [ ] Test coverage >90%
- [ ] All tests passing
- [ ] Edge cases covered
- [ ] Documentation complete

## Conclusion

This practice project provides a structured approach to learning software development through progressive implementation. Each version builds upon the previous one, introducing new concepts and best practices while maintaining a focus on code quality and testing.

For additional support or questions, feel free to refer to the Python documentation or seek help from the community.

## License and Usage Rights
Copyright © 2024 Maktabkhooneh. All rights reserved.
This educational material is the exclusive property of Maktabkhooneh and instructor Shahin ABDI. All rights to this content are reserved and protected under applicable copyright and intellectual property laws.
Usage Restrictions:

This material is exclusively for enrolled students of the Maktabkhooneh Python course.
Sharing, distributing, or reproducing this content outside of the enrolled student body is strictly prohibited.
No part of this material may be copied, stored, transmitted, or reproduced in any form or by any means without prior written permission from Maktabkhooneh and the instructor.

Instructor: Shahin ABDI

Maktabkhooneh Python Course

Contact: www.maktabkhooneh.org