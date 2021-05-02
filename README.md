# Test-of-Web-Scraping
'''using library'''
import requests
from bs4 import BeautifulSoup
import csv
import pandas as pd

'''get the link'''
key = 'data engineer'
location = 'indonesia'
url = 'https://www.jobstreet.co.id/en/job-search/data-engineer-jobs-in-indonesia/'.format(key,location)

req = requests.get(url)
print(req)

'''Gather the data'''
for page in range (1,10):
    data = []
    req = requests.get(url+str(page))
    soup = BeautifulSoup(req.text,'lxml')
    items = soup.find_all('div','FYwKg _17IyL_0 _2-ij9_0 _3Vcu7_0 MtsXR_0')
    for it in items:
        job_function = it.find('div','FYwKg _2j8fZ_0 sIMFL_0 _1JtWu_0').text
        company_name= it.find('span').text
        job_posting_time = it.find('span','FYwKg _2Bz3E C6ZIU_0 _1_nER_0 _3KSG8_0 _29m7__0').text
        data.append([job_function,company_name,job_posting_time])
 
 df = pd.DataFrame(data)
 newdf = df.rename(columns={0:'Job Function',1:'Company Name',2:'Job Posting Time'})
 newdf.to_csv("jobsteert scraping",sep=',')
 df.head()

        


