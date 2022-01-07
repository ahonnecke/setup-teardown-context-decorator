# Setup Teardown Content Decorator

This is a simple package that aims to make adding setup and teardown to pytest flavored tests quick and painless.

# Installation
```
python3 -m pip install setup-teardown
```

# Decorator Usage

## Custom Database Class
```
from setup_teardown import SetupTeardown

class PgSetupTeardown(SetupTeardown):
    def __init__(self, table, **kwargs):
        self.table = table
        self.__dict__.update(kwargs)

    def __enter__(self):
        # Perform test setup
        self.session = db.session.new_session()
        self.session.query(self.table).delete()
        return self

    def __exit__(self, typ, val, traceback):
        # Perform test teardown
        self.session.query(self.table).delete()
```

## Example test with decorator usage
```
class TestHandlerDatabaseRequired:
    @SetupTeardown(table="table_name")
    def test_handler_success(self, mock_datetime):
        """
        Example of using the SetupTeardown ContextDecorator with arbitrary setup and teardown
        """
        assert 1 == 1
        # Effect the database changes
```

## Example test with context manager usage

```
    def setup(self):
        db.session.new_session().query(self.table).delete()

    def teardown(self):
        db.session.new_session().query(self.table).delete()


    # contest manager application example
    with SetupTeardown(setup=setup, teardown=teardown):
        assert 1 == 1
        # Effect the database changes
```
