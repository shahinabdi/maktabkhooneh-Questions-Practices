# Python Advanced Concepts - Research Questions

Prepared for Maktabkhooneh Students

Instructor: Shahin ABDI

## Introduction
These research questions are designed to deepen your understanding of advanced Python concepts. Each question includes generic examples to illustrate the concepts clearly.

## Research Questions

### 1. Dataclasses Deep Dive
**Primary Question:**
> Research and explain when and why you would use dataclasses in Python. Focus on the `frozen` and `slots` parameters and their implications.

**Research Points:**
- Memory efficiency comparison
- Immutability benefits
- Inheritance behavior
- Performance considerations

**Example Scenario:**
```python
# Instead of:
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

# Consider:
@dataclass(frozen=True, slots=True)
class Point:
    x: float
    y: float
```

**Discussion Topics:**
1. When does memory optimization matter?
2. How do frozen classes affect code design?
3. What are the trade-offs of using slots?

### 2. Static Methods vs Class Methods
**Primary Question:**
> Explain the differences between @staticmethod and @classmethod decorators. When would you use each?

**Research Points:**
- Method access patterns
- Inheritance behavior
- Use cases for each
- Design implications

**Example Scenario:**
```python
class DateProcessor:
    date_format = "%Y-%m-%d"

    @staticmethod
    def is_weekend(date):
        """No class state needed"""
        pass

    @classmethod
    def from_string(cls, date_str):
        """Uses class state"""
        pass
```

### 3. Pytest Fixtures Scope
**Primary Question:**
> Research pytest fixture scopes and their impact on testing. When would you use each scope?

**Research Points:**
- Available scopes
- Resource management
- Performance impact
- State handling

**Example Scenario:**
```python
# Database connection example
@pytest.fixture(scope="session")
def database():
    # Expensive connection
    pass

@pytest.fixture(scope="function")
def temp_file():
    # New file for each test
    pass
```

### 4. Property Decorators
**Primary Question:**
> Explain the three types of property decorators (@property, @x.setter, @x.deleter) and their use cases.

**Research Points:**
- Encapsulation patterns
- Validation strategies
- Computed properties
- Property inheritance

**Example Scenario:**
```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def area(self):
        # Computed property
        pass

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value
```

### 5. JSON Serialization
**Primary Question:**
> How would you handle custom object serialization in Python? Explain different approaches.

**Research Points:**
- Custom encoders/decoders
- Complex object handling
- Circular references
- Performance considerations

**Example Scenario:**
```python
class CustomEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return {"_type": "datetime", "value": obj.isoformat()}
        return super().default(obj)
```

### 6. Type Hints and Generics
**Primary Question:**
> Explain the use of Generic types and TypeVar in Python. What problems do they solve?

**Research Points:**
- Generic type patterns
- Type constraints
- Collection types
- Bounded types

**Example Scenario:**
```python
T = TypeVar('T', bound=Number)

class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: List[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> Optional[T]:
        return self._items.pop() if self._items else None
```

### 7. Context Managers
**Primary Question:**
> How do context managers work in Python? Explain both class-based and decorator-based implementations.

**Research Points:**
- Resource management
- Error handling
- Context manager protocol
- Nested contexts

**Example Scenario:**
```python
class Timer:
    def __enter__(self):
        self.start = time.time()
        return self

    def __exit__(self, *args):
        self.end = time.time()
        self.duration = self.end - self.start

@contextmanager
def temp_directory():
    path = tempfile.mkdtemp()
    try:
        yield path
    finally:
        shutil.rmtree(path)
```

### 8. Pytest Parameterize
**Primary Question:**
> How can you use pytest's parameterize feature effectively? What are its limitations?

**Research Points:**
- Parameter combinations
- Test generation
- Data handling
- Fixture interaction

**Example Scenario:**
```python
@pytest.mark.parametrize("input,expected", [
    (2, 4),
    (3, 9),
    (4, 16),
    pytest.param(-1, 1, marks=pytest.mark.xfail),
])
def test_square(input, expected):
    assert square(input) == expected
```

### 9. Python Descriptors
**Primary Question:**
> What are Python descriptors and when should you use them?

**Research Points:**
- Descriptor protocol
- Implementation patterns
- Use cases
- Performance impact

**Example Scenario:**
```python
class ValidatedNumber:
    def __init__(self, min_value=None, max_value=None):
        self.min_value = min_value
        self.max_value = max_value

    def __set_name__(self, owner, name):
        self.name = f"_{name}"

    def __get__(self, instance, owner):
        if instance is None:
            return self
        return getattr(instance, self.name, None)

    def __set__(self, instance, value):
        if self.min_value is not None and value < self.min_value:
            raise ValueError(f"Value must be >= {self.min_value}")
        setattr(instance, self.name, value)
```

### 10. Mock Objects Deep Dive
**Primary Question:**
> Compare different types of mocks in Python. When would you use each type?

**Research Points:**
- Mock vs MagicMock
- Side effects
- Call assertions
- Mock patterns

**Example Scenario:**
```python
def test_api_call():
    mock_response = Mock()
    mock_response.json.return_value = {"status": "success"}
    mock_response.status_code = 200

    with patch('requests.get', return_value=mock_response):
        result = get_api_data()
        assert result["status"] == "success"
```

## Bonus Challenge
Create a generic class that demonstrates at least five of these concepts working together:
- Type safety
- Resource management
- Validation
- Serialization
- Testing

## Evaluation Criteria
For each research question:
1. Concept understanding
2. Implementation quality
3. Best practices knowledge
4. Trade-off analysis
5. Real-world applicability

## Additional Research Topics
1. Async context managers
2. Type hint extensions (typing_extensions)
3. Descriptor applications
4. Advanced pytest plugins
5. Memory optimization techniques

## Resources
1. Python Documentation
   - dataclasses
   - typing
   - contextlib
2. Pytest Documentation
   - fixtures
   - parametrize
3. Python Enhancement Proposals (PEPs)
   - PEP 484 (Type Hints)
   - PEP 557 (Data Classes)
4. Real Python Articles
5. Python Design Patterns Guide

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