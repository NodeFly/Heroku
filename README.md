[NodeFly](http://addons.heroku.com/nodefly) is an [add-on](http://addons.heroku.com) for monitoring Node.js applications.

NodeFly injects a lightweight analytics tool into your application
allowing you to see exactly what's going on.
The NodeFly agent tracks resources spent fulfilling each web request,
as well as overall application health.

NodeFly works well for both development and production,
and has helped many people boost performance.

NodeFly is _only_ available for Node.js.

## Provisioning the add-on

A NodeFly account can be created via the Heroku CLI:

    $ heroku addons:add nodefly
    -----> Adding nodefly to sharp-mountain-4005... done, v18 (free)

Once NodeFly has been added a `NODEFLY_APPLICATION_KEY` 
setting will be inserted into your application environment.
This can be confirmed using the `heroku config:get` command.

    $ heroku config:get NODEFLY_APPLICATION_KEY
    00000000-0000-0000-0000-000000000000

After registration, you _must_ configure your application to use the NodeFly agent as follows.

Add the nodefly module to your `package.json` in your web application:

    {
        "name": "my-awesome-application",
        "dependencies": {
            "nodefly": "stable"
        }
    }

Enter the following at the top of your application before any other requires:

    require('nodefly').profile(
        process.env.NODEFLY_APPLICATION_KEY,
        [process.env.APPLICATION_NAME,'Heroku']
    );

You should set `APPLICATION_NAME` using the command line:
    
    $ heroku config:set APPLICATION_NAME <A short but descriptive name>

## Dashboard

The NodeFly dashboard allows you to view the metrics collected by the NodeFly agent.

![NodeFly Dashboard](//raw.github.com/gist/7242786cbbcf5b044971/aa844493dfdd04bf53c5c9423f9dce8d42a38e62/dashboard.png)

The NodeFly Heroku Console can be accessed via the CLI:

    $ heroku addons:open nodefly
    Opening nodefly for sharp-mountain-4005…

or by visiting the [Heroku apps web interface](http://heroku.com/myapps) and selecting the application in question. 
Select NodeFly from the Add-ons menu.

From the NodeFly-Heroku console, 
you can open the dashboard without requiring an additional login; 
you will be automatically connected to the dashboard of your Heroku instance.

## Local setup

### Environment setup

After provisioning the add-on it’s necessary to locally replicate the config vars so your development environment can operate against the service.

When running locally in development mode, we recommend _changing_ your `APPLICATION_NAME` environment variable.
This will differentiate the development and production environments in your analytics.
You do _not_ have to change your `NODEFLY_APPLICATION_KEY`.

Use [Foreman](//devcenter.heroku.com/articles/procfile#developing-locally-with-foreman) 
to reliably configure and run the process formation specified in your app’s 
[Procfile](//devcenter.heroku.com/articles/procfile#declaring-process-types).
Foreman reads configuration variables from an .env file. 
Use the following command to add the NODEFLY_APPLICATION_KEY values retrieved from heroku config to `.env`.

    $ heroku config -s | grep NODEFLY_APPLICATION_KEY >> .env
    $ more .env

>  We also recommend a pure Node.js implementation of [Foreman](https://npmjs.org/package/foreman).

## Troubleshooting

After configuration, NodeFly is automatic.
If you should experience any issues, please let us know immediately.
There are a number of ways to get help:

- If you have a copy of the error, you can open a issue on our [Github Issue Tracker](https://github.com/NodeFly/NodeFly/issues)
- We also accept email question at support@nodefly.com
- Message us on [our twitter](https://twitter.com/NodeFly)

## Migrating between plans

Application owners should carefully manage the migration timing to ensure proper application function during the migration process.

Use the `heroku addons:upgrade` command to migrate to a new plan.

    $ heroku addons:upgrade nodefly:premium
    -----> Upgrading nodefly:premium to sharp-mountain-4005... done, v18 ($49/mo)
           Your plan has been updated to: nodefly:premium

## Removing the add-on

NodeFly can be removed via the  CLI.

This will destroy all associated data and cannot be undone!

    $ heroku addons:remove nodefly
    -----> Removing nodefly from sharp-mountain-4005... done, v20 (free)

