---
id: installation-linux
title: Installation for Linux
---

:::note
If you prefer using containers or have problems with configuring PostgreSQL, Redis, and Elasticsearch, see our [Using Docker](getting-started/docker.md) instructions.
:::

## Prerequisites

Before you are ready to run Saleor, you will need additional software installed on your computer.

### Python 3

Saleor requires Python version 3.7 or later. A compatible version comes pre-installed with most current Linux systems. If not, consult your Linux distributor for instructions on how to install Python 3.7 or 3.8.

:::note
If you get error `ModuleNotFoundError: No module named '_cffi_backend'` install cffi: 
`pip install cffi`

If you have trouble compiling uswgi check that you have the dev tools for your version of python: `apt-get install python3.x-dev`.
:::

### Node.js

Version 10 or later is required. See the [installation instructions](https://nodejs.org/en/download/package-manager/).

### PostgreSQL

Saleor needs PostgreSQL version 9.4 or above to work. Use the [PostgreSQL download page](https://www.postgresql.org/download/) to get instructions for your distribution.

### GTK+

Some features (for example, PDF creation) require that additional system libraries are present.

- Debian / Ubuntu

Debian 9.0 Stretch or newer, Ubuntu 16.04 Xenial or newer:

```shell-session
$ sudo apt-get install build-essential python3-dev python3-pip python3-cffi libcairo2 libpango-1.0-0 libpangocairo-1.0-0 libgdk-pixbuf2.0-0 libffi-dev shared-mime-info
```

- Fedora

```shell-session
$ sudo yum install redhat-rpm-config python-devel python-pip python-cffi libffi-devel cairo pango gdk-pixbuf2
```

- Arch Linux

```shell-session
$ sudo pacman -S python-pip cairo pango gdk-pixbuf2 libffi pkg-config
```

- Gentoo

```shell-session
$ emerge pip cairo pango gdk-pixbuf cffi
```

## Setup

### Create a PostgreSQL user

See PostgreSQL’s [createuser](https://www.postgresql.org/docs/current/app-createuser.html) command for details.

Unless configured otherwise, the store will use saleor as both the username and password. Remember to assign your user the SUPERUSER privilege. This will allow you to create databases and database extensions.

:::warning
While creating the database, Django will need to create some PostgreSQL extensions if they are not already present. This requires superuser privileges.

For local development you can grant your database user the `SUPERUSER` privilege. For publicly available systems, we recommend using a separate privileged user to perform database migrations.
:::

### Create a PostgreSQL database

See [PostgreSQL’s createdb command](https://www.postgresql.org/docs/current/static/app-createdb.html) for details.

:::note
The database name is extracted from the `DATABASE_URL` environment variable. If absent, it defaults to `saleor`.
:::

## Installation - Saleor Core (API)

Once you have installed or updated the pre-requisite software, you are ready to setup and install Saleor. Follow the steps below to start and execute the process:

### 1. Clone the repository (or use your own fork)

```shell-session
$ git clone https://github.com/mirumee/saleor.git
```

### 2. Enter into the directory

```shell-session
$ cd saleor/
```

### 3. Install all dependencies

We strongly recommend [creating a virtual environment](https://docs.python.org/3/tutorial/venv.html) before installing any Python packages.

```shell-session
$ pip install -r requirements.txt
```

### 4. Set `SECRET_KEY` environment variable

We try to provide usable default values for all the settings. We have decided not to provide a default for `SECRET_KEY` as we fear someone would inevitably ship a project with the default value left in code.

```shell-session
$ export SECRET_KEY='<mysecretkey>'
```

:::warning
The secret key should be a unique string that only your team knows. Running the code with a known `SECRET_KEY` defeats many of Django’s security protections; it can also lead to privilege escalation and remote code execution vulnerabilities. Consult [Django’s documentation](https://docs.djangoproject.com/en/1.11/ref/settings/#secret-key) for details.
:::

### 5. Prepare the database

```shell-session
$ python manage.py migrate
$ python manage.py collectstatic --no-input
$ python manage.py populatedb --createsuperuser
```

:::note
The `--createsuperuser` argument creates an admin account for `admin@example.com` with the password set to `admin`.
:::

:::warning
This command creates database extensions. If you get an error related to the `CREATE EXTENSION` command, please return to the instructions in the PostgreSQL user creation step.
:::

### 6. Install front-end dependencies

```shell-session
$ npm install
```

:::note
If this step fails, make sure you are using a recent version of Node.js.
:::

### 7. Compile e-mails

```shell-session
$ npm run build-emails
```

### 8. Start the development server:

```shell-session
$ python manage.py runserver
```

## Installation - Saleor Dashboard

### 1. Clone the repository (or use your own fork)

```shell-session
$ git clone https://github.com/mirumee/saleor-dashboard.git
```

### 2. Enter into the directory

```shell-session
$ cd saleor-dashboard/
```

### 3. Checkout to either stable or development version

- Stable

```shell-session
$ git checkout v2.0.0
```

- Development

```shell-session
$ git checkout master
```

See the list of all dashboard releases [here](https://github.com/mirumee/saleor-dashboard/releases/)

### 4. Install front-end dependencies

```shell-session
$ npm install
```

### 5. Set configuration options

- API_URI (required) - URI of a running instance of Saleor GraphQL API. If you are running Saleor locally with the default settings, set `API_URI` to: `http://localhost:8000/graphql/`.

```shell-session
$ export API_URI="http://localhost:8000/graphql/"
```

- APP_MOUNT_URI - URI at which the Dashboard app will be mounted. E.g. if you set `APP_MOUNT_URI` to `/dashboard/`, your app will be mounted at `http://localhost:9000/dashboard/`.

```shell-session
$ export APP_MOUNT_URI="/dashboard/"
```

- STATIC_URL - URL where the static files are located. E.g. if you use S3 bucket, you should set it to the bucket's URL. By default Saleor assumes you serve static files from the root of your site at `http://localhost:9000/`.

```shell-session
$ export STATIC_URL="http://localhost:9000/"
```

### 6. Start the dashboard

- Development

```shell-session
$ npm start
```

- Production

```shell-session
$ npm run build
```

## Installation - Saleor Storefront

### 1. Clone the repository (or use your own fork)

```shell-session
$ git clone https://github.com/mirumee/saleor-storefront.git
```

### 2. Enter into the directory

```shell-session
$ cd saleor-storefront/
```

### 3. Checkout to either stable or development version

- Stable

```shell-session
$ git checkout v0.7.0
```

- Development

```shell-session
$ git checkout master
```

See the list of all storefront releases [here](https://github.com/mirumee/saleor-storefront/releases)

### 4. Install front-end dependencies

```shell-session
$ npm install
```

### 5. Start the storefront

- Development

```shell-session
$ npm start
```

- Production

```shell-session
$ npm run build
```
