# Lecture 2 Data collection and scraping

##### How to get data

1. download data file
2. query data from database
3. query an API
4. scrap data from webpage

> The purpose is not memorizing the API, but understanding the basic idea and get back when we need to do it



##### HTTP request

- GET
- PUT
- POST
- DELETE



##### REST API (Representational State Transfer)

1. use standards HTTP interface
2. stateless - no maintaining connection

> if you are sending your account key along with each call, it's probably REST API.



##### Data formats

1. CSV (comma separated value) files
   1. structured data
   2. use `pandas` module to parse
2. JSON (Javascript object notion) files and strings
   1. hierarchical data
   2. use `json` module to parse
3. HTML/XML (hypertext makeup language / extensible markup language) files and strings
   1. most for the web
   2. use `BeautifulSoup` library



### Regular expression

search for specific elements within data

e.g., find the first occurrence of the string "data"

```python
import re
match = re.search(r"data", text)
```

> r before the string means regex.

1. `re.match()`: check if start of text match
2. `re.search()`: find first match or None
3. `re.finditer()`: iterate over all matches in the text
4. `re.findall()`: return all matches in the text
5. compiled version: `re.compile()`



##### match multiple potential characters

match single character:

1. character 'a': `a`
2. character 'a', 'b' or 'c': `[abc]`
3. character except 'a', 'b' or 'c': `[^abc]`
4. any digit: `\d` or `[0-9]`
5. any alpha-numeric: `\w` or `[a-zA-Z0-9]`
6. whitespace: `\s`
7. any character: `.`

match repeated characters:

1. character 'a' exactly once: `a`
2. character 'a' zero or one time: `a?`
3. character 'a' zero or more times: `a*`
4. character 'a' one or more times: `a+`
5. character 'a' exactly n times: `a{n}`

Poll Answer: 1, 2

##### Grouping

enclose portions of regex with parentheses to get the portions of matching results

##### Substitution

replace the text with outer text or include the text from matching group, using escaped sequences \1, \2, ... 

e.g., 

```python
better_text = re.sub(r"(\w+)\s([Ss])cience", r"\1 \2hmience", text)
```

##### Ordering

- add parenthesis to change the order
- add `?:` in the parenthesis to ignore it as a group
- e.g., `a(?:bc|de)f`

##### Greedy matching

- by default, regex try to capture as much as possible
- use `(.*?)` to capture the least of text possible

e.g., apply `<(.*)>` to `<a>text</a>`, the entire string will match. We need to use `<(.*?)>`