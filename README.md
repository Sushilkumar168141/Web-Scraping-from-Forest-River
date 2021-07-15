# Web-Scraping-from-Forest-River
In this project, I created a python script to collect data for about 100 brands and 10 zip codes for each branch from Forest River Inc. (https://forestriverinc.com/rvs/dealer-locator), and store it in a text file and csv file, for further analysis.

### Steps
1. Importing  libraries
```python
from http_request_randomizer.requests.proxy.requestProxy import  RequestProxy
from selenium import webdriver
from selenium.webdriver.support.ui import Select
import time
import urllib.request
from inscriptis import get_text
from selenium.webdriver.common.keys import Keys
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
import requests
import string
import lxml.html as lh
import pandas as pd
from bs4 import BeautifulSoup
from fake_useragent import UserAgent
import csv
```
2. Get a list of  different proxies to handle captcha
```python
req_proxy = RequestProxy()
proxies = req_proxy.get_proxy_list()
```
3. Collecting data from website
```python
zip_codes_done = 1
brands_done = 0
count = 0
proxies_start=23
for i in  range(proxies_start, len(proxies)):
    PROXY = proxies[i].get_address()
    webdriver.DesiredCapabilities.CHROME['proxy'] = {
        "httpProxy":PROXY,
        "ftpProxy":PROXY,
        "sslProxy":PROXY,

        "proxyType":"MANUAL",
    }
    
    print(i,  proxies[i].get_address())
    proxies_start=i+1
    running = True;
    while(running):
        options = webdriver.ChromeOptions()
        options.add_argument('--disable-notifications')
        options.add_argument('--mute-audio')
        options.add_argument("start-maximized")
        options.add_experimental_option("excludeSwitches", ["enable-automation"])
        options.add_experimental_option('useAutomationExtension', False)
        options.add_argument("window-size=800,600")
        # a = ua.random
        user_agent = ua.random
        print(user_agent)
        options.add_argument(f'user-agent={user_agent}')
        count = 0;
        driver = webdriver.Chrome("D:\Chromedriver\chromedriver.exe", chrome_options=options)
        driver.execute_script("Object.defineProperty(navigator, 'webdriver', {get: () => undefined})")
        
        try:
            zip_codes_done,  brands_done, count, status = getData(zip_codes_done, brands_done, count)
            # print(count)
            if (status == False):
                running = False
        except Exception as e:
            driver.quit()
            proxies_start=i+1
            break;
```
4. Saving the data collected in a text file for particular date.
```python
def saveData(data):
    with open('data.txt', 'a') as file:
        file.write(data)
```

##### Output is stored  in text file uploaded in this repository.
