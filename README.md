# Scaife Digital Library Viewer

The Scaife Viewer is the new reading environment for the Perseus Digital Library.

See [Ways to Contribute](https://github.com/scaife-viewer/scaife-viewer/wiki/Ways-to-Contribute).

## Getting Started

Requirements:

* Python 3.7.1
  * pipenv
* Node 10.7
* PostgreSQL 9.6
* Elastic Search

First, set up a database to use for local development:

    createdb scaife-viewer

This assumes your local PostgreSQL is configured to allow your user to create databases. If this is not the case you might be able to create the user yourself:

    createuser --username=postgres --superuser $(whoami)

Install the Node and Python dependencies:

    npm install
    pipenv install --dev

To run commands in the Python environment, you can use `pipenv shell` and carry on, but I find it has some nasty side-affects. So, to activate the pipenv environment in your current shell:

    source "$(pipenv --venv)/bin/activate"

Setup the database:

    python manage.py migrate
    python manage.py loaddata sites

Seed the text inventory to speed up local development:

    curl -s "https://scaife-cts-dev.perseus.org/api/cts?request=GetCapabilities" > ti.xml

You should now be set to run the static build pipeline and hot module reloading:

    npm start

In another terminal, start runserver:

    pipenv run python manage.py runserver

Browse to http://localhost:8000/.

Note that, although running Scaife locally, this is relying on the Nautilus server at https://scaife-cts-dev.perseus.org to retrieve texts.


## Translations

Before you work with translations, you will need gettext installed.

macOS:

    brew install gettext
    export PATH="$PATH:$(brew --prefix gettext)/bin"

To prepare messages:

    python manage.py makemessages --all

If you need to add a language; add it to `LANGUAGES` in settings.py and run:

    python manage.py makemessages --locale <lang>
