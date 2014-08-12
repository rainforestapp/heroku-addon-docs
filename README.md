[Rainforest](https://addons.heroku.com/marketplace/rainforest) is an [add-on](http://addons.heroku.com) for providing insanely simple integration testing for your Heroku app.

Adding functionality X to an application provides benefits X, Y and Z. [[Sell the benefits here! Don't skimp - developers have many options these days.]]

Rainforest is accessible via an API and has supported client libraries for [[Java|Ruby|Python|Node.js|Clojure|Scala]]*.

## Provisioning the add-on

Rainforest can be attached to a Heroku application via the  CLI:

> callout
> A list of all plans available can be found [here](https://addons.heroku.com/marketplace/rainforest).

```term
$ heroku addons:add rainforest
-----> Adding rainforest to sharp-mountain-4005... done, v18 (free)
```

Once Rainforest has been added a `ADDON-CONFIG-NAME` setting will be available in the app configuration and will contain the [[variable purpose, i.e. "canonical URL used to access the newly provisioned Rainforest service instance."]]. This can be confirmed using the `heroku config:get` command.

```term
$ heroku config:get ADDON-CONFIG-NAME
http://user:pass@instance.ip/resourceid
```

After installing Rainforest the application should be configured to fully integrate with the add-on.

## Dashboard

> For more information on the features available within the Rainforest dashboard please see the docs at [docs.rainforestqa.com](https://docs.rainforestqa.com/).

The Rainforest dashboard allows you to manage tests, request runs and to view results.

The dashboard can be accessed via the CLI:

```term
$ heroku addons:open rainforest
Opening rainforest for sharp-mountain-4005…
```

or by visiting the [Heroku apps web interface](http://heroku.com/myapps) and selecting the application in question. Select Rainforest from the Add-ons menu.

## Getting started

### Writing your first test

1. Click "New test"
2. Enter a relevant title for your test. In our case "Login into Twitter" would be a good name.
3. Enter the URL for this test. You only need to enter the portion of the URL starting at the first "/". In our case "/login" (and not "https://twitter.com/login").
4. Click "Create test!"

![Create a test](https://docs.rainforestqa.com/images/screenshots/screen2.png)

### Creating steps

A step contains an __action__ and a __question__ for the tester to answer. The question
should be answerable with yes or no. The question should be phrased so that yes is
a success and no is a failure. There's more [detail about steps here](/pages/step-writing-guide.html#what_is_a_step).

Our sample __step action__ is going to be "Enter the username
'rainforest-twitter-example' and password '8423ufdslk'".

Our sample __step question__ will be 'Have you been logged in?'.

![Adding Steps](https://docs.rainforestqa.com/images/screenshots/screen3.png)


### Re-using Tests

Lets create another test named "Send a new tweet".

Enter a second step with the action "Locate the compose a tweet box and
enter the current date and time. Then, Click the 'tweet' button." And the
response: "Was the message you added displayed on the Tweet Stream?"

Now, since we can only send tweets when we're logged in, we want to reuse the
first test we wrote which tests the login as part of this test, so our tester is logged in before trying to tweet.

Click "Add new test step" and select the "Login to Twitter" test we created earlier. Drag the newly added
step to the top of the steps so it gets executed first. 

![Reusing tests](https://docs.rainforestqa.com/images/screenshots/screen4.png)

In this way it's simple to re-use existing tests and keep your test suite modular and easy to maintain.

## Troubleshooting

If you find you get unexpected failures in your tests, this may be for a number of reasons. Common reasons include;

* Using domain specific knowledge
* Unspecific instructions or questions

You can request help by contacting us via the web interface.

## Migrating between plans

Use the `heroku addons:upgrade` command to migrate to a new plan. Your plan change will be reflected immediately.

```term
$ heroku addons:upgrade rainforest:newplan
-----> Upgrading rainforest:newplan to sharp-mountain-4005... done, v18 ($49/mo)
       Your plan has been updated to: rainforest:newplan
```

## Removing the add-on

Rainforest can be removed via the  CLI.

> warning
> This will destroy all associated account data and cannot be undone!

```term
$ heroku addons:remove rainforest
-----> Removing rainforest from sharp-mountain-4005... done, v20 (free)
```

## Support

All Rainforest support and runtime issues should be submitted via on of the [Heroku Support channels](support-channels).