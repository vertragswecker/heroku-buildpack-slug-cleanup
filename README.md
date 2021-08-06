# Slug cleanup

This is a tiny buildpack to run after all other buildpacks have completed their jobs. It will remove files you don't want to be included in the compiled slug, keeping it small to fit in the [maximum allowed slug size of 500 MB](https://devcenter.heroku.com/articles/slug-compiler#slug-size), and have faster dyno scaling.

Do not confuse it with the functionality of [.slugignore](https://devcenter.heroku.com/articles/slug-compiler#ignoring-files-with-slugignore), which causes files to be removed after you push code to Heroku and before the buildpack runs. The slugcleanup build pack is intended to remove files and build dependencies necessary for the build but not required at runtime.

## Usage

Add the buildpack to your application:

```bash
heroku buildpacks:add volders/slugcleanup
```

Make sure to keep this buildpack as the last one since you might want to remove things introduced by previous buildpacks.

Then, add a `.slugcleanup` file in the root of your repository listing the files and directories (it supports globs) that you want to remove after the app build and before the slug compiling. For example:

```
app/assets
node_modules
bin/node
vendor/yarn-*
```

In the next deployment, you should see a way smaller slug size in the build log:

```
...
-----> Compressing...
       Done: 30.7M
```

## License

Copyright 2021 Volders GmbH

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
