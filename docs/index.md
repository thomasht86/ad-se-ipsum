# What is this?

This webpage is where I write whatever pops into my mind.

My primary audience is myself - ad se ipsum is "to himself" in latin.

If others find my writings useful - great, but I am not seeking recognition.

## Sample Marimo Notebook


```python {marimo}
import marimo as mo

# define some sample data related to sales of ancient classic books
data = {
    "Book": ["The Great Gatsby", "Mediations", "The Art of War", "The Republic"],
    "Author": ["F. Scott Fitzgerald", "Marcus Aurelius", "Sun Tzu", "Plato"],
    "Sales": [100, 200, 300, 400],
    "Price": [10, 20, 30, 40],
}

mo.ui.table(data, selection=None)
```
