##Course Notes
A new offline-first way to write, read, and review your notes on your laptop and phone.

######Ideas:
- server is Express
- web client is Ampersand consuming REST API
- database is ??? (maybe RethinkDB for easier admin, or IndexedDB for laziness)
- iOS client consumes REST API
- Android client consumes REST API
- Dr. Benson mentioned the problem of doing a linear text search in-browser or client side within an app view, so we could look into something like [ElasticSearch](http://www.elasticsearch.org)

Originally, [hood.ie](http://hood.ie) sounded like it was going to be a good idea for this, but consuming Hoodie on Android natively is going to be too complicated (I think at least, also on iOS it'll have its own problems). I have a different idea:

**Reads**:

1. Check to see if there is any data in localStorage
    1. If no, get the data from the server. Render the views and everything, then store the latest data in localStorage.
    2. If yes, attempt to get the data from the server. If its unavailable, fallback to localStorage.

**Writes**:

1. When writing, autosave frequently to localStorage.
2. When 'Save' is clicked/tapped, attempt to contact server and write to database. If available, show message to user. If unavailable, write to localStorage with some flag indicating 'Sync required'. Sync when server becomes available.

**Accounts**:

1. I don't know enough about accounts to have any concrete ideas, but hopefully there's a way to consume some token once logged in so if the user goes offline, they aren't signed out and there is no possible way for the service to be disrupted.

##Proposed API Endpoints

Each **note** might have an id, date, subject name, author, last time reviewed, folder location, folder it belongs to, amount of times reviewed. In the next iteration, you could have which school it belongs to.

##Dev Stuff

####Starting the fake API for development

You'll need node and npm installed on your machine. From there,

```bash
$ cd server && npm install && npm start
```

This will be on `0.0.0.0:3000`. There are only two API endpoints so far: `http://0.0.0.0:3000/api/people` and `http://0.0.0.0:3000/api/people/:id`. The first supports **GET** and **POST** and the latter supports **GET**, **DELETE**, and **PUT**. Try anything you want because once the server is restarted the JSON will be reset so you can't mess anything up. Also, the `id`'s are just incremented starting at 1 (instead of using UUID or something) so if you go to `http://0.0.0.0:3000/api/people/2` you should expect to see the JSON payload `{"id":2,"firstName":"Bob","lastName":"Saget","coolnessFactor":2}`.

###Data Models

######USERS
- First Name
- Last Name
- Email
- Email isVerified
- Password
- Date they signed up
- Something for two factor auth (phone number???)
- School
- Program
- Major
- Year Level
- Country
- State/Province/Whatever
- City
- Language
- Date they last signed in
- Date they last wrote a note
- Date they last reviewed a note
- AppleID/Google Play Stuff
- Amount of times they signed in

######NOTES
- Title
- Date created
- Date updated
- User it belongs to:
    - All their info for tracking stats 
- Content (this is gonna be hard to store)
- Note number (auto-increment)
- Folder name
- Path to the document
- Time last reviewed
- Times reviewed

######How notes will be accessed:

`https://api.speedstudy.co/bentranter/databases/table-joins.json`

This'll be raw json data.

######How they'll appear to the user in a web browser:

`https://www.speedstudy.co/bentranter/databases/table-joins.html`

That will be a rendered document.

####Other Stuff

When browsing through code, power search for the following stuff:

- `@param` this describes the paramters every function takes, including their type
- `@return` this describes the type of the return value of a function
- `@api` will either be private or public. Private means that this function can only be called by another function, public means that this function can be called by a user action
- `@HTTP` this describes the HTTP method intended to be used by the function. This only appears in `server/api.js`
- `@TODO` this is placed anywhere something still needs to be done
- `@REVIEW` this is placed anywhere the code is in need of a review