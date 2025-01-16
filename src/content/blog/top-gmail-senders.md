---
author: Enrique Ayala
pubDatetime: 2025-01-15T20:17:55-06:00
title: Detect Top Consumer Emails by Sender
slug: top-gmail-senders
featured: true
draft: false
tags:
  - gmail
  - gmail api
  - python
description:
  Learn how to detect top consumer emails by sender to manage your Gmail storage effectively.
---
As I approached the free 15 GB quota of space in my personal Google account, I decided to clean up my emails, which were consuming around 30% of the storage. Over the years, I had accumulated many subscription emails. To manage this, I needed a way to detect the top senders by size and perform a manual cleanup by filtering the sender's email. With the help of GitHub Copilot and below references, I created a simple script that successfully analyzed approximately 45,000 messages from around 3,500 senders in over 100 minutes, recovering almost 2 GB of storage (manually).

Here are the steps I followed to achieve this:

## 1. Enable Gmail API
1. Go to the Google Cloud Console.
2. Create a new project or select an existing one.
3. Enable the Gmail API for your project.
    - Select `APIs and Services`.
    - On the left-hand menu, select Library and search for `Gmail API`.


## 2. Set Up Credentials
  1. To create an OAuth client ID, you must first configure your consent screen. 
      - Set it up for for External users.  (this is just for personal use, so minimal setup is fine).
      - Select scope `.../auth/gmail.readonly`
      - Add test user to your target mail to analyze
  
  2. In the left-hand menu click Credentials. Create OAuth 2.0 Credentials:
  3. Click Create Credentials and choose OAuth 2.0 Client ID.
  4. For application type, choose Desktop app. 
  6. Click Create, then Download the credentials.json file.

### 3. Run script

See the script on [GitHub](https://github.com/enrique-ayala/gmail-top-senders) and run it:

```bash
pip install uv
uv run gmail-top-senders.py
```

## References
- [Gmail API](https://developers.google.com/gmail/api/reference/rest)
- [Enabling an API in your Google Cloud project](https://cloud.google.com/endpoints/docs/openapi/enable-api)
- [Google Python quickstart](https://developers.google.com/classroom/quickstart/python)
- [Authentication methods at Google ](https://cloud.google.com/docs/authentication)

