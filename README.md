## Heroku Buildpack for Running Grunt and Bower

Run Grunt and Bower on Heroku during slug compilation.

## Why use this?

Do you...

* Run a Web application on Heroku and bundle a JavaScript framework such as AngularJS ?
* Have the JS framework placed in a sub-directory because you're a OCD neat freak?
* Want to be able to compile static resources during the slug compilation rather than commit into Git?

Then this might be what you're looking for.

## Getting Started

You will generally want to use this in conjunction with another build pack, if you are running a Ruby/Scala/etc. Web process. To use this with the standard build pack, you will need to use @ddollar's Multi-build pack:

   ```heroku config:add BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git```

### Ruby Build Pack example

vi .buildpacks  
  
    https://github.com/heroku/heroku-buildpack-ruby.git
    https://github.com/mefellows/heroku-buildpack-nodejs-grunt
   
See https://github.com/mefellows/sinatra-angular-seed for a real-life example.

## Acknowledgements

This is a fork of https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt/, forked in turn from the official NodeJS build pack https://github.com/heroku/heroku-buildpack-nodejs/
