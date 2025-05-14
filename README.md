# Git-Basics-Repository
This repository is meant for small illustration or training purposes.

Follow a good introduction to git by following the [git-scm tutorial.](https://git-scm.com/docs/gittutorial)

This repository targets to provide an easy start into Git, also for no-coders.

## This repository

The repository mainly consists of 

- a sample application with unit test
- a sample workflow to run the test via a github workflow
- instructions for starters of git and github workflows (e.g. `act`)

### Spring Application

This spring application is an assembly of different projects (see [NOTICE.md](./NOTICE.md)).

It's a basic API key implementation in spring boot, that is not the preferred way for production (we highly recommend e.g., an Identity Provider integration instead). This has been added to to also provide some tests that could be run with a github workflow to showcase a continous integration and continous deployment and how to test these workflows.

### Verify Tests

Just open `src/test/com.example/debug/security/ApiKeyTest.java` and run the tests -> all passed

Get comfortable with the `@WithMockApiKey` annotation

### Run application

You can verify everything works by running the application.

```sh 
cd <project dir>

mvn clean install spring-boot:run
>> application installs including running test
>> applicaiton serves on http://localhost:8080/exampleContext
>> check API key via postman (header = X-API-KEY) for http://localhost:8080/exampleContext/greeting
>> check swagger ui without api in browser at http://localhost:8080/exampleContext/swagger-ui/index.html
```

### Github Workflows

[Github Workflows](https://docs.github.com/en/actions/writing-workflows) allow automation on events.

Common events may be pull requests, updates in pull requeqsts, merges into main, releases, issues etc..

Common automations are e.g.,

- depenendency updates (e.g. dependabot)
- continous integration and continous deployment
- (security) tests

This repository contains a workflow to run unit-tests with maven that is based on another project (see [NOTICE.md](./NOTICE.md)).

#### Run them locally with act

[act](https://github.com/nektos/act) is a tool to run jobs within `./workflows` locally. Configuration of events can
be stored in `./act`

Install by downloading the released binaries and add them to your path.

*Note: act requires [docker](https://docs.docker.com/engine/install/).*

```shell
cd <root dir of repo>

act --list
>> INFO[0000] Using docker host 'unix:///var/run/docker.sock', and daemon socket 'unix:///var/run/docker.sock' 
>> Stage  Job ID     Job name   Workflow name  Workflow file       Events      
>> 0      unit-test  unit-test  Unit Test      unit-test-mvn.yaml  pull_request  

# run action with job-id lint-test for event as defined in pr-event.json
act --job lint-test -e .act/pr_event.json

# if tools are not known, you might need the full fledged runner
act --job unit-test -e .act/pr_event.json -P ubuntu-latest=catthehacker/ubuntu:full-22.04

# if you need maven
# see act --job check-dependencies-backend -e .act/pr_event.json -P ubuntu-latest=quay.io/jamezp/act-maven --pull=false -v
act --job unit-test -e .act/pr_event.json -P ubuntu-latest=quay.io/jamezp/act-maven --pull=false -v

```

## Git Basics

### Basic Configuration

```shell
# ensure correct line endings (repository always linux; when using windows, git knows how to handle it)
git config --global core.autocrlf true

# TODO: Adapt your name and e-mail (e-mail should be associated with your Github account)
git config --global user.name "Tom Meyer"
git config --global user.email Your-mail@domain.tld
```

### Basic Commands

THe following commands are used to create a basic change and prepare the code change for a pull request (pr, GitHub) / merge request (mr, GitLab)

```shell
# update your local working copy with the online state
git pull

# switch the branch (e.g. per feature, per release). Shells commonly allow autocompletion with TAB
git switch branch-name

git status
# indicates 
# - which branch you're working on
# - which files have been changed
# - which files you would like to commit (staged)

# create a branch with current non-committed changes from the branch shown in previous command
git checkout -b branch-name

# Add all changes to be part of the commit
git add *

# Add specific files to be part of the commit. Shells commonly allow autocompletion with TAB
git add path/file

# Do the commit
# That's like writing down your current changes / checking in into version control.
# Please note: THAT'S LOCALLY. OTHERS WON'T SEE IT.
git commit -m "docs: changed something"

# you can do further changes and committs

# send your changes to the online repository (called remote / origin)
# If someone worked on your branch there may be conflicts that need to be resolved with a "git merge"
# If there is an error regarding "remote unkown", copy and execute the mentioned command. You're just on a new branch for which the remote branch has not yet been configured. The command does it.
git push

# more advanced. Let's do together if needed
# It's easy with changes in different areas (automerge)
# It's easy for smaller changes with a ui.
git merge other-branch-of-that-you-want-to-include-changes-in-your-branch
# Note: That's also what happens during a PR
```

### Further Links

The following links support you normalizing / standardizing your daily work with git:

- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) - make concise descriptions of your pull requests
- [Semantic Versioning](https://semver.org/) - consider a versioning sccheme and write your code accordingly

### Test
