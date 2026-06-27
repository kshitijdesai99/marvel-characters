Perfect. Here is **one tiny end-to-end example** 👇

## 1. YAML file

Imagine you have a file called `config.yaml`:

```yaml
target: strength

num_features:
  - height
  - weight

cat_features:
  - hero_type

parameters:
  learning_rate: 0.01
  max_depth: 5
```

This is just config written outside Python.

---

## 2. Python reads YAML into dictionary

```python
import yaml

with open("config.yaml", "r") as f:
    config_dict = yaml.safe_load(f)

print(config_dict)
```

Output:

```python
{
    "target": "strength",
    "num_features": ["height", "weight"],
    "cat_features": ["hero_type"],
    "parameters": {
        "learning_rate": 0.01,
        "max_depth": 5
    }
}
```

Now YAML became a normal Python dictionary.

---

## 3. Convert dictionary into Pydantic object

```python
from typing import Any
from pydantic import BaseModel


class ProjectConfig(BaseModel):
    target: str
    num_features: list[str]
    cat_features: list[str]
    parameters: dict[str, Any]


config = ProjectConfig(**config_dict)
```

Now `config` is a Pydantic object.

So instead of:

```python
config_dict["target"]
```

you can do:

```python
config.target
```

---

## 4. Pass Pydantic object into preprocessing function

```python
def preprocess_data(df, config: ProjectConfig):
    X = df[config.num_features + config.cat_features]
    y = df[config.target]

    return X, y
```

Use it like:

```python
X, y = preprocess_data(df, config)
```

The function uses:

```python
config.num_features
config.cat_features
config.target
```

---

## 5. Pass same object into training function

```python
def train_model(X, y, config: ProjectConfig):
    learning_rate = config.parameters["learning_rate"]
    max_depth = config.parameters["max_depth"]

    print("Training model...")
    print("Target:", config.target)
    print("Learning rate:", learning_rate)
    print("Max depth:", max_depth)
```

Use it like:

```python
train_model(X, y, config)
```

---

# Full flow in one code block

```python
import yaml
from typing import Any
from pydantic import BaseModel


class ProjectConfig(BaseModel):
    target: str
    num_features: list[str]
    cat_features: list[str]
    parameters: dict[str, Any]


# 1. YAML file -> Python dictionary
with open("config.yaml", "r") as f:
    config_dict = yaml.safe_load(f)


# 2. Python dictionary -> Pydantic object
config = ProjectConfig(**config_dict)


# 3. Pass Pydantic object into functions
def preprocess_data(df, config: ProjectConfig):
    X = df[config.num_features + config.cat_features]
    y = df[config.target]
    return X, y


def train_model(X, y, config: ProjectConfig):
    learning_rate = config.parameters["learning_rate"]
    max_depth = config.parameters["max_depth"]

    print("Training model...")
    print("Target:", config.target)
    print("Learning rate:", learning_rate)
    print("Max depth:", max_depth)


X, y = preprocess_data(df, config)
train_model(X, y, config)
```

---

## The important idea 🧠

You create `config` once:

```python
config = ProjectConfig(**config_dict)
```

Then pass it everywhere:

```python
preprocess_data(df, config)
train_model(X, y, config)
```

So `config` becomes the **single source of truth** for your project settings.
