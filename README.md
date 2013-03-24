# grunt-maven-tasks

> Grunt maven tasks - deploy and release articats to maven repository.

## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-maven-tasks --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-maven-tasks');
```

## The "maven" tasks

### deploy task

_Run this task with the `grunt maven:deploy` command._

This tasks packages and deploys the artifact to the maven repository.

### release task

_Run this task with the `grunt maven:release` command._

This tasks packages and releases the artifact to the maven repository. It will update the version number in the package.json file to the next development version, and, if this is a git project, it will commit and tag the release.  

By default, it will increment the version number using the `minor` version. This can be overridden in the config section using the `mode` option.

_Run this task with the `grunt maven:release:major` command to bump the next development version using the `major` version mode._

_Run this task with the `grunt maven:release:1.2.0` command to release version `1.2.0`._

_Run this task with the `grunt maven:release:1.2.0:major` command to release version 1.2.0 and bump the next development version using the `major` version mode._

### Overview
In your project's Gruntfile, add a section named `maven` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  maven: {
    options: {
      groupId: 'com.example'
    },
    deploy: {
      options: { url: '<snapshot-repository-url>' },
      files: [ { src: [ '**', '!node_modules/**' ] } ]
    },
    release: {
      options: { url: '<release-repository-url>' },
      files: [ { src: [ '**', '!node_modules/**' ] } ]
    },
  },
})
```

### Options

#### options.groupId
Type: `String`
Required

The maven group id to use when deploying and artifact

#### options.artifactId
Type: `String`
Default: name found in package.json

The maven artifact id to use when deploying and artifact

#### options.version
Type: `String`
Default: version found in package.json

The version to use when deploying to the maven repository

#### options.mode
Type: `String`
Default: minor

The mode passed to semver.inc to determine next development version.

#### options.packaging
Type: `String`
Default: zip

The packaging to use when deploying to the maven repository. Will also
determine the archiving type. As internally the grunt-contrib-compress
plugin is used to package the artifact, only archiving types supported
by this module is supported.

#### options.url
Type: `String`
Required

The url for the maven repository to deploy to.

#### options.repositoryId
Type: `String`
Optional

The repository id of the repository to deploy to. Used for looking up authentication in settings.xml.

### Usage Examples

#### Default Options
In this example, only required options have been specified.

Running `grunt maven:deploy` will deploy the artifact to the `snapshot-repos` folder using the groupId `com.example`, the artifactId set to the name in `package.json` and the version set to the version in `package.json`.

Running `grunt maven:release` will deploy the artifact to the `release-repo` folder using the groupId `com.example`, the artifactId set to the name in `package.json` and the version set to the version in `package.json`, but with the `-SNAPSHOT` suffix removed. The version in `package.json` will be incremented to the next minor SNAPSHOT version, ie. if it was `1.0.0-SNAPSHOT` it will end up at `1.1.0-SNAPSHOT`. If this is a git repository, it will also commit and tag the release version, as well as commiting the updated package.json version.

```js
grunt.initConfig({
  maven: {
    options: { groupId: 'com.example' },
    deploy: {
      options: { url: 'file://snapshot-repo' },
      files: [ { src: [ '**', '!node_modules/**' ] } ]
    },
    release: {
      options: { url: 'file://release-repo' },
      files: [ { src: [ '**', '!node_modules/**' ] } ]
    }
  }
})

grunt.registerTask('deploy', [ 'clean', 'test', 'maven:deploy' ]);
grunt.registerTask('release', [ 'clean', 'test', 'maven:release' ]);
```

#### Custom Options
In this example, the artifactId has been explicitly set, and the version bumping used when releasing is set to `'patch'` level rather than the default `'minor'`.

```js
grunt.initConfig({
  maven: {
    options: { groupId: 'com.example', artifactId: 'example-project' },
    deploy: {
      options: { url: 'file://snapshot-repo' },
      files: [ { src: [ '**', '!node_modules/**' ] } ]
    },
    release: {
      options: { url: 'file://release-repo', mode: 'patch' },
      files: [ { src: [ '**', '!node_modules/**' ] } ]
    }
  }
})
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History
_(Nothing yet)_