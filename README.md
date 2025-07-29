# Disclaimer

It should work just fine, I regularly use the script myself, but if anything wrong happens I am not taking any responsibility. Do not use this script if the 0.1% possible failure scares you. 

I, Kadse, modified this script heavily from Lyfhael's Original "DeleteTweets" (Found at https://github.com/Lyfhael/DeleteTweets) because it didn't work anymore. But I am a JS-Noob, so maybe also don't use it if the slightly higher 1% possibility of failure scares you. 

This entire code is provided "AS IS" and I take absolutely no responsibility and no warranty is given. I take no Copyright for these modifications. The code is, as far as I am concerned, still intellectual property of Lyfhael. 


# Prerequisites

This will ONLY work in Google Chrome at a Desktop Computer (Tested on Windows and Linux)
The script WON'T work in any kind of Firefox-Browser!!!

# Tutorial

## Text Tutorial

FIRST You should copy the entire raw content of the main.js into a text editor of your choice. Don't directly paste it into the console as it will be hard to edit the Options correctly!

- Go to https://x.com/
- Open the DevTools by pressing CTRL + SHIFT + I or F12
- Click on Network tab in the DevTools console
  - If requests are not being recorded, press CTRL + E and wait 5 seconds
- You should now have something like this : ![ULXBFrT](https://github.com/teisseire117/DeleteTweets/assets/43145883/f784c575-efbb-42a2-a217-4700ba715b7e)
- Click on Fetch/XHR : ![KtZYL0L](https://github.com/teisseire117/DeleteTweets/assets/43145883/f0cdb3e8-f9ee-4ce3-ac39-c0a463c00bf6)
- Now go to your X Profile and click on your "Replies" tab
- Now click on the request which Name starts with `UserTweetsAndReplies`, and look for the authorization/X-Client-Transaction-Id values.
- Now, in your text editor, replace the values in the .js from this repository of the according variables, by the values of the two variables you found.
- (Ignore the X-Client-UUID Values in any screenshots you see in this tutorial, this value has been removed! Don't add it!)

## If you encounter 403/404 errors
Before opening an Issue, try to change the "override resource" variable I specified in the config options.

When you look into the "UserTweetsAndReplies" request, you should see that massive request URL right at the top: 
<img width="821" height="63" alt="image" src="https://github.com/user-attachments/assets/b25a10fc-2c51-4d99-89c9-8e3c90eeb154" />

In this Request URL there is that part after "graphql" between two Slashes. Copy only the Part between the Slashes (As highlightet in the image, in this Example it would be WJdO9AzTVxm7lmjLgreeEA ) and set it into the "override_resource" variable.
<img width="375" height="50" alt="image" src="https://github.com/user-attachments/assets/9c4b6d5f-47a3-4870-87cd-82f17dbe7d0a" />

Now it should work!

Note that with the new Rate Limits and X's stupid handling of these, it is normal to encounter multiple 404 errors in a deletion. Thats completely normal. You only need to open an Issue here or change the "override_resource" variable when it doesn't even start, even after a minute of trying!

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
When it's over, launch the script a second time, there sometime are a few leftovers. Don't worry, the second time should only take some seconds because there are probably less than 10 Tweets to fetch at all.

# Support

I allow tickets in German 🇩🇪 but prefer English if you can speak it, so everyone can understand.

# FAQ

## Do I need to include the Bearer part of the authorization key ?
Yes, but I have it prefilled for you. Just replace the *** with the token.

## I can't find X-Client-Transaction-Id/authorization or get an Error 400 / 403
In the request list, make sure you select a Request that starts with `UserTweetsAndReplies?v`...
You get such a request only when you are on your own profile and select the "Replies" Tab.

## Uncaught TypeError: entries is not iterable

If you have this error, please open an Issue so I can update the Query Endpoints in the script.

# Other

Original Repo:
https://github.com/Lyfhael/DeleteTweets

Donation link for the original creator:
https://ko-fi.com/lolarchiver#
