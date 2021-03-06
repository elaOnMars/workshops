# NLP

### Setup

As usual, you'll need to sign up. It's free. And you'll need a Google/Gmail account.

- Click "Sign up for Free"
- Log in with Google
- Choose "Create Agent" (an agent is like a dialogflow bot)
- Name it "Blank agent"
- In the sidebar, chose "Prebuilt Agents"
- Then in the main area, find the logo for the "Small Talk" prebuilt agent (Note, this is _not_ the "Small Talk" option in the left-side menu)
- Just leave the "Link to Google Project" line empty and hit OK
- Wait and then click "Proceed to Agent"
- This is tricky ... now in the _drop-down_ menu, chose "Small-Talk." Again, not the "Small Talk" item that's always in the sidebar. Look for the hyphen in `Small-Talk`. That's the right one.
- Now, to end this craziness, let's rename it. Click the gear next to `Small-Talk` (with the hyphen)
- Call it "My-Workshop-Bot"
- Click "Save"

### Play a little

Find the "Try it now" box at the top and try typing some random phrases that might constitute small talk. What happens?

Pay close attention to the "Intent" and "Action" areas.

Also try things that might be casual synonyms for "yes" and "no."

## Update your Dexter bot


```
+ *
% do you like dogs
$ GET https://api.api.ai/v1/query?v=20150910&query=<call>encode_uri <star></call>&lang=en&sessionId=<_platformId> {"headers":{"Content-Type":"application/json", "Authorization": "<bot apiai>"}}
* ${{result.action}} == smalltalk.confirmation.yes => <set dogvar=yes> Do you like cats?
* ${{result.action}} == smalltalk.confirmation.no => <set dogvar=no> Do you like cats?

+ *
% do you like cats
$ GET https://api.api.ai/v1/query?v=20150910&query=<call>encode_uri <star></call>&lang=en&sessionId=<_platformId> {"headers":{"Content-Type":"application/json", "Authorization": "<bot apiai>"}}
* ${{result.action}} == smalltalk.confirmation.yes =>  <set catvar=yes> {@ answered <get dogvar> <get catvar>}
* ${{result.action}} == smalltalk.confirmation.no => <set catvar=no> {@ answered <get dogvar> <get catvar>}

+ *
$ GET https://api.api.ai/v1/query?v=20150910&query=<call>encode_uri <star></call>&lang=en&sessionId=<_platformId> {"headers":{"Content-Type":"application/json", "Authorization": "<bot apiai>"}}
* ${{result.fulfillment.speech}} != "" => ${{result.fulfillment.speech}} 
- Sorry, I have no idea what you just said.

> object encode_uri javascript
    return encodeURIComponent(args[0])
< object

! var apiai = Bearer putyourapikeyhere
```

Update the yes/no handler to: 

```
+ handle yesno *
* <star> == smalltalk.confirmation.yes => <set openended-answer=yes> {@ openended reset and route}
* <star> == smalltalk.confirmation.no => <set openended-answer=no> {@ openended reset and route}
- Is that a yes or a no? ^buttons("Yes!", "No!")
```

## Connect your bot to API.ai

Finally, we wire up the NLP ...

- Back at the API.ai settings page, copy the "Client Access Token"
- Switch to your Dexter bot
- Paste the "Client Access Token" at the very top of your bot script.
- In front of the token, add `! var apiai = Bearer ` so it looks something this:

Walk through changes in the [cat-dog-nlp](./cat-dog-nlp.rs) script.


```
! var apiai = Bearer ab12cd34ef56ab78cd90ef12
```
