# Exercise: CORS Policy

## Step 0: 
> In package.json: `./realapp/backend`: 

Win users, make sure that lines 6-9 are as follows:

```bash
    "start": "SET DB_FILE=db/dev.db & node app.js",
    "dev": "SET DB_FILE=db/dev.db & nodemon app.js",
    "migrate": "SET DB_FILE=db/dev.db & npx sequelize db:migrate",
    "seed": "SET DB_FILE=db/dev.db & npx sequelize db:seed:all"
```

Mac/linux users, make sure that lines 6-9 are as follows:

```bash
    "start": "DB_FILE=db/dev.db node app.js",
    "dev": "DB_FILE=db/dev.db nodemon app.js",
    "migrate": "DB_FILE=db/dev.db npx sequelize db:migrate",
    "seed": "DB_FILE=db/dev.db npx sequelize db:seed:all"
```

## Set up

In this exercise, you will see how the CORS policy on an application's server
can affect how a different malicious application can access the server.

`cd` into the __malicious-app__ folder.

Run `npm install` and start the server by running `npm run dev`.

Then open another terminal and `cd` into the __real-app/backend__ folder.

Run `npm install`, `npm run migrate`, and `npm run seed`. Start the server by
running `npm run dev`.

## Ambiguous CORS Policy

Take a look at the CORS policy defined in line 26 of the
__real-app/backend/app.js__ file.

This CORS policy sets the `Access-Control-Allow-Origin` header to `"*"`. This is
the most ambiguous CORS policy and allows any origin to make requests to the
server from a typical browser.

