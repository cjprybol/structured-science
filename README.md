# structured-science

## What

Provide a concise, structured template for conducting, recording, and reporting on experiments in a scalable, reproducable, and machine-readable way.

## Why

In order for an entity to be "data-driven" or "informatics-led" in decision making, "artisan" proof-of-concept and exploratory experiments need to be structured, templatized, and captured in such a way that they can be trivially be automated and reproduced.

All experimental details need to be machine read-able, and complete recording of all inputs, metadata, and raw outputs should to be captured.

By having stuctured, fully-machine readable records of experiments, entities can quantify how much each experiment/workflow contributes to closing knowledge-gaps.

If we have fully-machine readable experiments that enable us to quantify a metric of interest, and also know both of the following:
1. what knowledge we are looking to acquire
2. The workflow(s) that need to be completed to obtain that knowledge
Then we have evertyhing we need to enable automated feedback loops of what needs to be done, in ranked-order of priority

## How

1. define the cost-function that determines whether an objective has been reached
2. define a workflow template and data capture framework that can be run to generate data to asess against the cost function
3. define the parameter space to optimize against
4. conduct experiments to test the parameter space until we have completed enough work to converge 

non-computational project templating:
- labguru
- benchling

Computational project templating (a collection of 1 or more experiments of the same template):
- python-based
  - [cookie cutter datascience](https://github.com/drivendata/cookiecutter-data-science)
    - use conda or virtual environments to manage and define your python environment
- julia-based
  - [doctorwatson](https://juliadynamics.github.io/DrWatson.jl/dev/)
    - use project.toml to manage and define your julia environment
- R-based
  - http://projecttemplate.net/index.html

- all
  - use containers to pre-configure and define the environment
  - containers should pre-install all necessary system packages
  - containers should load in the langauge-specific environment configs

Computational notebook:
- jupyter
- zeppelin
- pluto
- observable
- many more...

Non-computational notebooks:
- labguru
- benchling
- custom solution webapp solution that allows for using templates, forms, and recording of data in a database
  - google forms
  - plolty dash

workflow manager:
- use snakemake or make to define workflows within a repo/project
- use airflow to define workflows that connect repo/projects

transitioning from prototype to production:
- https://12factor.net/

cross-experiment tracking:

![https://preview.redd.it/b3rlshkcqdq61.png](https://preview.redd.it/b3rlshkcqdq61.png?width=1600&format=png&auto=webp&s=c55613adee339bee7f606b81f2a4427cb670ab32)

- straight git*
  - each git branch is an experiment
  - branch/experiment naming pattern?
  - params/config.yaml or .json defines setup
  - jupyter notebook records outputs in a report (use papermill)
  - results, artifacts, and figures are appended to database/data warehouse
  - CI/CD reports back if an experiment was successful or not
  - tableau visualizations of success
- https://iterative.ai/*
  - commercially supported version of the above git-based workflow
- https://github.com/mlflow/mlflow*
- https://github.com/allegroai/clearml/*
- https://github.com/IDSIA/sacred
- https://www.kubeflow.org/
- https://www.pachyderm.com/
- https://github.com/polyaxon/polyaxon
- https://wandb.ai/site (not open source)
- https://neptune.ai/ (not open source)
- https://www.comet.ml/site/ (not open source)

[good guide on an open source mlops flow with deployment and monitoring](https://blog.verta.ai/robust-mlops-with-open-source-modeldb-docker-jenkins-and-prometheus)

[deploy to production with github actions](https://docs.github.com/en/actions/migrating-to-github-actions/migrating-from-jenkins-to-github-actions)

Use time-based scheduling (cron) or event-driven scheduling (make a commit) to trigger CI/CD analyses/rebuilds/

workflows:
- on merge to master, require tests to pass (optional but preferred also a code review)
- all development happens on dev branches that branch off master
- experiments branch off master (or even better, a specific version tag on master)
- experiments upload results and parameters to cloud data storage
- experiment branch names are REPO-VERSION-EXP0000000#

[use repository, user, and organization secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)

## Guidelines

Models should be interpretable in why they are making decisions:
- https://christophm.github.io/interpretable-ml-book/local-methods.html

Provide feedback audits:
- computational: github code reviews
- ELN reviews/audits

Keep experiments and reports to 1-7 pages, where each page is 1 figure with corresponding text. Shorter is better.

Avoid storing any raw data in the experiment.

Instead, store metadata and configuration parameters that can be used to acquire all of the raw data:
- pull from cloud storage
- generate from simulations based on the parameters given
- metadata used to configure a biochemical assay that will generate the results
- etc.

We recommend using github codespaces for development, because:
- super easy to setup
- flexible configuration with variable timeouts, access, and instance sizing
- every project is developed in the docker container in which it will be deployed to production
- YYYY-MM-DD-USER_ID-EXPERIMENT_TITLE.ipynb as the naming convention

[work optimization](https://github.com/oxinabox/ProjectManagement.jl)

## Interaction guidelines

Use the git limit for repo storage as your barometer for whether to cache the results within the repo or whether to rely on external storage locations.

Maximum file size is 100 MB
Maximum repository size is 10 GB (10K MB)
Include as many data points as we can

| # of results (rows/files) | @ result size | sharding required |
|---------------------------|---------------|-------------------|
| 10B                       | 1b            | no                |
| 1B                        | 10b           | no                |
| 100M                      | 100b          | no                |
| 10M                       | 1Kb           | no                |
| 1M                        | 10Kb          | no                |
| 100K                      | 100Kb         | no                |
| 10K                       | 1Mb           | no                |
| 1K                        | 10Mb          | no                |
| 100                       | 100Mb         | no                |
| 10                        | 1Gb           | yes               |
| 1                         | 10Gb          | yes               |
