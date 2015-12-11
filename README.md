# I Dig Doge

## Getting Started

### First Run

- Copy `config.default.js` to `config.js`, and fill it out with the appropriate details.
- Install the dependencies using `npm install`.
- Install `grunt-cli` globally using `npm install grunt-cli --global`. You may need to `sudo` that.
- Build all the client-side JS and CSS files using `grunt`.
- (Optional) Build the scrypt module by changing into the `scrypt` directory and running `make node`.

### Running

- First of all, there's `node server.js`. It's your basic run command.
- If you want to log the output to a file, you can add `>> /tmp/file.log`.
- Putting `&` at the end of the command runs it in the background, so it won't be killed when you close your terminal window or SSH connection.
- If you want to pass in options to the application, you put them at the beginning in the form of `NODE_PORT=9870`.

When you put them all together, you get what I use in my start script:

`NODE_PORT=9870 NODE_ENV=production node server.js >> /tmp/server.log &`

#### server.js

`server.js` uses local filesystem instead of Redis to hold workitems info, sent to workers.

##### Options

- `NODE_ENV` modifies settings specified in [Environments](#environments).
- `NODE_PORT` or `PORT` controls the port for HTTP requests. It will overwrite the setting in `config.js`. HTTPS is not yet supported.

### Environments

Express checks the `NODE_ENV` environment variable to know what features to turn on. I've configured two:

- `development` Logs requests to the console, serves `demo/index.html` at the site root.
- `production` Turns on `trust proxy`. Does not log requests.

## Changes You Might Care About

### Version 2.0.2

- Use `PORT` or `NODE_PORT` environment variables.
- The default config file is now separate.
- There's options for which scrypt implementation you use.

### Version 2.0.1

- Minor changes for HTTPS.
- Uses another scrypt implementation on the server for easier install.
- Checks Dogecoin addresses properly, by decoding and validating the checksum.

### Version 2.0.0

- Brand new look.
- Using Grunt for minifying and combining front-end files.
- All the scrypt implementations use the same C++ code base.
- In Chrome, we now use [PNACL](https://developers.google.com/native-client/dev/) instead of JavaScript.
- Switched to SASS for CSS.
- The giant JS file has been split up into manageable chunks.