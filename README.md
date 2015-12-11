# Fundtroversy

## Getting Started

### First Run

- Copy `config.default.js` to `config.js`, and fill it out with the appropriate details.
- Do you actually have nodejs? Ubuntu will lie to you. Be sure: `sudo apt-get --purge remove node` and
	install nodejs with `sudo apt-get install nodejs`.
- While you're at it, go get Node Version Manager, too. Fundtroversy will sit there and blink at you if you use any nodejs below v0.10.40, <a href="https://www.digitalocean.com/community/tutorials/how-to-install-node-js-with-nvm-node-version-manager-on-a-vps">so install nvm if you don't have it already and set it to the appropriate version number</a>
- You may need Node Package Manager if you don't already have it `sudo apt-get install npm`.
- Install the dependencies in /fundtroversy/ using `npm install`.
- Install `grunt-cli` globally using `npm install grunt-cli --global`. You may need to `sudo` that.
- Because of the retarded way Ubuntu handles node, you'll have to symlink if it gives you this message: `/usr/bin/env: node: No such file or directory`. Command you'll need is: `ln -s /usr/bin/nodejs /usr/bin/node`
- Build all the client-side JS and CSS files using `grunt`.
- (Optional) Build the scrypt module by changing into the `scrypt` directory and running `make node`.

### Running

- First of all, there's `node server.js`. It's your basic run command.
- If you want to log the output to a file, you can add `>> /tmp/file.log`.
- Putting `&` at the end of the command runs it in the background, so it won't be killed when you close your terminal window or SSH connection. Personally, I just run the whole thing in `screen` and Ctrl-a, Ctrl-d to leave it running.
- If you want to pass in options to the application, you put them at the beginning in the form of `NODE_PORT=9870`.

When you put them all together, you get what I use in my start script:

`NODE_PORT=9870 NODE_ENV=production node server.js >> /tmp/server.log &`

- If you don't already have `mod_rewrite` and `mod_proxy` enabled, you'll need to do that.
- `a2enmod rewrite`
- `a2enmod proxy`
- `service apache2 restart`

- The previous implementation had an index page that handled communicating information to the node server. Since this will no longer be the case, changes to .htaccess will be in order:

    `RewriteEngine On`
    
    `RewriteRule ^public/(.*)$ http://localhost:9870/public/$1 [P,L]`
    
    `RewriteRule ^api/(.*)$ http://localhost:9870/api/$1 [P,L]`
    

- Drag /demo to /var/www/html and type in the appropriate address in your browser to check to see if it's running properly. Modify index.html to your liking, or, better yet, take the appropriate code from index.html and input it into your existing page.


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


### Version 1.0 forked

- Redis removed, payer.js removed and no longer functional
- idigdoge page resources gutted
- Demo page added, notated where to copy in order to paste into existing pages, foregoing hashrate and button at site owner's option

=======

