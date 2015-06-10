# Conference APP
A conference app written in Python for Google App Engine

## Task 1: Add Sessions to a Conference

Session has been created as a child of a conference.

I left both date and startTime as Strings. Instead of converting them, I used leading zero format and 24 hour clock:

- Comparing two date in the format "YYYY-MM-DD" is equivalent to comparing two strings.

- Comparting two times in the format "HH:MM" (24 hour clock) is quivalent to comparing two strings.

I created TypeOfSession as Enum type. The rest of the field are Strings.

I didn't create a separate entity for the speaker, but it should be interesting in a future release. A speaker can have: name, email, organization...

## Task 2: Add Sessions to User Wishlist

A WishList has been created as a child of a Profile.

In a WishList we store only the sessionId.

## Task 3: Work on indexes and queries

### Create indexes

For this task I made sure to test most of the queries on local server.

All the new entities that I added use the ancestor key.

### Come up with 2 additional queries

1) Save a ConferenceQueryForms array and use it to display a list of conferences the user is interested in. Something like the "Recommended" list of YouTube.

2) Save a ConferenceQueryForms array and use it to notify the user of the new conferences that have been added during last week. A cron job can be created to notify the users each Sunday.

I implemented the part that saves and retrieves a ConferenceQueryForms array. The API endpoits are:

- setInterestListFilters: Update the filters of the logged in user

- getInterestListFilters: Retreive the filsters of the logged in user

- getConferencesInInterestList: Use the saved ConferenceQueryForms array to query conferences

### Solve the following query related problem: How would you handle a query for all non-workshop sessions before 7 pm?

This query requires two inequality filters. But we are only allowed to have one inequality filter.

One solution to this problem is to convert the inequality on the Enum type to multiple equalities combined with "OR":

 Session.query(ndb.AND(Session.startTime < "19:00", ndb.OR(Session.typeOfSession=TypeOfSession.LECTURE, Session.typeOfSession=TypeOfSession.KEYNOTE)))

## Task 4: Add a Task

Whenever we find a speaker that speaks for more than a session in the same conference, we update the memcache value. The getFeaturedSpeaker API endpoint call returns the last featured speaker.


## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
