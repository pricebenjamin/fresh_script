# fresh_script

This program will search for spotify tracks posted in the HipHopHeads subreddit and add them to a playlist of your choice. HipHopHeads is a subreddit dedicated to everything hiphop, including the latest mixtapes, videos, news, and anything else hip hop related from your favorite artists.

## New features!
[Flask](http://flask.pocoo.org/) has recently been added to the project. You can read up on how to get it setup [here](flask.md).

## Getting started

### Prerequisites

This project uses Python 3.

#### Spotify
You will need to setup a [Spotify developer account][spotify-dev-login] and create a new app from the [dashboard][spotify-app-dashboard].

Fill out the form by giving your app a name (e.g., `fresh_script`) and short description (e.g., "creates Spotify playlist from songs posted on reddit").
Feel free to check "I don't know" when the form asks "What are you building?".
Accept the Terms of Service, and you'll be brought to your new application's analytics page.

The last thing you must do is specify a redirect URI for your application.
This is done by clicking "Edit Settings", and adding a URI to the "Redirect URIs" list.
(If you don't know what this setting is for, don't panic--just add the following: `http://localhost/`)

Take note of the following information; it will be used to configure `fresh_script`:
* client ID
* client secret
* redirect URI
* your spotify username
* playlist ID of the playlist you want to add the tracks to

[spotify-dev-login]: https://developer.spotify.com/dashboard/login
[spotify-app-dashboard]: https://developer.spotify.com/dashboard/applications

#### reddit
You will need to create a reddit account, if you don't already have one. Then, [create a new app][reddit-app-dashboard].

Your app only needs a name (e.g., `fresh_script`) and a redirect URI (e.g., `http://localhost/`); the rest of the fields are optional.

As before, take note of the following:
 * client ID (this is underneath the name of your app and looks something like `Dx_ucAxJTjHMGA`)
 * client secret (if not visible, click "edit" under the app you just made)
 * your reddit username

[reddit-app-dashboard]: https://www.reddit.com/prefs/apps/

#### Setup your credentials

To set up your credentials, create a new file called `credentials.json` in the root of the project with the following contents:

```
{
    "spotify": {
        "username": "[Spotify username]",
        "client_id": "[client id]",
        "client_secret": "[client secret]",
        "redirect": "[redirect uri]"
    },
    "reddit": {
        "username": "[reddit username]",
        "client_id": "[client id]",
        "client_secret": "[client secret]"
    }
}
```
 
Replace the square-bracketed text with the information gathered in the previous steps.

### Installing dependencies
This project uses a dependency manager called [pipenv](https://pipenv.readthedocs.io). Follow the instructions to install it [here](https://pipenv.readthedocs.io/en/latest/install/#installing-pipenv).

The project dependencies are listed in a [Pipfile](https://github.com/pypa/pipfile). Using pipenv, you can install all the dependencies with the following commands:
```bash
cd fresh_script
pipenv install
``` 

Pipenv uses [virtualenv](https://virtualenv.pypa.io/en/stable/) to create a python environment with all the dependencies listed in the Pipfile. Before running the fresh.py script, you must first activate the environment:
```bash
pipenv shell
```

If you wish to deactivate the environment use the command
```bash
exit
```

### Running the script

Running the program is simple. The first time you run it, if you did not create `credentials.json`, you will be prompted to enter your Spotify / reddit credientials. Choose to sort results by hot or new, enter a post limit, and enjoy!

```
python3 fresh.py
```

### Script arguments

The following arguments can be passed to the script

| Short | Long             | Type   | Description |
|-------|------------------|--------|-------------|
| -s    | --sort           | string | Sort by hot, new, rising, random_rising, controversion or top |
| -l    | --limit          | int    | How many posts to grab |
| -t    | --threshold      | int    | Only posts with score above threshold |
| -f    | --fresh          | bool   | Only add tracks with the \[FRESH\] tag |
| -ia   | --include-albums | bool   | Include tracks from albums |
| -v    | --verbose        | bool   | Output songs being added and other info |
| -p    | --playlists      | bool   | List, add, or remove playlists to add songs to |

### Running the script using cron

We can use cron to automatically run the script periodically in order to keep it up-to-date. You will need either macOS or Linux to use cron.

1. Follow the `running the script` instructions to make sure your `.config.ini` file is generated with the required parameters
2. Run `crontab -e` to open the cron editor, which is similar to vim
3. Use the following format to create a line for your cron
    ```
    * * * * * command to be executed
    - - - - -
    | | | | |
    | | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
    | | | ------- Month (1 - 12)
    | | --------- Day of month (1 - 31)
    | ----------- Hour (0 - 23)
    ------------- Minute (0 - 59)
    ```
    For example, you would do the following to run this everyday at 9AM
    ```
    0 9 * * * python /home/jsmith/fresh.py
    ```

## Contributing

I appreciate any help and support. Feel free to [fork](https://github.com/amcquade/fresh_script#fork-destination-box) and [create a pull request](https://github.com/amcquade/fresh_script/compare)
