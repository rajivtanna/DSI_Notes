# Twitter Access

```
import twitter # Using Twitter Python Library

consumer_key = 'TgLftuTWNsYkXSSXfnmOAZvRM'
consumer_secret = 'HZZQgeWaOCOP1CEgt5Bj8O2Jm9LxhJlf9YTuuCyh6X7lSHDtPt'
access_token_keys = '590840514-ssu4mAZEcborCVErlpJKAqeKMqHgnjmvW6vs9BkO'
access_token_secret = 'oXvB8XMLzeEPomooknqrkeECnIkyYabUZRSeEChCeICu1'
api = twitter.Api(consumer_key=consumer_key,
                  consumer_secret=consumer_secret,
                  access_token_key=access_token_keys,
                  access_token_secret=access_token_secret)
print api.VerifyCredentials()

test = api.GetSearch(raw_query='q=bitcoin%20blockchain')

```
