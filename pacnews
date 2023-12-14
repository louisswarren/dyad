#!/usr/bin/env python3

import os
import requests
import sys
import textwrap
import time

import feedparser
import bs4

def get_kernel_date():
    with open('/proc/version') as f:
        version = f.read()
        datestr = version[version.rfind(', ') + 2:].strip()
    return time.strptime(datestr, '%d %b %Y %H:%M:%S %z')

def get_news():
    url = r'https://archlinux.org/feeds/news/'
    headers = {
        "Accept": "text/rss",
        "Accept-Encoding": "gzip, deflate, br",
        "Accept-Language": "en-US,en;q=0.5",
        "Alt-Used": "archlinux.org",
        "Connection": "keep-alive",
        "Host": "archlinux.org",
        "Upgrade-Insecure-Requests": "1",
        "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:109.0) "
            "Gecko/20100101 Firefox/117.0",
    }
    res = requests.get(url, headers=headers)
    if res.status_code != 200:
        raise Exception(f"Failed ({res.status_code}) on url {url}")
    return res.content.decode(errors='replace')

# For testing, so we aren't downloading each time
def get_news_cached():
    p = 'news.rss'
    if os.path.exists(p):
        print("Reading", p, "from cache", file=sys.stderr)
        with open(p) as f:
            return f.read()
    contents = get_news()
    with open(p, 'w') as f:
        f.write(contents)
    return contents

def body_lines(body_html):
    for para in body_html.split('<p>'):
        if not para.strip():
            continue
        para_text = bs4.BeautifulSoup(para, features="lxml").get_text()
        for line in textwrap.wrap(para_text, 80):
            yield line
        yield ''

def pacnews():
    feed = feedparser.parse(get_news())
    after = get_kernel_date()
    entries = (x for x in feed['entries'] if x['published_parsed'] >= after)

    for entry in sorted(entries, key=lambda x: x['published_parsed']):
        datemsg = time.strftime('%d %B %Y', entry['published_parsed'])
        print((' ' + datemsg + ' ==').rjust(80, '='))
        print('\n'.join(textwrap.wrap(entry['title'], 80)))
        print('-' * 80)
        print('\n'.join(body_lines(entry['summary'])))
        print(entry['link'])
        print()

if __name__ == '__main__':
    pacnews()
