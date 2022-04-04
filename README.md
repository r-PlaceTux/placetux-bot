# Reddit Place Script 2022

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://forthebadge.com)

## About

This is a script to draw the offical TUX onto r/place (<https://www.reddit.com/r/place/>).

## Features

- Support for multiple accounts
- Determines the cooldown time remaining for each account
- Detects existing matching pixels on the r/place map and skips them
- Automatically converts colors to the r/place color palette
- Easy(ish) to read output with colors
- SOCKS proxy support
- No client id and secret needed
- Proxies from "proxies.txt" file
- Tor support

## Requirements

- [Latest Version of Python 3](https://www.python.org/downloads/)

## MacOSX
If you are using MacOSX and encounter an SSL_CERTIFICATE error. Please apply the fix detailed https://stackoverflow.com/questions/42098126/mac-osx-python-ssl-sslerror-ssl-certificate-verify-failed-certificate-verify  

If you want to use tor on MacOSX. you'll need to provide your own tor binary and start it manually. deactivate the "use_builtin tor"
option in the config and make sure you configure your tor to use the specified ports and password. 
<br>*note that socks proxy connection to tor doesn't work for the time being, so the config value is for an httpTunnel port*

## Get Started

Move the file 'config_example.json' to config.json

Edit the values to replace with actual credentials and values

Note: Please use https://jsonlint.com/ to check that your JSON file is correctly formatted

```json
{
  //The hash of the latest update
  "image_hash_url": "https://r-placetux.github.io/place_tux/bot_hash",
  // delay between starting threads (can be 0)
  "thread_delay": 10,
  //the images
  "images": {
    //add here images for the bots to use
    "prio":"https://r-placetux.github.io/place_tux/priority"
  },

  // array of accounts to use
  "workers": {
    // username of account 1
    "worker1username": {
      // password of account 1
      "password": "password",
      // which pixel of the image to draw first
      "start_coords": [0, 0],
      //the image this bot will build
      "image": "prio"
    },
    // username of account 2
    "worker1username": {
      // password of account 2
      "password": "password",
      // which pixel of the image to draw first
      "start_coords": [0, 0]
      //the image this bot will build
      "image": "prio"
    }
    // etc... add as many accounts as you want (but reddit may detect you the more you add)
  }
}
```

### Notes

-  Multiple fields can be passed into the arrays to spawn a thread for each one.
- If you use 2 factor authentication (2FA) in your account, then change "password" to "password:XXXXXX" where XXXXXX is your 2FA code.
- Images are configurable but require a "positions.json" in the same directory as the image_url

## Run the Script

### Windows

```shell
start.bat or startverbose.bat
```

### Unix-like (Linux, MacOS etc.)

```shell
chmod +x start.sh startverbose.sh
./start.sh or ./startverbose.sh
```

### You can get more logs (`DEBUG`) by running the script with `-d` flag

`python3 main.py -d` or `python3 main.py --debug`


## Configure the Image(s)

The bot supports all the art from [The Offical Art Repo](https://github.com/r-PlaceTux/place_tux/tree/main/artwork)! It also supports specieving for each worker a dedicated image to work on. Add images to the config.json as follows:
```json
    "images": {
        "tux":"https://r-placetux.github.io/place_tux/artwork/tux/",
        "<image_name>":"https://r-placetux.github.io/place_tux/artwork/<dir>/",
        //this is the priority image 
        "prio":"https://r-placetux.github.io/place_tux/priority"
    }
```
The image name and directory can be found in  [The Offical Art Repo](https://github.com/r-PlaceTux/place_tux/tree/main/artwork) 

## Multiple Workers

Just create multiple child arrays to "workers" in the .json

```json
{
  "image_hash_url": "https://r-placetux.github.io/place_tux/bot_hash",
  "thread_delay": 2,

  "workers": {
    "worker1username": {
      "password": "password",
      "start_coords": [0, 0],
      "image": "image"
    },
    "worker2username": {
      "password": "password",
      "start_coords": [0, 50],
      "image": "image"
    }
  }
}
```

In this case, the first worker will start drawing from (0, 0) and the second worker will start drawing from (0, 50) from the input image.jpg file.

This is useful if you want different threads drawing different parts of the image with different accounts.

## Other Settings

If any JSON decoders errors are found, the `config.json` needs a fix. Make sure to add the below 2 lines in the file.

```json
{
    "thread_delay": 2,
    "unverified_place_frequency": false,
    "proxies": ["socks5://<username>:<password>@<ip>:<port>","2.2.2.2:1234"],
    "compact_logging": true
}
```

- thread_delay - Adds a delay between starting a new thread. Can be used to avoid ratelimiting
- unverified_place_frequency - Sets the pixel place frequency to the unverified account limit
- proxies - Sets proxies to use for sending requests to reddit. The proxy used is randomly selected for each request. Can be used to avoid ratelimiting
- compact_logging - Disables timer text until next pixel

- Transparency can be achieved by using the RGB value (69, 42, 0) in any part of your image
- If you'd like, you can enable Verbose Mode by adding --verbose to "python main.py". This will output a lot more information, and not neccessarily in the right order, but it is useful for development and debugging.
- You can also setup proxies by creating a "proxies" and have a new line for each proxie

## Docker

A dockerfile is provided. Instructions on installing docker are outside the scope of this guide.

To build: After editing your config.json, run `docker build . -t place-bot`. and wait for the image to build

You can now run with 

`docker run place-bot`


## Developing
The nox CI job will run flake8 on the code. You can also do this locally by pip installing nox on your system and running 
`nox` in the repository directory.

## Contributing

See the [Contributing Guide](docs/CONTRIBUTING.md)
