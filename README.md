[Rainforest](https://addons.heroku.com/marketplace/rainforest) is an [add-on](http://addons.heroku.com) for providing insanely simple integration testing for your Heroku app.

Rainforest allows you to setup blazing fast integration tests and run them against your app against all major browsers in under thirty minutes. Faster than automated testing. Light-years quicker than manual testing.

We've designed Rainforest for Continuous Deployment. We have plugins to integrate seamlessly into your existing deployment workflow. If you get a failure with Rainforest, we fail your build just like Rspec.

## Provisioning the add-on

Rainforest can be attached to a Heroku application via the  CLI:

> callout
> A list of all plans available can be found [here](https://addons.heroku.com/marketplace/rainforest).

```term
$ heroku addons:add rainforest
-----> Adding rainforest to sharp-mountain-4005... done, v18 (free)
```

Rainforest requires zero configuration in your application; you should start by going to the dashboard to create your first test.

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

## Integrating with your CI Server

Rainforest is the simplest way of doing integration testing. Since integration tests play such a big role in assuring the quality of your software, it is important to run these tests on a regular basis. 

The best way to achieve this is to integrate Rainforest with your existing continuous integration system or with your deployment system. 

We've built a tool that lets you achieve this in minutes. 

### rainforest-cli

To integrate Rainforest with any CI server or deployment tool, we wrote a Command Line Interface that triggers a run of your tests from any script. This tool is called [rainforest-cli](https://github.com/rainforestapp/rainforest-cli) and is compatible with all build systems.

It's easy to install (requires ruby 1.9 or newer):

```
gem install rainforest-cli
```

You can then run all of your tests using the following command:

```
rainforest run --token <YOUR API TOKEN> --fg --fail-fast all
```

You can find your API token in the [settings](https://app.rainforestqa.com/settings/account) pane of your account.

The `--fg` option ensures that the command only returns when the run is complete. If you do not specify it, the tool will trigger the run, but will then exit immediately. 

The `--fail-fast` option makes the rainforest tool return immediately after the first failure. 
For all options, refer to the [Github page](https://github.com/rainforestapp/rainforest-cli) of the tool.

### Suggested Workflow

The perfect workflow will of course depends on your application and your specific needs, but here's one we have found works well for most. 

We found it extremely powerful to have our CI server handle our deployments. We've configured it to deploy to our staging server whenever new code is merged into the `staging` branch. 

When new code is merged into that branch, the following happens:

1. All the units tests are run
2. The code new code is pushed to your staging server, database migrations are run, etc
3. The rainforest tools triggers a new run of all your tests against the staging server

Once all your tests are green, you can confidently deploy that code to your production server. Hopefully, your process is as simple as merging the `staging` branch into the `production` branch. This triggers a deploy to production and you code is now live and bug free.

### Running Only Specific Tests

The easiest to achieve this is to use tags. See [related documentation](https://docs.rainforestqa.com/tags/). 

If you tag all of your tests with a tag named `run-me`, you can do so with the following command.

```
rainforest run --token <YOUR API TOKEN> --tag run-me --fg --fail-fast
```

You can also use tags to only run a subset of tests that correspond to the subsystem being deployed.

### More

If the command line interface does not provide enough flexibility for your needs, we also have an [API](https://docs.rainforestqa.com/runs-api/) that you can use directly. 

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