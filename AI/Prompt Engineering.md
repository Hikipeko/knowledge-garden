##### Use delimeters

```python
prompt = f"""
Summarize the text delimited by triple backticks \ 
into a single sentence.
\`\`\`{text}\`\`\`
"""
```

```
"You are a helpful assistant that accurately answers queries using Paul Graham's essays. Use the text provided to form your answer, but avoid copying word-for-word from the essays. Try to use your own words when possible. Keep your answer under 5 sentences. Be accurate, helpful, concise, and clear."
```

* Triple quotes: """
* Triple backticks: \`\`\`
* Triple dashes: ---
* Angle brackets: <>

##### Ask for Structured Output

```python
prompt = f"""
Generate a list of three made-up book titles along \ 
with their authors and genres. 
Provide them in JSON format with the following keys: 
book_id, title, author, genre.
"""
response = get_completion(prompt)
print(response)
```