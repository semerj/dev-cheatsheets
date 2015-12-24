# SQLAlchemy

### connect to db
```python
from sqlalchemy import create_engine
engine = create_engine('mysql://user@localhost:3306/test')

# engine = create_engine('mysql://user:pw@localhost:3306/mydb', echo=False)
```

### call session
```python
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine)
session = Session()
```

### define models
```python
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String

Base = declarative_base()
class MyTable(Base):
     __tablename__ = 'mytable'

     id = Column(Integer, primary_key=True)
     name = Column(String(100))
     value = Column(String(100))

     def __init__(self, name, value):
             self.name = name
             self.value = value

     def __repr__(self):
             return "<MyTable(%s, %s)>" % (self.name, self.value)
```

### operations

* add new record:

    ```python
    new_record = MyTable('Genius', 'me')
    session.add(new_record)
    session.commit()
    ```

* add multiple records at once:

    ```python
    list_or_records = [MyTable('Genius', 'me'), MyTable('Super', 'me')]
    session.add_all(list_of_records)
    session.commit()
    ```

* query table:

    ```python
    records = session.query(MyTable).filter_by(name='Genius')
    # or
    all_records = session.query(MyTable).all()
    ```

* delete records:

    ```python
    records_to_delete = session.query(MyTable).filter_by(name='Super')
    for record in records_to_delete:
            session.delete(record)
    session.commit()
    ```
