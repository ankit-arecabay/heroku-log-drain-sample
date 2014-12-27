# Processing logs from Heroku apps

One of the most hidden, valuable gems of Heroku is [Logplex](https://devcenter.heroku.com/articles/logplex). It is a log streaming and pub/sub service that takes care of shipping logs from every app running on Heroku to registered consumers. By issuing a simple

```
heroku addons:add papertrail
```

on your app, you can tell Logplex to forward all logs from the app to the [Papertrail](https://addons.heroku.com/papertrail) service. Papertrail is one of several [logging add-on providers](https://addons.heroku.com/#logging) that you can choose as a Heroku developer.

Now, what if you wanted to do some custom processing of your logs? That's easy too! You can tell logplex to forward logs to [any destination that accepts either syslog protocol or HTTP based log streams](https://devcenter.heroku.com/articles/log-drains), including your own custom code.

This repo is an example of an HTTP based log drain that you can add to any Heroku app to get simple traffic stats.

# Deploy to Heroku

You'll need to run this app somewhere. It doesn't have to be run on Heroku, but it's probably the easiest option:

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

# Add to your apps

To forward logs from one of your Heroku apps to this drain app, you can add a drain like this:

```
heroku drains:add -a myapp https://user:$(heroku config:get AUTH_SECRET -a drainapp)@herokuapp.com/log
```

where drainapp app is the app name of your deployment of this repo to Heroku. This presumes that you've deployed this app successfully (using Heroku button or other means).
