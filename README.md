# ontology-development-kit

[![Build Status](https://travis-ci.org/INCATools/ontology-development-kit.svg?branch=master)](https://travis-ci.org/INCATools/ontology-development-kit)
[![DOI](https://zenodo.org/badge/48047921.svg)](https://zenodo.org/badge/latestdoi/48047921)


Initialize a Git repo for managing your ontology the OBO Library way!

For more details, see

 * [2018 Article](https://douroucouli.wordpress.com/2018/08/06/new-version-of-ontology-development-kit-now-with-docker-support/)
 * [ICBO Workshop Slides 2018](https://docs.google.com/presentation/d/1nIybviEEJiRKHO2rkBMZsQ0QjtsHyU01_-9beZqD_Z4/edit?usp=sharing)
 * [ICBO Workshop Slides 2017](https://docs.google.com/presentation/d/1JPAaDl6Nitxet9NVqWI30eIygcerYAjdMIGmxbRtIn0/edit?usp=sharing)

# Instructions: Create a new ontology project

We will walk you though the steps to make a new ontology project

## 1. Install and Start Docker

 * [docker](https://www.docker.com/get-docker)

See below for an alternative protocol where you install the software yourself rather than use Docker

## 2. Download the wrapper script and pull latest ODK version

 * Linux/Mac: [seed-via-docker.sh](https://raw.githubusercontent.com/INCATools/ontology-development-kit/master/seed-via-docker.sh)
 * PC: [seed-via-docker.bat](https://raw.githubusercontent.com/INCATools/ontology-development-kit/master/seed-via-docker.bat)
 * First, make sure you have Docker running (you will see the Docker whale in your toolbar on a Mac)
 * To make sure you have the latest version of the ODK installed, run in the command line 

    `docker pull obolibrary/odkfull`

**NOTE** The very first time you run this it may be slow, while docker downloads necessary images. Don't worry, subsequent runs should be much faster!

## 3. Run the wrapper script

You can either pass in a `project.yaml` file that specifies your ontology project setup, or you can pass arguments on the command line.

**NOTE** As per 17th November 2020, the OBO foundry strongly discourages the use of two or three letter OBO ids, and does not permit OBO ids with the letter "O" in it.

Passing arguments on the command line:

    ./seed-via-docker.sh -d po -d ro -d pato -u cmungall -t "Triffid Behavior ontology" triffo

Using a the predefined [examples/triffo/project.yaml](examples/triffo/project.yaml) file:

    ./seed-via-docker.sh -C examples/triffo/project.yaml

You can add a -c (lowercase) just before the -C (capital c) in the command to first delete any previous attempt to generate your ontology with the ODK, and then replaces it with a completely new one.

This will create your starter files in
`target/triffid-behavior-ontology`. It will also prepare an initial
release and initialize a local repository (not yet pushed to your Git host site such as GitHub or GitLab).

You can customize at this stage, or (recommended) after making an initial push to your git host.

## 4. Push to Git hosting website

The development kit will automatically initialize a git project, add all files and commit.

You will need to create a project on you Git hosting site.

*For GitHub:*

 1. Go to: https://github.com/new
 2. The owner MUST be the org you selected with the `-u` option. The name MUST be the one you set with `-t`.
 3. Do not initialize with a README (you already have one)
 4. Click Create
 5. See the section under "…or push an existing repository from the command line"

*For GitLab:*

 1. Go to: https://gitlab.com/projects/new
 2. The owner MUST be the org you selected with the `-u` option. The name MUST be the one you set with `-t`.
 3. Do not initialize with a README (you already have one)
 4. Click 'Create project'
 5. See the section under "Push an existing Git repository"

Follow the instructions there. E.g.

```
cd target/triffid-behavior-ontology
git remote add origin git@github.com:cmungall/triffid-behavior-ontology.git
git push -u origin master
```

Note: you can now mv `target/triffid-behavior-ontology` to anywhere you like in your home directory. Or you can do a fresh checkout from github.


## Next Steps: Edit and release cycle

In your repo you will see a README-editors.md file that has been customized for your project. Follow these instructions.

Generally the cycle is to:

 - branch
 - edit the edit.owl file
 - make test
 - git commit
 - git push

To make a release:

`make prepare_release`

Note that any make step can be preceded by run.sh if you have Docker installed:

`sh run.sh make prepare_release`

## OBO Library metadata

The assumption here is that you are ahdering to OBO principles and
want to eventually submit to OBO. Your repo will contain stub metadata
files to help you do this.

You can create pull requests for your ontology on the OBO Foundry. See the `src/metadata` file for more details.

For more documentation, see http://obofoundry.org

## Additional

You will want to also:

 * enable travis
 * enable zenodo (optional)

See the README-editors.md file that has been generated for your project.

## Troubleshooting.

If you have issues, file them here: https://github.com/INCATools/ontology-development-kit/issues

Some things to check:

 * if something goes wrong you can try again. You may want to remove the `target` dir, or use the `-c` option
 * make sure your ontid has no spaces
 * if your title has spaces, enclose it in quotes


## Customizing

You will likely want to customize the build process, and of course to edit the ontology.

We recommend that you do not edit the main Makefile, but instead the supplemental one (e.g. myont.Makefile) is src/ontology

## Regenerating configuration files in an existing project

DOCUMENTATION TODO

You can recreate the Makefile by running `odk.py create_makefile -C project.yaml`

## Adapting an existing ontology repo

The ODK is designed for creating a new repo for a new ontology. It can still be used to help figure out how to migrate an existing git repository to the ODK structure. There are different ways to do this.

 * Manually compare your ontology against the [template](https://github.com/INCATools/ontology-development-kit/tree/master/template) folder and make necessary adjustments
 * Run the seed script as if creating a new repo. Manually compare this with your existing repo and use `git mv` to rearrange, and adding any missing files by copying them across and doing a `git add`
 * Create a new repo de novo and abandon your existing one, using, for example, github issue mover to move tickets across.
 
Obviously the second method is not ideal as you lose your git history. Note even with `git mv` history tracking becomes harder

If you have built your ontology using a previous version of ODK,
migration of your setup is unfortunately a manual process. In general
you do not absolutely *need* to upgrade your setup, but doing so will
bring advantages in terms of aligning with emerging standards ways of
doing things. The less customization you do on your repo the easier it
should be to migrate.

Consult the [Changes.md](Changes.md) file for changes made between
releases to assist in upgrading.

## More documentation

You will find additional documentation in the src/ontology/README-editors.md file in your repo

## Alternative to Docker

You can run the seed script without docker, you will need Python3.6 or
higher and Java. See requirements.txt for python requirements.

# Running OBO dashboard with ODK

*Note: this is an _highly experimental_ feature as of ODK version 1.2.24. Note that the display and the scores are under active development and will change considerably in the near future.*

Example implementation: 
- https://github.com/obophenotype/obophenotype.github.io ([web](https://obophenotype.github.io/dashboard/index.html))
You need two files to run the ODK dashboard generator:

1. An ODK container wrapper (called `odk.sh` in the following), similar to the `run.sh` file in your typical repos `src/ontology` directory.
2. A dashboard config YAML file (called `dashboard-config.yml` in the following)

With both files, you can then create a dashboard using the following command:

```
sh odk.sh obodash -C dashboard-config.yml
```

The wrapper (`odk.sh`) should contain something like the following:

```
#!/bin/sh
# Wrapper script for ODK docker container.
#
docker run -e ROBOT_JAVA_ARGS='-Xmx4G' -e JAVA_OPTS='-Xmx4G' \
  -v $PWD/dashboard:/tools/OBO-Dashboard/dashboard \
  -v $PWD/dashboard-config.yml:/tools/OBO-Dashboard/dashboard-config.yml \
  -v $PWD/ontologies:/tools/OBO-Dashboard/build/ontologies \
  -v $PWD/sparql:/tools/OBO-Dashboard/sparql \
  -w /work --rm -ti obolibrary/odkfull "$@"
```

Note that this essentially binds a few local directories to the running ODK container. The directories serve the following purposes:

1. `dashboard`: this is where the dashboard is deposited. Look at index.html in your browser.
2. `ontologies`: this is where ontologies are downloaded to and synced up
3. `sparql`: an optional directory that allows you to add custom checks on top of the usual OBO profile.

This is a minimal example dashboard config for a potential phenotype dashboard:

```
title: OBO Phenotype Dashboard
description: Quality control for OBO phenotype ontologies. Under construction.
ontologies:
  custom:
    - id: wbphenotype
    - id: dpo
      base_ns:
        - http://purl.obolibrary.org/obo/FBcv
environment:
  ROBOT_JAR: /tools/robot.jar
  ROBOT: robot
``` 

The ontologies will, if they exist, be retrieved from their OBO purls and evaluated. There are more options potentially of interest:

```
title: OBO Phenotype Dashboard
description: Quality control for OBO phenotype ontologies. Under construction.
ontologies:
  custom:
    - id: myont
      mirror_from: https://raw.githubusercontent.com/obophenotype/c-elegans-phenotype-ontology/master/wbphenotype-base.owl
    - id: dpo
      base_ns:
        - http://purl.obolibrary.org/obo/FBcv
prefer_base: True
profile:
  baseprofile: "https://raw.githubusercontent.com/ontodev/robot/master/robot-core/src/main/resources/report_profile.txt"
  custom:
    - "WARN\tfile:./sparql/missing_xrefs.sparql"
report_truncation_limit: 300
redownload_after_hours: 2
environment:
  ROBOT_JAR: /tools/robot.jar
  ROBOT: robot
``` 

- `mirror_from` allows specifying a download URL other than the default OBO purl
- `base_ns` allows specifying the set of namespaces considered to be _owned_ by the ontology (only terms in these namespaces will be evaluated for this ontology. Default is http://purl.obolibrary.org/obo/CAPTIALISEDONTOLOGYID).
- `report_truncation_limit` allows truncating long (sometimes HUGE ontology reports) to make them go easier on GITHUB version control.
- `redownload_after_hours`: this allows to specify how long to wait before trying to download an ontology (which could be a time consuming process!) again.
- `environment`: is currently a necessary parameter but will be made optional in future versions. It allows adding environment variables directly to the config, rather than passing them in as -e parameters to the docker container (both are equivalent though.)
- `profile` is an optional parameter that allows specifying your own profile for the quality control (ROBOT) report. By default, this is using the ROBOT report default profile. You can either specify your own profile from scratch, or extend the current default with additional test by using the `baseprofile` parameter. Find out more about ROBOT profiles [here](http://robot.obolibrary.org/report#profiles).

A fully working example can be found [here](https://github.com/obophenotype/obophenotype.github.io).
