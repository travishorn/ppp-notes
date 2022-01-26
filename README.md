# Plutus Pioneer Program Notes

Notes from the Plutus Pioneer Program 3rd cohort

See the published notes at https://travishorn.github.io/ppp-notes/

## Development

To run these notes locally, clone this repository

```bash
git clone https://github.com/travishorn/ppp-notes
```

Change into the repo directory

```bash
cd ppp-notes
```

Install dependencies

```bash
npm install
```

Run the dev server

```bash
npm run dev
```

The site is served at http://localhost:5000/ppp-notes

Modify markdown files in the repository. The site will automatically be
hot-reloaded.

## Deployment

Modify `retype.yml` and set the `url` to the URL where the site will be served.

### Using GitHub Pages

Pushing this repository to GitHub will automatically invoke a GitHub Action
which will create a new branch called `retype`.

Visit your repository on GitHub. Go to **Settings** > **Pages**. Under
**Source**, choose the `retype` branch and click **Save**.

Your site will be live at https://[username].github.io/[repo name] shortly.

### Using other static hosting

Build the site

```bash
npm run build
```

Built files are created in a directory called `.retype`. You can publish those
files to a static web server.

## License

The MIT License (MIT)

Copyright © 2022 Travis Horn

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the “Software”), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
