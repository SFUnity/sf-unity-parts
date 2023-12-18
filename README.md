SF Unity Parts
===========

SF Unity Parts is a web-based system for tracking parts through the design and manufacturing cycle. It assigns
part numbers with which CAD files can be saved to Onshape and stores information about parts'
current manufacturing status.

SF Unity Parts is written in Ruby using the [Sinatra](http://sinatrarb.com) framework and uses MySQL as the
backing datastore. Development and production are run on UNIX (OS X and Ubuntu), so there are no guarantees
it'll work on Windows.

SF Unity Parts is built on Cheesy Parts, a tool created by Pat Fairbank at [Team 254](https://github.com/Team254) and Deep Blue Parts, a fork created by [Team 199](https://github.com/DeepBlueRobotics/).

## Team 9038 Specific Features

Our fork differs from 254's in a few ways:

1. Users can edit their own account settings, including changing passwords, names, and choosing from one of several stylesheets.
1. Workflow, status options, and other attributes are generally optimized for our team's resources.
1. Secure file upload is added so that members with edit privileges may upload drawings, documentation, and toolpath files. Each part and assembly carries revision letters that are automatically managed by the program. Lite version control is offered, with the ability to restore previous revisions if user is an administrator.
1. Slack and Trello integeration is offered, since teams may use both tools for communication and task tracking.
1. Additional dashboards and reports are offered for use.
1. Includes compatibility with bootstrap 4.

## Planned Additions
1. STL upload feature, allowing STL models to be uploaded to the database, so members assigned to 3D print a part can find the status of the part and grab the model of it from the same place, just as they would if they were going to manufacture something on the CNC router
1. Link to relevant Onshape documents in the descriptions of assemblies
1. Allow users to include COTS parts in assemblies in the parts manager

## Deploying using Docker

If you want to test-drive the database, you can use Docker to quickly set up the server.

Prerequisites:
 * [Docker](https://docs.docker.com/install/)
 * [Docker compose](https://docs.docker.com/compose/install/)

To run SF Unity Parts in a container:

1. Make sure Docker is running.
1. Populate `config.json` with parameters for the dev and prod environments. `config.json.docker` contains all the presets you would need to run with the provided `docker-compose.yml`. Feel free to change the database, username, and password fields, as long as you do it in both the config and compose files.
1. Run `docker-compose up -d web`. This will build and run your SF Unity Parts server in the background.
1. Run `docker-compose run migrations`. This will perform the database migration operations that are needed. You may need to run this more than once if an error occurs.
1. You're now ready to access the server at `localhost:9000`! 
1. See the text below the Development instructions to login.
1. When you want to stop the server, you can do so with `docker-compose down`.

## Development

Prerequisites:

* Ruby 2.3 and development packages
* [Bundler](https://bundler.io/)
* MySQL and development packages
* g++ for installation of some dev packages (optional)

To run SF Unity Parts locally:

1. Create an empty MySQL database and a user account with full permissions on it.
1. Populate `config.json` with the parameters for the development and production environments. Set
`enable_wordpress_auth` to false and `members_url` to blank; they are used for a single sign-on (SSO)
mechanism specific to Team 254. Make sure to add your Slack and Trello API tokens if using those features.
1. Run `bundle install`. This will download and install the gems that SF Unity Parts depends on.
1. Run `bundle exec rake db:migrate`. This will run the database migrations to create the necessary tables in
MySQL.
1. Run `ruby parts_server_control.rb <command>` to control the running of the parts server, where
`<command>` can be one of `start`|`stop`|`run`|`restart`.

The database migration will create an admin account (username "deleteme@team199.org", password "shark-tank")
that you can use to first get into the system and create other accounts. It is highly recommended that you
delete this account after having created your own admin account.

Uploads are stored in `uploads/` and at this time can only be directly managed by logging into your server.

## Contributing

If you encounter a bug, please create a GitHub issue and let us know so that we can look into it.

If you have a suggestion for a new feature in Cheesy Parts, create an issue on GitHub or shoot an e-mail to
[pat@patfairbank.com](mailto:pat@patfairbank.com). Or if you have some Ruby-fu and are feeling adventurous,
fork Team 254 and send a pull request. If you are interested in adding to Deep Blue Parts, please contact Matt (@morops) or Cole (@icecube45).
