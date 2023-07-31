---
title: Scraping LinkedIn
---
This is the first part of a series where I will build a [[Automated job application system]]. An important component of an automated job application system is finding jobs to apply to. I eventually plan to incorporate multiple sites as data sources, each with its own scraper. For now, I will use only LinkedIn. 

This series assumes some basic pre-requisites such as the knowing [[how to manage python environments]] and install packages. To start, import Beautiful Soup and Requests:
```
from bs4 import BeautifulSoup
import requests
```

Now we need a webpage to scrape. Navigate to LinkedIn, search for a job, and copy the URL. This is what the URL for Data Engineer in the U.S. looks like: 
```
url = "https://www.linkedin.com/jobs/search/?currentJobId=3679487498&distance=25&f_E=2&geoId=90000079&keywords=Data%20Engineer"
```

Use Requests to bring in the page and parse it with BeautifulSoup:
```
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')
```

The ```soup``` variable now holds the content of the webpage at the URL we specified. We want our scraper to collect links to job postings for our system to apply to. We can use BeautifulSoup to extract these links. To do so, we must inspect the webpage and identify the HTML element(s) we want to target. In this case, our links are part of a series of HTML list items (```<li>```) in an parent container that has a class attribute ```jobs-search__results-list```. We iterate over the list items, extracting each link using its class attribute:
```
links = []
for posting in soup.find(class_='jobs-search__results-list').find_all('li'):
    links.append(posting.find('a', class_='base-card__full-link')['href'])
```

For more information detailing how to use BeautifulSoup, see the [documentation](https://beautiful-soup-4.readthedocs.io/en/latest/#searching-the-tree). In the next part, I will expand our scope to scrape evert page of results, not only the first. Stay tuned!
