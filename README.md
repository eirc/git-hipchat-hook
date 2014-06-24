# Git HipChat Hook

A simple GIT `post-receive` hook script for notifying a room in HipChat.

## Installation

Clone this repository somewhere in your GIT repository host server.
For example:

```sh
cd /home/git
git clone git://github.com/eirc/git-hipchat-hook.git
```

Clone [hipchat-cli](https://github.com/hipchat/hipchat-cli) somewhere in your GIT repository host server.

```sh
git clone git://github.com/hipchat/hipchat-cli.git
```

Go to the `hooks` directory in a bare repository you want to setup the hooks for and add a `post-receive` script like this one and make sure it's executable.

You can lookup the HipChat room id from the [rooms/list](https://www.hipchat.com/docs/api/method/rooms/list) API or use the HipChat room name (remember to urlencode it)

```sh
#!/bin/sh

HIPCHAT_SCRIPT="/path/to/hipchat_room_message"
HIPCHAT_ROOM="HipChat room name or room_id"
HIPCHAT_TOKEN="1234567890"
HIPCHAT_FROM="GIT"

. /path/to/hipchat-post-receive
```

- If using gitorious make sure to add GIT_PROJECT to the hook:

  ```sh
  GIT_PROJECT="MyScripts"
  ```

- For Redmine make sure to include project and repository identifiers.

  ```sh
  REDMINE_PROJECT="my-project"
  REDMINE_PROJECT_NAME="My Project"
  REDMINE_REPO_ID="my-project"  # optional for "Main repository"
  ```

  Also remember that you'll need to [notify Redmine to refresh the repository](http://www.redmine.org/projects/redmine/wiki/HowTo_setup_automatic_refresh_of_repositories_in_Redmine_on_commit#Git), otherwise the commit won't be parsed yet when you click the link in HipChat. Add the following line before `. /path/to/hipchat-post-receive`:

  ```sh
  curl "http://<redmine url>/sys/fetch_changesets?key=<your service key>&id=<project id>" > /dev/null 2>&1 
  ```

- For GitWeb, CGit, Gitorious and Redmine integrations (optional) add the following configuration to the `post-receive` hook before the `hipchat-post-receive` source line.

  Note that CGit and GitWeb, and Redmine and JIRA, are mutually exclusive.

  ```sh
  CGIT="http://git.example.com/cgit"
  GITWEB="http://gitweb.example.com"
  JIRA="http://jira.example.com"
  REDMINE="http://redmine.example.com"
  GITORIOUS="http://gitorious.example.com"
  ```

And you're done!

## Contributions

* [graffic](http://github.com/graffic) for the sed RegEx to link to Redmine issues.
* [jparise](http://github.com/jparise) for CGit and JIRA integrations.
