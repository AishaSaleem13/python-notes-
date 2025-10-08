pydantic notes 

🧠 PYDANTIC — COMPLETE NOTES (For Developers & Non-Coders)
🌍 1. What Is Pydantic?

Pydantic is a Python library that helps you make sure your data is correct, clean, and safe before your program uses it.

In simple words:

“It checks your data for mistakes — automatically.”

It’s the data guard of your app 💪

⚙️ 2. Why We Need It

Whenever you receive data:

From frontend forms

From APIs

From a database

From users typing random stuff

That data might be:

Incomplete ❌

Wrong type ❌

Missing values ❌

Mismatched (like a string instead of number) ❌

Instead of manually checking everything, Pydantic does it for you — automatically validates and converts.

🧩 3. How It Works — in Simple Words

You create a Model (class) that describes what kind of data you expect.

Example 👇

from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
    email: str


Now if someone sends:

data = {"name": "Aisha", "age": "22", "email": "aisha@gmail.com"}
user = User(**data)


✅ It automatically converts "22" (string) into integer 22.
If any field is missing or invalid, it gives an error instantly — before your app breaks.

🧾 4. Output Example (Validation)

If a user sends bad data:

data = {"name": "Aisha", "age": "twenty"}
user = User(**data)


Pydantic says:

pydantic.error_wrappers.ValidationError:
age
  value is not a valid integer


Boom 💥 It caught the error before your code used it.

🧮 5. Type Conversion (Magic!)

Pydantic automatically converts types when possible.

Input	Expected	Output
"22"	int	22
"true"	bool	True
"2025-10-08"	datetime	datetime(2025,10,8)

That’s why FastAPI uses it — it saves hundreds of manual conversions.

🧱 6. Core Concept: BaseModel

Everything in Pydantic starts from this:

from pydantic import BaseModel


You inherit from BaseModel to create your own models (like blueprints).

🧩 7. Example with Default Values & Optional Fields
from typing import Optional
from pydantic import BaseModel

class Product(BaseModel):
    name: str
    price: float
    in_stock: bool = True
    description: Optional[str] = None


Now:

in_stock has a default value True

description is optional

If user skips those, Pydantic still works fine.

🔒 8. Data Validation Example
item = Product(name="Laptop", price="1000")
print(item)


✅ Converts "1000" → 1000.0
✅ Fills in_stock=True automatically
✅ Adds description=None

Output:

name='Laptop' price=1000.0 in_stock=True description=None

⚡ 9. Error Handling Example

If wrong data type:

Product(name=123, price="abc")


❌ Error:

name: str expected
price: value is not a valid float

🧩 10. .dict() and .json() Methods

Convert your validated data into Python dict or JSON.

user = User(name="Aisha", age=22, email="aisha@gmail.com")
print(user.dict())   # {'name': 'Aisha', 'age': 22, 'email': 'aisha@gmail.com'}
print(user.json())   # JSON string


Perfect for saving to databases or sending in API responses.

🧭 11. Nested Models

You can nest one model inside another.

class Address(BaseModel):
    city: str
    country: str

class User(BaseModel):
    name: str
    address: Address


Input:

data = {"name": "Aisha", "address": {"city": "Karachi", "country": "Pakistan"}}
user = User(**data)
print(user.address.city)


✅ Works beautifully!

🧰 12. Pydantic in FastAPI

FastAPI uses Pydantic for everything:

Request validation (input)

Response models (output)

Environment settings (via pydantic-settings)

Example route:

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

@app.post("/items/")
def create_item(item: Item):
    return {"item": item}


When user sends wrong data — FastAPI instantly responds with a 400 error + validation message — all thanks to Pydantic.

⚙️ 13. Pydantic Settings (Configuration Management)

You can use it to safely load .env files:

from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    DB_URL: str
    API_KEY: str

    class Config:
        env_file = ".env"

setting = Settings()
print(setting.DB_URL)


✅ Keeps secrets safe
✅ No hardcoding credentials

📦 14. Install Command
pip install pydantic


and for config:

pip install pydantic-settings

🧾 15. Summary Table
Concept	What It Does	Example
BaseModel	Defines data shape	class User(BaseModel):
Type hints	Ensure correct data	name: str, age: int
Validation	Stops bad data	"age": "abc" → error
Default values	Fills missing data	in_stock: bool = True
Nested models	Models inside models	User + Address
dict/json	Converts to other forms	user.dict()
pydantic-settings	Loads env configs	.env files
🚀 16. Why Companies Love Pydantic

✅ Prevents crashes
✅ Easy to debug
✅ Great performance
✅ Works perfectly with FastAPI
✅ Makes code secure and professional