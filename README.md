[![made-with-python](https://img.shields.io/badge/Made%20with-Python-blue.svg)](https://www.python.org/)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://github.com/l3uddz/traktarr/blob/master/LICENSE)
[![Feature Requests](https://img.shields.io/badge/Requests-Feathub-blue.svg)](http://feathub.com/l3uddz/traktarr)
[![Discord](https://img.shields.io/discord/381077432285003776.svg)](https://discord.gg/xmNYmSJ)


# traktarr

**traktarr** uses Trakt to add new shows into Sonarr and new movies into Radarr.

Types of Trakt lists supported:

- Official Trakt lists

  - Trending

  - Popular

  - Anticipated

  - Boxoffice

- Public lists

- Private lists*

  - Watchlist

  - Custom list(s)

\* Support for multiple (authenticated) users.

---

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [traktarr](#traktarr)
- [Demo](#demo)
- [Requirements](#requirements)
- [Installation](#installation)
	- [1. Base Install](#1-base-install)
	- [2. Create a Trakt Application](#2-create-a-trakt-application)
	- [3. Authenticate User(s) (optional)](#3-authenticate-users-optional)
- [Configuration](#configuration)
	- [Sample Configuration](#sample-configuration)
	- [Core](#core)
	- [Automatic](#automatic)
		- [Personal Watchlists](#personal-watchlists)
		- [Custom Lists](#custom-lists)
			- [Public Lists](#public-lists)
			- [Private Lists](#private-lists)
	- [Filters](#filters)
		- [Movies](#movies)
		- [Shows](#shows)
	- [Notifications](#notifications)
		- [Pushover](#pushover)
		- [Slack](#slack)
	- [Radarr](#radarr)
	- [Sonarr](#sonarr)
		- [Tags](#tags)
	- [Trakt](#trakt)
- [Usage](#usage)
	- [Automatic (Scheduled)](#automatic-scheduled)
		- [Setup](#setup)
		- [Customize](#customize)
	- [Manual (CLI)](#manual-cli)
		- [General](#general)
		- [Movie (Single Movie)](#movie-single-movie)
		- [Movies (Multiple Movies)](#movies-multiple-movies)
		- [Show (Single Show)](#show-single-show)
		- [Shows (Multiple Shows)](#shows-multiple-shows)
	- [Examples (Manual)](#examples-manual)

<!-- /TOC -->

---




# Demo

Click to enlarge.

[![asciicast](assets/demo.gif)](https://asciinema.org/a/180044)


# Requirements

1. Ubuntu/Debian

2. Python 3.5 or higher (`sudo apt install python3 python3-pip`).

3. requirements.txt modules (see below).

# Installation

## 1. Base Install

Install traktarr to be run with `traktarr` command.

1. `cd /opt`

2. `sudo git clone https://github.com/l3uddz/traktarr`

3. `sudo chown -R user:group traktarr` (run `id` to find your user / group)

4. `cd traktarr`

5. `sudo python3 -m pip install -r requirements.txt`

6. `sudo ln -s /opt/traktarr/traktarr.py /usr/local/bin/traktarr`

7. `traktarr` - run once to generate a sample a config.json file.

8. `nano config.json` - edit preferences.

## 2. Create a Trakt Application

1. Create a Trakt application by going [here](https://trakt.tv/oauth/applications/new)

2. Enter a name for your application; for example `traktarr`

3. Enter `urn:ietf:wg:oauth:2.0:oob` in the `Redirect uri` field.

4. Click "SAVE APP".

5. Open the traktarr configuration file `config.json` and insert the Client ID in the `client_id` and the Client Secret in the `client_secret`, like this:

   ```
       {
           "trakt": {
               "client_id": "my_client_id",
               "client_secret": "my_client_secret_key"
           }
       }
   ```

## 3. Authenticate User(s) (optional)

For each user you want to access the private lists for (i.e. watchlist and/or custom lists), you will need to to authenticate that user.

Repeat the following steps for every user you want to authenticate:

1. Run `traktarr trakt_authentication`

2. You wil get the following prompt:

   ```
   - We're talking to Trakt to get your verification code. Please wait a moment...
   - Go to: https://trakt.tv/activate on any device and enter A0XXXXXX. We'll be polling Trakt every 5 seconds for a reply
   ```
3. Go to https://trakt.tv/activate.

4. Enter the code you see in your terminal.

5. Click continue.

6. If you are not logged in to Trakt, login now.

7. Click "Accept".

8. You will get the message: "Woohoo! Your device is now connected and will automatically refresh in a few seconds.".

You've now authenticated the user.

You can repeat this process for as many users as you like.


# Configuration

## Sample Configuration

```json
{
  "core": {
    "debug": false
  },
  "automatic": {
    "movies": {
      "anticipated": 3,
      "boxoffice": 10,
      "interval": 24,
      "popular": 3,
      "trending": 2
    },
    "shows": {
      "anticipated": 10,
      "interval": 48,
      "popular": 1,
      "trending": 2
    }
  },
  "filters": {
    "movies": {
      "disabled_for": [],
      "allowed_countries": [
        "us",
        "gb",
        "ca"
      ],
      "allowed_languages": [],
      "blacklist_title_keywords": [
        "untitled",
        "barbie",
        "ufc"
      ],
      "blacklisted_genres": [
        "documentary",
        "music",
        "animation"
      ],
      "blacklisted_max_year": 2019,
      "blacklisted_min_runtime": 60,
      "blacklisted_min_year": 2000,
      "blacklisted_tmdb_ids": []
    },
    "shows": {
      "disabled_for": [],
      "allowed_countries": [
        "us",
        "gb",
        "ca"
      ],
      "allowed_languages": [],
      "blacklisted_genres": [
        "animation",
        "game-show",
        "talk-show",
        "home-and-garden",
        "children",
        "reality",
        "anime",
        "news",
        "documentary",
        "special-interest"
      ],
      "blacklisted_max_year": 2019,
      "blacklisted_min_runtime": 15,
      "blacklisted_min_year": 2000,
      "blacklisted_networks": [
        "twitch",
        "youtube",
        "nickelodeon",
        "hallmark",
        "reelzchannel",
        "disney",
        "cnn",
        "cbbc",
        "the movie network",
        "teletoon",
        "cartoon network",
        "espn",
        "yahoo!",
        "fox sports"
      ],
      "blacklisted_tvdb_ids": []
    }
  },
  "notifications": {
    "pushover": {
      "service": "pushover",
      "app_token": "",
      "user_token": "",
      "priority": 0
    },
    "slack": {
      "service": "slack",
      "webhook_url": ""
    },
    "verbose": true
  },
  "radarr": {
    "api_key": "",
    "profile": "HD-1080p",
    "root_folder": "/movies/",
    "url": "http://localhost:7878/"
  },
  "sonarr": {
    "api_key": "",
    "profile": "HD-1080p",
    "root_folder": "/tv/",
    "tags": {},
    "url": "http://localhost:8989/"
  },
  "trakt": {
    "client_id": "",
    "client_secret": ""
  }
}
```


## Core

```json
"core": {
  "debug": false
},
```

`debug` - show debug messages.
  - Default is `false` (keep it off unless your having issues).


## Automatic
Used for automatic / scheduled traktarr tasks.

Movies can be run on a separate schedule then from Shows.

_Note: These settings are only needed if you plan to use traktarr on a schedule (i.e. via manual/CLI command only); see [Usage](#usage)._

```json
"automatic": {
  "movies": {
    "anticipated": 3,
    "boxoffice": 10,
    "interval": 24,
    "popular": 3,
    "trending": 2,
    "watchlist": {},
    "lists": {}
  },
  "shows": {
    "anticipated": 10,
    "interval": 48,
    "popular": 1,
    "trending": 2,
    "watchlist": {},
    "lists": {}
  }
},
```

`interval` - specify how often (in hours) to run traktarr task.

`anticipated`, `popular`, `trending`, `boxoffice` (movies only) - specify how many items from each Trakt list to find.

`watchlist` - specify which watchlists to fetch (see explanation below)

`lists` - specify which custom lists to fetch (see explanation below)

### Personal Watchlists

The watchlist task can be scheduled with a differtent item limit for every (authenticated) user.


So for every user, you will add: `"username": limit` to the watchlist key. For example:

```json
"automatic": {
  "movies": {
    "watchlist": {
        "user1": 10,
        "user2": 5
    }
  },
  "shows": {
    "watchlist": {
        "user1": 2,
        "user3": 1
    }
  }
},
```

Of course you can combine this with running the other list types as well.

### Custom Lists

You can also schedule any number of public or private custom lists.

For both public and private lists you'll need the url to that list. When viewing the list on Trakt, simply copy the url from the address bar of the your browser.

#### Public Lists

Public lists can be added by specifying the url and the item limit like this:

```json
"automatic": {
  "movies": {
    "lists": {
        "https://trakt.tv/users/rkerwin/lists/top-100-movies": 10
    }
  },
  "shows": {
    "lists": {
        "https://trakt.tv/users/claireaa/lists/top-100-tv-shows-of-all-time-ign": 10
    }
  }
},
```


#### Private Lists

Private lists can be added in two ways:

1. If there is only one authenticated user, you can add the private list just like any other public list:

   ```json
   "automatic": {
     "movies": {
       "lists": {
           "https://trakt.tv/users/user/lists/my-private-movies-list": 10
       }
     },
     "shows": {
       "lists": {
           "https://trakt.tv/users/user/lists/my-private-shows-list": 10
       }
     }
   },
   ```

2. If there are multiple authenticated users you want to fetch the lists from, you'll need to specify the username under `authenticate_as`.

   _Note: The user should have access to the list (either own the list or a list that was shared to them by a friend)._

   ```json
   "automatic": {
     "movies": {
       "lists": {
           "https://trakt.tv/users/user/lists/my-private-movies-list": {
               "authenticate_as": "user2",
               "limit": 10
           }
       }
     },
     "shows": {
       "lists": {
           "https://trakt.tv/users/user/lists/my-private-shows-list": {
               "authenticate_as": "user2",
               "limit": 10
           }
       }
     }
   },
   ```


## Filters

Use filters to specify the movie/shows's country of origin or blacklist (i.e. filter-out) certain keywords, genres, years, runtime, or specific movies/shows.

### Movies

```json
  "movies": {
    "disabled_for": [],
    "allowed_countries": [
      "us",
      "gb",
      "ca"
    ],
    "allowed_languages": [],
    "blacklist_title_keywords": [
      "untitled",
      "barbie"
    ],
    "blacklisted_genres": [
      "documentary",
      "music",
      "animation"
    ],
    "blacklisted_max_year": 2019,
    "blacklisted_min_runtime": 60,
    "blacklisted_min_year": 2000,
    "blacklisted_tmdb_ids": []
  },
```

`disabled_for` - specify for which lists the blacklist must be disabled when running in automatic mode

Example:

```
    "disabled_for": [
        "anticipated",
        "watchlist:user1",
        "list:http://url-to-list"
    ],
```

`allowed_countries` - only add movies from these countries.

`allowed_languages` - only add movies with these languages (default/blank=English).

- By default, traktarr will only query shows in English. If you need to search for other languages (e.g. Japanese for anime), you must add those languages here.
- Languages are in [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) format (e.g. `ja` for Japanese.)

`blacklist_title_keywords` - blacklist certain words in titles.

`blacklisted_genres` - blacklist certain genres.

`blacklisted_max_year` - blacklist release dates after specified year.

`blacklisted_min_runtime` - blacklist runtime duration lower than specified time (in minutes).

`blacklisted_min_year` - blacklist release dates before specified year.

`blacklisted_tmdb_ids` - blacklist certain movies with their TMDB IDs.

### Shows

```json
"shows": {
  "allowed_countries": [
    "us",
    "gb",
    "ca"
  ],
  "allowed_languages": [],
  "blacklisted_genres": [
    "animation",
    "game-show",
    "talk-show",
    "home-and-garden",
    "children",
    "reality",
    "anime",
    "news",
    "documentary",
    "special-interest"
  ],
  "blacklisted_max_year": 2019,
  "blacklisted_min_runtime": 15,
  "blacklisted_min_year": 2000,
  "blacklisted_networks": [
    "twitch",
    "youtube",
    "nickelodeon",
    "hallmark",
    "reelzchannel",
    "disney",
    "cnn",
    "cbbc",
    "the movie network",
    "teletoon",
    "cartoon network",
    "espn",
    "yahoo!",
    "fox sports"
  ],
  "blacklisted_tvdb_ids": []
}
```

`disabled_for` - specify for which lists the blacklist must be disabled when running in automatic mode

Example:

```
    "disabled_for": [
        "anticipated",
        "watchlist:user1",
        "list:http://url-to-list"
    ],
```

`allowed_countries` - only add shows from these countries.

`allowed_languages` - only add shows with these languages (default/blank=English).

- By default, traktarr will only query shows in English. If you need to search for other languages (e.g. Japanese for anime), you must add those languages here.
- Languages are in [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) format (e.g. `ja` for Japanese.)

`blacklisted_genres` - blacklist certain genres.

`blacklisted_max_year` - blacklist release dates after specified year.

`blacklisted_min_runtime` - blacklist runtime duration lower than specified time (in minutes).

`blacklisted_min_year` - blacklist release dates before specified year.

`blacklisted_networks` - blacklist certain network.

`blacklisted_tvdb_ids` - blacklist certain shows with their TVDB IDs.


## Notifications

Notification alerts for traktarr tasks.

_Note: Manual commands need the `--notifications` flag._

Currently, only Pushover and Slack are supported. More will be added later.


```json
"notifications": {
  "pushover": {
    "service": "pushover",
    "app_token": "",
    "user_token": "",
    "priority": 0
  },
  "slack": {
    "service": "slack",
    "webhook_url": ""
  },
  "verbose": true
},
```

`verbose` - toggle detailed notifications.
  - Default is `true` (keep it off unless your having issues).


### Pushover

`app_token` and `user_token` - retrieve from Pushover.net.

You can specify a priority for the messages send via Pushover using the `priority` key. It can be any Pushover priority value (https://pushover.net/api#priority).

_Note: The key name (i.e the name right under notifications) can be anything, but the `"service":` must be exactly `"pushover"`._


### Slack

`webhook_url` - webhook URL you get after creating an "Incoming Webhook" under "Custom Integrations".

_Note: The key name (i.e the name right under notifications) can be anything, but the `"service":` must be exactly `"slack"`._


## Radarr

Radarr configuration.

```json
"radarr": {
  "api_key": "",
  "profile": "HD-1080p",
  "root_folder": "/movies/",
  "url": "http://localhost:7878"
},
```
`api_key` - Radarr's API Key.

`profile` - Profile that movies are assigned to.

`root_folder` - Root folder for movies.

`url` - Radarr's URL.


## Sonarr

Sonarr configuration.


```json
"sonarr": {
  "api_key": "",
  "profile": "HD-1080p",
  "root_folder": "/tv/",
  "tags": {},
  "url": "http://localhost:8989"
},
```

`api_key` - Sonarr's API Key.

`profile` - Profile that TV shows are assigned to.

`root_folder` - Root folder for TV shows.

`tags` - assign tags to shows based the network it airs on. More details on this below.

`url` - Sonarr's URL.


### Tags

The `tags` option allows Sonarr to assign tags to shows from specific television networks, so that Sonarr can filter in/out certain keywords from releases.

**Example:**

To show how tags work, we will create a tag `AMZN` and assign it to certain television networks that usually have AMZN releases.

1. First, we will create a tag in Sonarr (Settings > Indexers > Restrictions).

   ```
   Must contain: BluRay, Amazon, AMZN
   Must not contain:
   Tags: AMZN
   ```

2. And, finally, we will edit the traktarr config and assign the `AMZN` tag to some networks.

   ```json
   "tags": {
     "amzn": [
       "hbo",
       "amc",
       "usa network",
       "tnt",
       "starz",
       "the cw",
       "fx",
       "fox",
       "abc",
       "nbc",
       "cbs",
       "tbs",
       "amazon",
       "syfy",
       "cinemax",
       "bravo",
       "showtime",
       "paramount network"
     ]
   }
   ```

## Trakt

Trakt Authentication info:

```json
"trakt": {
  "client_id": "",
  "client_secret": ""
}
```

`client_id` - Fill in your Trakt API key (_Client ID_).

`client_secret` - Fill in your Trakt Secret key (_Client Scret_)


# Usage

## Automatic (Scheduled)

### Setup

To have traktarr get Movies and Shows for you automatically, on set interval, do the following:

1. `sudo cp /opt/traktarr/systemd/traktarr.service /etc/systemd/system/`

2. `sudo nano /etc/systemd/system/traktarr.service` and edit user/group to match yours.

3. `sudo systemctl daemon-reload`

4. `sudo systemctl enable traktarr.service`

5. `sudo systemctl start traktarr.service`

### Customize

You can customize how the scheduled traktarr is ran by editing the `traktarr.service` file and adding any of the following options:

```
  -d, --add-delay FLOAT  Seconds between each add request to Sonarr / Radarr.
                         [default: 2.5]
  --no-search            Disable search when adding to Sonarr / Radarr.
  --run-now              Do a first run immediately without waiting.
  --no-notifications     Disable notifications.
  --ignore-blacklist     Ignores the blacklist when running the command.
  --help                 Show this message and exit.
```

You can bring up the list, anytime, by running the following command:

```
traktarr run --help
```

\* Remember, other configuration options need to go into the `config.json` file under the `Automatic` section.

## Manual (CLI)

### General

```
traktarr
```

```
Usage: traktarr [OPTIONS] COMMAND [ARGS]...

  Add new shows & movies to Sonarr/Radarr from Trakt.

Options:
  --version       Show the version and exit.
  --config PATH   Configuration file  [default: /opt/traktarr/config.json]
  --logfile PATH  Log file  [default: /opt/traktarr/activity.log]
  --help          Show this message and exit.

Commands:
  movie                 Add a single movie to Radarr.
  movies                Add multiple movies to Radarr.
  run                   Run in automatic mode.
  show                  Add a single show to Sonarr.
  shows                 Add multiple shows to Sonarr.
  trakt_authentication  Authenticate traktarr.
  ```

### Movie (Single Movie)

```
traktarr movie --help
```

```
Usage: traktarr movie [OPTIONS]

  Add a single movie to Radarr.

Options:
  -id, --movie_id TEXT  Trakt movie_id.  [required]
  -f, --folder TEXT     Add movie with this root folder to Radarr.
  --no-search           Disable search when adding movie to Radarr.
  --help                Show this message and exit.
```

_Note: This command only works with `-id` or `--show_id` specified (i.e. not with lists), and supports both Trakt IDs and IMDB IDs._

### Movies (Multiple Movies)

```
traktarr movies --help
```


```
Usage: traktarr movies [OPTIONS]

  Add multiple movies to Radarr.

Options:
  -t, --list-type TEXT      Trakt list to process. For example, anticipated,
                            trending, popular, boxoffice, watchlist or any URL
                            to a list  [required]
  -l, --add-limit INTEGER   Limit number of movies added to Radarr.  [default:
                            0]
  -d, --add-delay FLOAT     Seconds between each add request to Radarr.
                            [default: 2.5]
  -g, --genre TEXT          Only add movies from this genre to Radarr.
  -f, --folder TEXT         Add movies with this root folder to Radarr.
  --no-search               Disable search when adding movies to Radarr.
  --notifications           Send notifications.
  --ignore-blacklist        Ignores the blacklist when running the command.
  --authenticate-user TEXT  Specify which user to authenticate with to
                            retrieve Trakt lists. Default: first user in the
                            config.
  --help                    Show this message and exit.
```

### Show (Single Show)

```
traktarr show --help
```


```
Usage: traktarr show [OPTIONS]

  Add a single show to Sonarr.

Options:
  -id, --show_id TEXT  Trakt show_id.  [required]
  -f, --folder TEXT    Add show with this root folder to Sonarr.
  --no-search          Disable search when adding show to Sonarr.
  --help               Show this message and exit.
```

_Note: This command only works with `-id` or `--show_id` specified (i.e. not with lists), and supports both Trakt IDs and IMDB IDs._


### Shows (Multiple Shows)

```
traktarr shows --help
```


```
Usage: traktarr shows [OPTIONS]

  Add multiple shows to Sonarr.

Options:
  -t, --list-type TEXT      Trakt list to process. For example, anticipated,
                            trending, popular, watchlist or any URL to a list
                            [required]
  -l, --add-limit INTEGER   Limit number of shows added to Sonarr.  [default:
                            0]
  -d, --add-delay FLOAT     Seconds between each add request to Sonarr.
                            [default: 2.5]
  -g, --genre TEXT          Only add shows from this genre to Sonarr.
  -f, --folder TEXT         Add shows with this root folder to Sonarr.
  --no-search               Disable search when adding shows to Sonarr.
  --notifications           Send notifications.
  --ignore-blacklist        Ignores the blacklist when running the command.
  --authenticate-user TEXT  Specify which user to authenticate with to
                            retrieve Trakt lists. Default: first user in the
                            config
  --help                    Show this message and exit.
```

## Examples (Manual)

- Add the movie "Black Panther (2018)":

  ```
  traktarr movie -id black-panther-2018
  ```

- Add the show "The 100":

  ```
  traktarr show -id the-100
  ```

- Add boxoffice movies, labeled with the comedy genre, limited to 10 items, and send notifications:

  ```
  traktarr movies -t boxoffice -g comedy -l 10 --notifications
  ```

- Add popular shows, limited to 2 items, and don't start the search in Sonarr:

  ```
  traktarr shows -t popular -l 2 --no-search
  ```

- Add all shows from the watchlist of `user1`:

  ```
  traktarr shows -t watchlist --authenticate-user user1
  ```

- Add all movies from the public list `https://trakt.tv/users/rkerwin/lists/top-100-movies`:

  ```
  traktarr movies -t https://trakt.tv/users/rkerwin/lists/top-100-movies
  ```

- Add all movies from the private list `https://trakt.tv/users/user1/lists/private-movies-list` of `user1`:

  ```
  traktarr movies -t https://trakt.tv/users/user1/lists/private-movies-list --authenticate-user=user1
  ```
