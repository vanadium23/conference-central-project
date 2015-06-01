App Engine application for the Udacity training course.

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


## Task 1: Add Sessions to a Conference
####Explain your design choices
There is a straight as possible. Speakers I've implemented as a string to perfom a simple quiries, but this decision is not scalable, if new tasks will be to add additional informaion to them.

##Task 3: Work on indexes and queries
####Come up with 2 additional queries
```
# sessions that are workshop or lectures
sessions = Session.query(ndb.OR(Session.typeOfSession == SessionType.WORKSHOP, 
                                      Session.typeOfSession == SessionType.LECTURE))
```

```
# get all sessions holding in city
confs = Conference.query(Conference.city=="Default City")
sessions = []
for conf in confs:
            sessions += Session.query(ancestor=conf.key)
```

####Solve the following query related problem
This quieries has two ineqaulity statement, so we need to make one statement from != into IN.
```
seven_pm = datetime.datetime.strptime("19:00", "%H:%M").time()
allowed_types = [key for key in SessionType if key != SessionType.WORKSHOP]
for session in Session.query(ndb.AND(Session.typeOfSession.IN(allowed_types), 
                                     Session.startTime < seven_pm)):
    print '%s - %s - %s' % (session.name, session.typeOfSession, 
                                session.startTime.strftime("%H:%M"))
```




[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
