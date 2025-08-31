# auto-tweet-youtube-videos

This repository contains an n8n workflow(yt tweet.json) that automatically fetches YouTube video links from a Google Sheet, processes them, shortens the URLs, and posts them as tweets on Twitter (X).

**Workflow overview**

**1. Trigger:**

    Starts on a schedule (every 5 minutes).
    
**2. Gather Data:**

    Reads video URLs from a connected Google Sheet.
    
**3. Extract Video ID:**

    Cleans and extracts the videoId from YouTube URLs.
    Checks for duplicate entries (ignores if already seen).

**4. Check Upload Status:**

    Skips rows that are already marked as uploaded.
    Updates Google Sheet with processing status.

**5. Filter New Videos:**

    Keeps only rows with new, unprocessed videos.

**6. Fetch YouTube Metadata:**

    Calls YouTube Data API v3 to fetch:
    Title
    Channel name
    Tags
    Privacy status (public, unlisted, private)

**7. Process Video Status:**

    Updates Google Sheet with fetched metadata.
    Flags only public videos to move forward.

**8. Shorten Links:**

    Uses Bitly API to shorten the original YouTube URL.
    Updates Google Sheet with the shortened link.

**9. Prepare Tweet:**

    Formats tweet text:
      Video Title
      Shortened Link
      Tags converted to hashtags (#tag)
    Ensures tweet length ≤ 250 characters (keeps buffer for Twitter limit).

**10. Update Status:**

    Marks the row as uploaded in the Google Sheet.

**11. Tweet:**

    Posts the formatted tweet to Twitter (X) using OAuth2 credentials.



**REQUIREMENTS**

Before running this workflow, make sure you have:

1. n8n installed (self-hosted or cloud).

2. A Google Sheets API OAuth2 credential (linked in n8n).

3. A YouTube Data API v3 key.

4. A Bitly API token.

5. A Twitter (X) API OAuth2 credential.


**Google Sheet Setup**

Create a google sheet with following colunms
1. URL
2. shortLink
3. upStatus

the other colunms are automatically updated during the workflow


**How to Use**

1. Import yt tweet.json into your n8n instance.

  1. Google Sheets

  2. YouTube Data API

  3. Bitly API

  4. Twitter (X) API

3. Link your Google Sheet ID in the workflow nodes.

4. Activate the workflow.

The workflow will now check your sheet every 5 minutes, fetch new public videos, and automatically tweet them. 🎉


**Notes**

1. Only public videos are tweeted.
2. Duplicate YouTube links are ignored.
3. Tweets are automatically trimmed to fit within Twitter’s limit.
4. Workflow status is always updated back into the Google Sheet for tracking.
