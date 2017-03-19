# serverless-gulp
A simple wrapper around [serverless 1.x](https://serverless.com) that allows you to create tasks using gulp. If you like having all your tasks for local and CI in one place eg running tests, environment specific stuff, and deployment of lambdas, etc and love the simplicity that gulp brings, then this module may help you.
Some of the example spawn a process with options as arguments to serverless. The approach used by this module uses serverless framework as a node module and provides options to it via your gulpfile.

To get started, npm install this module as a dev dependency. i.e.

```javascript
npm install --save-dev serverless-gulp
```

This module has a dependency on the following modules:

* serverless 1.x
* gulp
* gulp-util for logging

Once installed, unless you need other gulp modules, you should not really need any other dependencies. Start developing your services as you would.
To get started, use the code below as your initial *gulpfile.js*. The idea of this module is to keep things simple, so regardless of how the *serverless framework* evolves, this module will allow you to specify any command and option to it within a gulp task.

```javascript
const gulp = require('gulp');
const serverlessGulp = require('serverless-gulp')
const util = require('gulp-util');

const paths = {
  serverless: ['./**/serverless.yml', '!node_modules/**/serverless.yml']
}

gulp.task('deploy', () => {
  gulp.src(paths.serverless)
      .pipe(serverlessGulp('deploy', { stage: 'dev' }))
})

gulp.task('remove', () => {
  gulp.src(paths.serverless)
    .pipe(serverlessGulp('remove', { stage: 'dev' }))
})
```

The first argument to serverless-gulp is the command you would pass to *serverless framework*, the second takes options. So, for the following command line:

```bash
serverless invoke --function someFunction --stage en --region eu-west-2
```

your gulp task would look something like:

```javascript
gulp.task('invoke', () => {
  gulp.src(paths.serverless)
    .pipe(serverlessGulp('invoke', { function: 'someFunction', stage: 'en', region: 'eu-west-1' }))
})
```
