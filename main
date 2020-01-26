"""
from bs4 import    BeautifulSoup
import requests        #导入requests包
url = 'https://www.wuxiaworld.com/novel/renegade-immortal/rge-chapter-1'
strhtml = requests.get(url)        #Get方式获取网页数据
#print(strhtml.text)
soup=BeautifulSoup(strhtml.text,'lxml')
#


data = soup.find('div', attrs={'class':'caption clearfix'}).find('h4').text

"
<p dir="ltr"><span style="color: rgb(0, 0, 0); background-color: transparent; font-weight: 400; font-style: normal; font-variant: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap">Tie Zhu sat on the side of a little road in the village, looking at the blue sky in a daze. Tie Zhu was not his real name, but a nickname. Due to his poor health as a child his father was afraid he might not live so he was given this nickname as a tradition.</span></p>

print(data)

"""
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import re
import requests
from requests.exceptions import RequestException
from bs4 import BeautifulSoup

def download(url, headers, num_retries=3):
    print("download", url)
    try:
        response = requests.get(url, headers=headers)
        print(response.status_code)
        if response.status_code == 200:
            return response.content

        return None
    except RequestException as e:
        print(e.response)
        html = ""
        if hasattr(e.response, 'status_code'):
            code = e.response.status_code
            print('error code', code)
            if num_retries > 0 and 500 <= code < 600:
                html = download(url, headers, num_retries - 1)
        else:
            code = None
    return html


def find_chapter(url, headers):
    cont = ""
    r = download(url, headers=headers)

    #print(r)
    page = BeautifulSoup(r, "lxml")
    chapter_header = page.find('div', attrs={'class':'p-15'}).find('h4').text

    #all_items = page.find('div',attrs={'id':'chapter-content','class':'fr-view'}).find_all('p', attrs={'dir': 'ltr'})
    #if len(all_items) == 0:
    all_items = page.find('div',attrs={'id':'chapter-content','class':'fr-view'}).find_all('p')


    for all in all_items:
        content = "    "+all.text
        cont = cont+content+"\n"
    return chapter_header,cont


def func1(chapter):
    headers = {
        'User-agent': "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.139 Safari/537.36"
    }
    URL = 'https://www.wuxiaworld.com/novel/renegade-immortal/rge-chapter-'+str(chapter)

    k = find_chapter(URL, headers=headers)
    return k
    #print(k[1])

if __name__ == '__main__':
    file = "./renegade immortal.txt"
    #with open(file, 'w',encoding='utf-8') as f:  # 如果filename不存在会自动创建， 'w'表示写数据，写之前会清空文件中的原有数据！
      #  f.write("Renegade immortal\n\n\n\n\n\n")

    for i in range(717,2088):
        temp = func1(i+1)
        with open(file, 'a',encoding='utf-8') as f:  # 如果filename不存在会自动创建， 'w'表示写数据，写之前会清空文件中的原有数据！

            f.write("第"+str(i+1)+"章" +"\n\n")
            f.write(temp[0])
            f.write("\n\n")
            f.write(temp[1])
            f.write("\n\n\n\n")