Navigate to the frontend of the real application, [http://localhost:5001].
You should see the front page of the real application (a clone of Twitter)
populated with the tweets received from the API of the real application.

Now, navigate to the frontend of the malicious application
[http://localhost:5002]. You should see the front page of the malicious
application which should also be populated with the tweets received from the API
of the real application.

The ambiguous CORS policy on the real application's server allows any
application to make `GET` requests to its server through a browser.

There are some exceptions to the ambiguous CORS policy. Some browsers **DO NOT**
allow you to have this ambiguous CORS policy if the `Content-Type` header of the
request is `"application/json"` and the method of the request is not a `GET`.

Try logging in to the real application by navigating to
[http://localhost:5001/login]. Try submitting the form with
the credentials of `DemoUser` for the `Username` field and `password` for the
`password` field. You should be successful in logging in.

Log out from the real application.

Then try logging in to the real application through the malicious application by
navigating to [http://localhost:5002/login]. If you try submitting the form with
the same credentials (`DemoUser` for the `Username` field and `password` for the
`password` field), then you will get an error.

Open up the Developer Tool's Console to see more specifics on the error. The
browser did not allow you to make the request because the CORS policy was
ambiguous.

## Less-Ambiguous CORS Policy

Comment out the previous CORS policy defined from lines 27-29 of the
__real-app/backend/app.js__ file.

Comment in the next CORS policy defined from lines 32-35 of the
__real-app/backend/app.js__ file.

This CORS policy is a dynamic policy on the server. The syntax is a Regular
Expression which is a way to pattern match strings. This particular pattern
match with any string that starts with `"http://localhost"`. If the beginning of
the URL or the origin of the request starts with `"http://localhost"`, the CORS
policy will set the `Access-Control-Allow-Origin` header to the incoming origin.
Otherwise, it will not set the header at all.

The browser will prevent all other origins ones starting in `"http://localhost"`
from making a request to the server.

Navigate to the frontend of the real application, [http://localhost:5001].
You should see the front page of the real application (a clone of Twitter)
populated with the tweets received from the API of the real application.

Now, navigate to the frontend of the malicious application
[http://localhost:5002]. You should see the front page of the malicious
application which should also be populated with the tweets received from the API
of the real application.

Try logging in to the real application by navigating to
[http://localhost:5001/login]. Try submitting the form with
the credentials of `DemoUser` for the `Username` field and `password` for the
`password` field. You should be successful in logging in.

Log out from the real application.

Then try logging in to the real application through the malicious application by
navigating to [http://localhost:5002/login]. If you try submitting the form with
the same credentials (`DemoUser` for the `Username` field and `password` for the
`password` field), you should be successful in logging in.

Since the CORS policy matches **BOTH** the real application and the malicious
application URLs, the browser will allow both to make **ANY** request to the
real application's server.

## Specific CORS Policy

Comment out the previous CORS policy defined from lines 32-35 of the
__real-app/backend/app.js__ file.

Comment in the next CORS policy defined from lines 38-41 of the
__real-app/backend/app.js__ file.

This CORS policy is a specific policy on the server. The CORS
policy will set the `Access-Control-Allow-Origin` header to
`"http://localhost:5001"` which is the URL of the real application. The browser
will prevent all other origins besides `"http://localhost:5001"` from making
a request to the server.

Navigate to the frontend of the real application, [http://localhost:5001].
You should see the front page of the real application (a clone of Twitter)
populated with the tweets received from the API of the real application.

Now, navigate to the frontend of the malicious application
[http://localhost:5002], or refresh the page if you are already at that URL. You should **NOT** see the front page of the malicious
application this time.

Open up the Developer Tool's Console to see more specifics on the error. The
browser did not allow you to make the request because the CORS policy was
ambiguous.

Try logging in to the real application by navigating to
[http://localhost:5001/login]. Try submitting the form with
the credentials of `DemoUser` for the `Username` field and `password` for the
`password` field. You should be successful in logging in.

Log out from the real application.

Then try logging in to the real application through the malicious application by
navigating to [http://localhost:5002/login]. If you try submitting the form with
the same credentials (`DemoUser` for the `Username` field and `password` for the
`password` field), you will **NOT** be successful.

Open up the Developer Tool's Console to see more specifics on the error. The
browser did not allow you to make the request because the CORS policy was
ambiguous.

Since the CORS policy only matches the real application URL and **NOT** the
malicious application URL, the browser will allow **NOT** allow the
malicious application to make requests to the real application's server.

**IMPORTANT NOTE**: 
- This only prevents **BROWSERS** from not making requests to the server. If a request is made through Postman or any other client, CORS may not be enforced. 
- **Remember**, CORS policy is only enforced on the client-side, not the server-side.

## Extra: Multiple-Origin CORS Policy

In this exercise, you will see how to add multiple origins to a CORS policy on an application's server.

Comment out the previous CORS policy defined from lines 38-41 of the
__real-app/backend/app.js__ file.

Comment in the very last CORS policy defined from lines 45-48 of the
__real-app/backend/app.js__ file.

This CORS policy is a specific policy on the server. The CORS
policy will set the `Access-Control-Allow-Origin` header to
`"http://localhost:5001"` most of the time. However, if the origin of the URL is
`"http://localhost:5002"`, then it will be set to `"http://localhost:5002"`.
That means CORS will not be blocked for `"http://localhost:5001"` OR
`"http://localhost:5002"` from a browser.

Navigate to the frontend of the real application, [http://localhost:5001].
You should see the front page of the real application (a clone of Twitter)
populated with the tweets received from the API of the real application.

Now, navigate to the frontend of the malicious application
[http://localhost:5002]. You should see the front page of the malicious
application which should also be populated with the tweets received from the API of the real application.

Try logging in to the real application by navigating to
[http://localhost:5001/login]. Try submitting the form with
the credentials of `DemoUser` for the `Username` field and `password` for the
`password` field. You should be successful in logging in.

Log out from the real application.

Then try logging in to the real application through the malicious application by
navigating to [http://localhost:5002/login]. If you try submitting the form with
the same credentials (`DemoUser` for the `Username` field and `password` for the
`password` field), you should also be successful on this app to logging in.


[http://localhost:5001]: http://localhost:5001
[http://localhost:5002]: http://localhost:5002
[http://localhost:5001/login]: http://localhost:5001/login
[http://localhost:5002/login]: http://localhost:5002/login

[REF]: https://github.com/appacademy/practice-for-week-12-cors
[CORS]: https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#cors