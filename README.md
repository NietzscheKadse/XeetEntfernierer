# Disclaimer

This script *should* not delete anything you do not want, I use it rather often, but I'm not responsible for any issues. Use at your own risk if the 1% chance of failure concerns you.

I, Kadse, heavily modified the script heavily from Lyfhael's "DeleteTweets" (Found at https://github.com/Lyfhael/DeleteTweets) since it stopped working. I am not a JS-Expert, so there is a risk of issues, be aware of that. 

The code is provided "AS IS" with no warranties. I claim no Copyright; The code remains, as far as I am concerned, Lyfhael's intellectual property. 


# Prerequisites

This script works only on Google Chrome on a desktop computer (tested on Windows 11 and Linux).
> [!WARNING]
> This script will **not** work in any Firefox Browser or fork of it!

# How to Use

## Guide

FIRST You should copy the entire raw content of the main.js into a text editor of your choice. Don't directly paste it into the console as it will be hard to edit the Options correctly!

- Copy the entire [main.js](main.js) raw content into a text editor. Do NOT paste it directly into the console, as editing the options will be rather difficult.
- Go to https://x.com/
- Open the DevTools with <kbd>CTRL</kbd> + <kbd>SHIFT</kbd> + <kbd>I</kbd> or <kbd>F12</kbd>
- Got to the "Network" tab in DevTools
  - If requests aren't being recorded, press <kbd>CTRL</kbd> + <kbd>E</kbd> and wait 5 seconds
- Click on the Button "Fetch/XHR" to filter for only these requests in the Network tab.
- Now in the same window navigate to your profile and select the "Replies" tab.
- Find the request in the Network tab starting with `UserTweetsAndReplies` and click to open that request, scroll to "Request Headers".
- Find the `authorization` value (should start with `Bearer AAAAAAAAA...`), copy it, and paste it into the `var authorization` variable in your text editor.
- Find the `X-Client-Transaction-Id` value, copy it, and paste it into the `var client_tid` variable in your text editor.
- In your text editor set the variable `var username` to your accounts handle (without the @!)
- Now you can change the options and filters in the section below to your liking (explained with the comments in the code).
- And then you copy then entire content of the text editor and paste it into the console of your DevTools.
- The script will now delete your tweets at an average rate of ~1 Tweet/s or 3600 Tweets/h; Just keep the tab open and wait for it to finish.
- When it's done, launch the script a second time, there sometime are a few leftover tweets. Don't worry, the second time should only take some seconds because there are probably less than 20 Tweets that were not fetched the first time.

This is how it should look like:
<img width="1424" height="1113" alt="image" src="https://github.com/user-attachments/assets/6972600b-f7ae-4c66-b0a7-f5618708edd5" />
Everything above the green line is required. Between the green and red lines there are optional variables you can set, but don't have to.

> [!WARNING]
> Editing anything below the red line will probably result in the script not working or behaving in unforseen ways!

## If you encounter 403/404 errors
Before opening an Issue, try to change the "override resource" variable I specified in the config options.

When you look into the "UserTweetsAndReplies" request, you should see that massive request URL right at the top:  
<img width="821" height="63" alt="image" src="https://github.com/user-attachments/assets/b25a10fc-2c51-4d99-89c9-8e3c90eeb154" />
  
In this Request URL there is that part after "graphql" between two Slashes. Copy only the Part between the Slashes (As highlightet in the image, in this Example it would be WJdO9AzTVxm7lmjLgreeEA ) and set it into the "override_resource" variable.  
<img width="375" height="50" alt="image" src="https://github.com/user-attachments/assets/9c4b6d5f-47a3-4870-87cd-82f17dbe7d0a" />

Now it should work!

Note that with the new Rate Limits and X's stupid handling of these, it is normal to encounter multiple 404 errors in a deletion. Thats completely normal. In that case just wait for the next try. Only if multiple tries return 404/403 you have to troubleshoot.  
If really nothing works, contact me on Discord: arm_11813  
I am more active there than here.  

## Filtering / Options
Now that you filled in the authentication details, you can filter which tweets to delete in the `delete_options` Array. It is right under the Authentication Variables you just filled in.  
  
You can choose to delete only tweets that are within a specific date range. For this, edit "before_date" and "after_date" These will look like that :  
```
	"after_date":new Date('1900-01-01'), // year-month-day
	"before_date":new Date('2100-01-01') // year-month-day
```
Say you want to delete only tweets that happened on July 3rd 2025. You would set the date to that :  
```
	"after_date":new Date('2025-07-02'), // year-month-day
	"before_date":new Date('2025-07-04') // year-month-day
```
It means : Delete tweet AFTER July 2nd 2023, and BEFORE July 4th 2023. These two dates are not included, so it's only what's in-between these dates, and what's before 2nd and 4th, you got it, 3rd.  
NOTE: This is not optimized at all. Meaning the script will go through ALL of your tweets no matter what date you gave. It will only delete tweets that are in the date range you gave, but it will go through all tweets. I will try to optimize it later.  

- You can also choose to remove tweets only if they contain a link in them. Just change `delete_message_with_url_only` to `true`. You can combine this option with the keywords option.
- You can also add tweet ids that you want to keep, so they won't get removed. It's the `tweets_to_ignore` property. Just add the tweet id in the array
- You can also choose to unretweet or not, by changing the "unretweet" property in the delete_options variable. Set it to true if you want to remove retweets too. Otherwise your retweets will be kept and not un-retweeted. It combines with the other filters.
- IF the script removed some tweets but not all, AND that there were no error thrown, then you can set the option "old_tweets":false to true in the delete_options object. Then launch the script again, and it should delete these older tweets. (Probably doesn't work anymore as of May 2025, will fix in the future. Bug-Reports are appreciated!)
- With `do_not_remove_pinned_tweet`, which is set to true by default, it won't remove your pinned tweet by mistake.
- The `delete_specific_ids_only` option oerrides the default tweet search, and only remove tweets from their IDs that you have put in this option(it's an array, example: `["1111111111","22222222222","3333333333"]`). If it is empty (default) this option is ignored. If there is even a single ID in there, all other options are ignored!
- With the `from_archive` option the deletion is WAY faster, no rate-limit, and it's more complete. Download your archive from Twitter then enable `from_archive` by setting it to true in the script, then you'll see a box asking you to drag your tweets.js file into it.

Now you can copy/paste the script in the console, press Enter, and wait for the deletion to complete. It should write "DELETION COMPLETE" in the console when it's over.  

# Support

I allow tickets in German but prefer English if you can speak it, so everyone can understand.  
Feel free to ping me on Discord about a new Issue, I am not that much on GitHub: arm_11813  

# FAQ

## Do I need to include the Bearer part of the authorization key ?
Yes, but I have it prefilled for you. Just replace the *** with the token. But be careful that there is really only ONE whitespace between the "Bearer" string and the token, otherwise it will return a 404.  

## I can't find X-Client-Transaction-Id/authorization or get an Error 400 / 403
In the request list, make sure you select a Request that starts with `UserTweetsAndReplies?v`...  
You get that request only when you are on your own profile and select the "Replies" Tab!  

## Uncaught TypeError: entries is not iterable

If you have this error, please open an Issue so I can update the Query Endpoints in the script.  

# Other

Original Repo:
https://github.com/Lyfhael/DeleteTweets  

Donation link for the original creator:  
https://ko-fi.com/lolarchiver#  
