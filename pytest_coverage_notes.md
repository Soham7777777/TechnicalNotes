## **Pytest Basics**
---

- When grouping tests inside a class, each test functions has unique instance of that class.

- Fixture availability is determined from the perspective of the test.

- A fixture can also request any other fixture, no matter where it’s defined, so long as the test requesting them can see all fixtures involved.

- The firxtures are searched from test's perspective, for example:
```python3
import pytest


@pytest.fixture
def order():
    return []


@pytest.fixture
def outer(order, inner):
    order.append("outer")


class TestOne:
    @pytest.fixture
    def inner(self, order):
        order.append("one")

    def test_order(self, order, outer):
        assert order == ["one", "outer"]


class TestTwo:
    @pytest.fixture
    def inner(self, order):
        order.append("two")

    def test_order(self, order, outer):
        assert order == ["two", "outer"]
```

- Here there are two inner fixtures inside both classes, which one will the outer use will be determined by the eye of test function.

- In first class the test function is seeing the inner inside its class so outer will use that inner.
