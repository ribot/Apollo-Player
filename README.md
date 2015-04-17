# Apollo Player

Opensource community driven playlist using Spotify, Mopidy, Node.js and Backbone.js


![alt text](https://s3.amazonaws.com/uploads.hipchat.com/24388/138929/xVFWu0fql1hJEkz/output_EfMMwh.gif "Apollo Demo")


![alt text](https://s3.amazonaws.com/uploads.hipchat.com/24388/138929/6pSihkoZfc3MthL/apollo_mobile.png "Apollo Demo Mobile")

## Features

- Simple and elegant group playlist
- Song Bombs =  3 (configurable) unique votes skips the track and "boos" the player
- Search keywords or Spotify URI's
- Support for multiple backends (merged from https://github.com/lemmy/Apollo-Player/)
- Play / Pause toggle
- Default Playlist (plays when Apollo runs out of songs to play) (optional)

## Get Started

### Prerequisites
1. Don't talk about Fight Club.
2. You'll need to have a basic understanding of Node and Node's package manager.

*I'll add that I didn't have a basic understanding of Node at all before this project and used Apollo to help me learn it... So there's hope for you too!*

### Install and configure Mopidy

First we'll need to install Mopidy. Mopidy is a opensource music server that you can connect to many "frontends" or roll your own, which what Apollo is. To read more about Mopidy go [here](http://www.mopidy.com/).

1. Install [Mopidy](http://docs.mopidy.com/en/latest/installation/). Mopidy is an opensource music server that installable on OSX, Debian/Ubuntu, Rasberry Pi. *note: so far we've only tested on Debian and OSX.
    1. You'll need to do the [Spotify installation](https://github.com/mopidy/mopidy-spotify)
    2. Next you'll need to [enable http on Mopidy](http://docs.mopidy.com/en/latest/ext/http/)
2. Start Mopidy on the commandline (if this doesn't work check the mopidy docs, probably a python path issue.)
    ```
    $ mopidy
    ```
3. Optional: You'll want to have Mopidy running in the background at all times for Apollo to work. You'll want to look into [making Mopidy a Daemon](http://www.benjaminguillet.com/blog/2013/08/16/launch-mopidy-at-login-on-os-x/) so that if it crashes it will auto-restart.

### Install Apollo

Now that you have Mopidy up and running, let's setup up Apollo as our Frontend for it.

1. Clone Apollo to a local folder
    ```
    $ git clone https://github.com/samcreate/Apollo-Player.git

    $ cd Apollo-Player
    ```
2. Install Node dependencies
    ```
    $ npm install
    ```
3. Configure Apollo
    1. Create a [Google Project](https://console.developers.google.com/)
        1. Create a new Google Project
        2. Go to the Credentials section under ApIs & Auth.
        3. Click create new Client ID
        4. Select Web Application
            1. Authorized Javascript origins: must match the origin for your requests
            2. Authorized Redirect URIS: a public facing url or private IP to which google redirect the user during the auth callback (must end in /auth/google/callback)
    2. Create a file called config.js in root directory of Apollo

      ```javascript
      $ touch config.js (or just create it with an editor)
      ```
    3. add this structure into config.js file:
        ```javascript
        module.exports = {
          server: {
            host: 'localhost',
            port: process.env.PORT || 3000,
          },
          mopidy: {
            host: 'localhost:6680'
          },
          googleAuth: {
            consumerKey: 'GOOGLE_CLIENT_ID',
            consumerSecret: 'GOOGLE_CLIENT_SECRET'
          },
          default_playlist_uri: "", // Optional. Spotify playlist URI to play when no songs are queued. Cannot be private
          htmlPretty: "true", // Optional. If the HTML should be outputted as pretty
          bombThreshold: 3, // Optional. How many bomb votes are needed to skip a song
          bomb_track: "spotify:track:1JFeNGtkTjiTWgSSz0iHq5" // Optional. Track to play when a song is skipped
        };
        ```

    4. Add your Spotify default playlist uri (this has to be a public playlist to work). This is what plays when all the songs in the queue have finished playing (optional)

3. Start Apollo
    ```javascript
    $ node apollo.js
    ```

Once Apollo is started, navigate to your localhost: http://localhost:3000

Optionally on Debian/Ubuntu, you might want to start Apollo as a system [service](https://gist.github.com/peterhost/715255).

Finished!

### Notes

- If you change a value in config.js, you'll need to restart node (ctrl+c)
- Apollo works best in Chrome.

### Tech used

- Frontend: Backbone, RequireJS, Bootstrap, Handelbars, Jade templating and stylus
- Backend: Node.js, ExpressJS

### Help? Bugs?

- [Ask questions here](https://groups.google.com/forum/#!forum/apolloplayer)
- [Report Bugs here](https://github.com/samcreate/Apollo-Player/issues)

### Credit

- Main background: [Not in the spotlight by Caroline Granycome](https://www.flickr.com/photos/cgranycome/10382878966/in/photolist-7Czew1-ayDLk8-rDdFsm-7cZxm3-6XYMyc-asZcbB-ohXABH-s8gD27-qBDdZq-aBFUrc-aASKb3-gPuYbA-8Pzjiq-6f3Zj-rJaepX-acWSUe-5apw6r-4ceaRR-eWb6v7-bhzxC6-9Uyxtw-Dwe2M-acVczK-rmGKtp-4P9JGL-fzi1My-5vbGRE-bZKSwq-fJ9t5d-552BQL-5b2bJz-4P9JJh-619xYr-9eJu6m-oRCYoA-acW4FG-9HHGYa-2xvyxD-7C6qWz-hsTi5E-aAuMe8-avnont-5buUZ2-5CY8o8-ccUma9-T4mg-fCfnhM-5PM6q-eCFAK5-8EtSuw) (CC BY-SA 2.0)
