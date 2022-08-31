# A UI for ansible-events

## Setting up a development environment

### 1. Install taskfile

[Taskfile](https://taskfile.dev/) is a task runner that aims to be simpler and easier
replacement for a GNU Make.

Install taskfile following the [installation guide](https://taskfile.dev/installation/).

### 2. Clone the repository

First you need to clone the `ansible-events-ui` repository:

```shell
git clone https://github.com/benthomasson/ansible-events-ui.git
cd ansible-events-ui
```

### 3. Virtual environment

Create virtual environment and install project

```shell
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

Install Ansible and `benthomasson.eda` collection:

```shell
pip install ansible
ansible-galaxy collection install benthomasson.eda
```

### 4. Services

You need to set up a PostgreSQL database sever. The easiest way is using the `docker-compose`:

```shell
docker-compose -p ansible-events -f tools/docker/docker-compose.yml up -d postgres
```

Then run database migrations:

```shell
alembic upgrade head
```

### 5. User interface

Build UI files (requires Node >= v16):

```shell
cd ui
npm install
npm run build
cd ..
```

### 6. Start server

```shell
ansible-events-ui
```

Visit this url: <http://localhost:8080/docs#/auth/register_register_api_auth_register_post>

Click "Try it out" on `/api/auth/register`

Change email and password

Click execute

Visit this url: <http://localhost:8080/eda>

Also you can check the [openapi specification.](http://localhost:8080/docs)

You have set up the development environment.

## Run the application with docker-compose

Requires docker-compose installed. [See the documentation](https://docs.docker.com/compose/install/) for instructions. (latest stable version is recommended)

```sh
cd tools/docker
docker-compose up --build
```

## Running tests

If not started, start the PostgreSQL service, which is required for running integration tests.

```shell
docker-compose -f tools/docker/docker-compose.yml up postgres
```

Run all tests:

```shell
task test
```

Or call `pytest` directly:

```shell
python -m pytest 
```
