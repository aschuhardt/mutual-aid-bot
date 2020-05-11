# Summary

## Web App
- Provides access to donation services (via PayPal)
- Hosts publicly-accessible statistics
- Provides a front-end for administrative tasks
- Payout links can redirect to a web page with PayPal auth portal

## Discord Bot
- Easy, familiar means of submitting request, querying stats, and holding votes
- Secure pathway for user engagement (i.e. Discord users have to be authenticated, verified)


# User Flows

## Donation
- I have two main ideas for how this would work...
- The first option would let us keep track of the unique Discord usernames of users who made a single donation:
  1. User triggers a bot command
  2. Bot stores a record of the request, including the unique Discord account ID of the user
  3. Bot generates a unique code and attaches it to a URL, which it then PMs to the user
  4. User opens the URL with the unique code
  5. Upon receiving the request for the donation page with the unique code appended to it, the web service performs the following steps:
    1. Generate a donation page with the user's Discord nickname and a PayPal donation link
    2. Either deletes the record created in step 2 so that the URL/code combination cannot be re-used; or sets a flag on the record, invalidating the unique code 
    3. Sends the generated web page to the user so that they can access the donation form
  6. Upon the web page registering that a donation has been successfully made, a new record is stored on the server containing the amount that was donated along with the user's ID (for stats querying and analytics)
- The second option is more privacy-friendly, and consists of a static web page with an optional "nickname" field that can be associated with the transaction if the user so chooses

### Considerations
- Maybe include an optional comment field?
- Maybe introduce a subscription option if PayPal supports that?

## Aid requests
1. User triggers a bot command
2. Bot generates a record server-side containing the unique Discord account ID of the user, the amount request, the time the request was created at, and the time at which the request will expire
3. Bot sends a message in a pre-configured channel and adds a reaction for users to add if they want to vote to accept the request
  - Message should ideally contain the following:
    - A @role ping for all the users in the pre-configured voting role
    - A @user ping for the requesting user
    - The amount requested (USD I guess)
    - Maybe some kind of cute picture or quip (meme, anime tiddies, emojipasta, etc.)
4. If 60% (or some arbitrary pre-configured ratio) of the members of the voting role have provided an approval reaction to the post, then the bot generates a unique URL for the user to provide their paypal info at (not sure how this will work exactly) ... WIP
