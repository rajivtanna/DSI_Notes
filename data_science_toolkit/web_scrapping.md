# Web Scrapping

# Beautiful Soup - Popular Python library for web scrapping

[Simple Tutorial](http://web.stanford.edu/~zlotnick/TextAsData/Web_Scraping_with_Beautiful_Soup.html)
[Useful Tutorial](http://zevross.com/blog/2014/05/16/using-the-python-library-beautifulsoup-to-extract-data-from-a-webpage-applied-to-world-cup-rankings/)

[Beautiful Soup Documentation](https://media.readthedocs.org/pdf/beautiful-soup-4/latest/beautiful-soup-4.pdf)


Set-up - Coding Example
```
from bs4 import BeautifulSoup

doc = ['<html><head><title>Page title</title></head>',
       '<body><p id="firstpara" align="center">This is paragraph <b>one</b>.',
       '<p id="secondpara" align="blah">This is paragraph <b>two</b>.',
       '</html>']
doc = ''.join(doc)
doc

soup = BeautifulSoup(doc)
print soup.html.title
print titleTag.contents[0]

# Find all lines with the HTML tag p
soup.findAll('p')

# Or find with tag and some other HTML element
soup.find('p', align="center")

# Can use .select to pick based on CSS selectors
soup.select(".first-item")

```

[Notebook Example](assets/web_scaping_example.ipynb)
