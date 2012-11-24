[NodeFly](http://addons.heroku.com/nodefly) is an [add-on](http://addons.heroku.com) for monitoring Node.js applications.

NodeFly injects a lightweight analytics tool into your application
allowing you to see exactly what's going on.
The NodeFly agent tracks overall time per request,
and reports how much time in MySQL, Redis, and other resources.

NodeFly works well for both development and production,
and has helped many people track down memory-leaks and unexpected latencies.

NodeFly is accessible via an API and has supported client libraries for Node.js.

## Provisioning the add-on

NodeFly can be attached to a Heroku application via the  CLI:

<div class="callout" markdown="1">
A list of all plans available can be found [here](http://addons.heroku.com/nodefly).
</div>

    :::term
    $ heroku addons:add nodefly
    -----> Adding nodefly to sharp-mountain-4005... done, v18 (free)

Once NodeFly has been added a `NODEFLY_APPLICATION_KEY` setting will be available in the app configuration and will contain the application key required to begin monitoring your application. This can be confirmed using the `heroku config:get` command.

    :::term
    $ heroku config:get NODEFLY_APPLICATION_KEY
    00000000-0000-0000-0000-000000000000

After installing NodeFly the application should be configured to fully integrate with the add-on.

Add nodefly to your package.json in your web application:

    {
        "name": "my-awesome-application",
        "dependencies": {
            "nodefly": "stable"
        }
    }

Enter the following at the top of your module before any other requires:

    require('nodefly').profile(
        process.env.NODEFLY_APPLICATION_KEY,
        [process.env.APPLICATION_NAME,'Heroku']
    );

You should set `APPLICATION_NAME` using the command line:
    
    :::term
    $ heroku config:set APPLICATION_NAME <A short but descriptive name>

## Local setup

### Environment setup

After provisioning the add-on it’s necessary to locally replicate the config vars so your development environment can operate against the service.

<div class="callout" markdown="1">
Though less portable it’s also possible to set local environment variables using `export NODEFLY_APPLICATION_KEY=value`.
</div>

Use [Foreman](config-vars#local_setup) to reliably configure and run the process formation specified in your app’s [Procfile](procfile). Foreman reads configuration variables from an .env file. Use the following command to add the NODEFLY_APPLICATION_KEY values retrieved from heroku config to `.env`.

    :::term
    $ heroku config -s | grep NODEFLY_APPLICATION_KEY >> .env
    $ more .env

> If you prefer not to install Ruby or Ruby Gems,
> you may use a Node.js implementation of [Foreman](https://npmjs.org/package/foreman).

<p class="warning" markdown="1">
Credentials and other sensitive configuration values should not be committed to source-control. In Git exclude the .env file with: `echo .env >> .gitignore`.
</p>

## Dashboard

<div class="callout" markdown="1">
For more information on the features available within the NodeFly dashboard please see the docs at [mysite.com/docs](mysite.com/docs).
</div>

The NodeFly dashboard allows you to [[describe dashboard features]].

The dashboard can be accessed via the CLI:

    :::term
    $ heroku addons:open nodefly
    Opening nodefly for sharp-mountain-4005…

or by visiting the [Heroku apps web interface](http://heroku.com/myapps) and selecting the application in question. Select NodeFly from the Add-ons menu.

## Troubleshooting

If [[feature X]] does not seem to be [[common issue Y]] then 
[[add specific commands to look for symptoms of common issue Y]].

## Migrating between plans

<div class="note" markdown="1">Application owners should carefully manage the migration timing to ensure proper application function during the migration process.</div>

Use the `heroku addons:upgrade` command to migrate to a new plan.

    :::term
    $ heroku addons:upgrade nodefly:premium
    -----> Upgrading nodefly:premium to sharp-mountain-4005... done, v18 ($49/mo)
           Your plan has been updated to: nodefly:premium

## Removing the add-on

NodeFly can be removed via the  CLI.

<div class="warning" markdown="1">This will destroy all associated data and cannot be undone!</div>

    :::term
    $ heroku addons:remove nodefly
    -----> Removing nodefly from sharp-mountain-4005... done, v20 (free)

## Support

All NodeFly support and runtime issues should be submitted via on of the [Heroku Support channels](support-channels). Any non-support related issues or product feedback is welcome at [NodeFly Support](http://nodefly.com/support).
