## Heroku Buildpack for Running Grunt and Bower

Run Grunt and Bower on Heroku during slug compilation.

## Why use this?

Do you...

* Run a Web application on Heroku and bundle a JavaScript framework such as AngularJS ?
* Have the JS framework placed in a sub-directory because you're a OCD neat freak?
* Want to be able to compile static resources during the slug compilation rather than commit into Git?

Then this might be what you're looking for.

## Getting Started - the TL;DR version

You will generally want to use this in conjunction with another build pack, if you are running a Ruby/Scala/etc. Web process. To use this with the standard build pack, you will need to use @ddollar's Multi-build pack:

      heroku config:add BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git
      echo 'public' > .node

vi .buildpacks:
      
      https://github.com/heroku/heroku-buildpack-ruby.git
      https://github.com/mefellows/heroku-buildpack-nodejs-grunt

vi public/Gruntfile.js:

    grunt.registerTask('heroku', [
        'clean:dist',
        'bowerInstall',
        'useminPrepare',
        'concurrent:dist',
        'autoprefixer',
        'concat',
        'ngmin',
        'copy:dist',
        'cdnify',
        'cssmin',
        'uglify',
        'rev',
        'usemin',
        'htmlmin',
        'copy:ruby'
    ]);

## Ruby Build Pack example

See https://github.com/mefellows/sinatra-angular-seed for a real-life example, the basis for which forms this guide.

Let's assume the following tree structure:

      .
      ├── Gemfile
      ├── Gemfile.lock
      ├── Procfile
      ├── README.md
      ├── Rakefile
      ├── app
      │   ├── routes
      │   └── views
      ├── app.rb
      ├── bin
      │   └── magic
      ├── config.ru
      ├── lib
      │   └── sitemap
      ├── public
      │   ├── Gruntfile.js
      │   ├── app
      │   ├── bower.json
      │   ├── dist
      │   ├── ...
      │   ├── node_modules
      │   ├── package.json
      │   └── test
      ├── magic.gemspec
      └── spec
          ├── magic_spec.rb

Note that Angular is placed in the public directory, where the ```Gruntfile.js``` and ```bower.json``` files live. We need to tell the build pack where to find this.

### Specify .node

You then need to specify the base directory for Grunt to execute in, which you can do in two ways - via a ```.node``` file or the ```NODE_BUILDPACK_DIR``` Environment variable.

    echo 'public' > .node

or

    heroku config:set NODE_BUILDPACK_DIR='public'

### Add a .buildpacks file

Now we need to tell Heroku which buildpacks to use during slug compilation. As ours is a Ruby app, we'll add the default Heroku Ruby build pack and our new Node buildpack:

vi .buildpacks  
  
      https://github.com/heroku/heroku-buildpack-ruby.git
      https://github.com/mefellows/heroku-buildpack-nodejs-grunt


### Add a 'heroku' build task to your grunt file

Lastly, this buildpack will run the 'heroku' build task (in case you need to do anything specific on Heroku). Typically, this will be your 'build' task. Register a new task as follows:

    grunt.registerTask('heroku', [
        'clean:dist',
        'bowerInstall',
        'useminPrepare',
        'concurrent:dist',
        'autoprefixer',
        'concat',
        'ngmin',
        'copy:dist',
        'cdnify',
        'cssmin',
        'uglify',
        'rev',
        'usemin',
        'htmlmin',
        'copy:ruby'
    ]);

### Finish

    git push heroku master

## Acknowledgements

This is a fork of https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt/, forked in turn from the official NodeJS build pack https://github.com/heroku/heroku-buildpack-nodejs/
