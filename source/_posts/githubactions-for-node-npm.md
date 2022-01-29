---
title: How to setup Github Actions for nodejs project?
date: 2022-01-28 19:51:36
tags:
  [Test Automation, QA, Tutorial, Github Actions, SuperTest, CICD, Nodejs, npm]
---

<p>&nbsp;</p>
{%asset_img githubactions_banner.png Header Image%}
<p>&nbsp;</p>

## Introduction

Yesterday I was doing a POC on Super Test, an HTTP assertions library that allows you to test your APIs using Javascript. A thought came to my mind to add the pipeline using Github Actions, since my code is checked-in on Github. It will help me running the regression tests on the pipeline and provide me an advantage to check for the failing cases/issues in the code before I merge anything to main branch. Hence, I added the github action workflow in the repository.
In this blog, I would be providing the walkthrough of how to setup github actions for nodejs. It would be a guiding tutorial for the folks who want to use this feature and would be a documentation for me as well.

## What is github actions?

As per the [Github actions website][githubactions], GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.

GitHub Actions goes beyond just DevOps and lets you run workflows when other events happen in your repository. For example, you can run a workflow to automatically add the appropriate labels whenever someone creates a new issue in your repository.

GitHub provides Linux, Windows, and macOS virtual machines to run your workflows, or you can host your own self-hosted runners in your own data center or cloud infrastructure.

## How to setup github actions workflow?

Setting up github actions is very easy, there is no complex tasks or files that you need to create. It can be setup in the following simple steps:

### Step 1 - Create a Github Repo and check-in all your code on Github

Creating and checking all your code on Github is again simple task. Checkout the [link here][createagithubrepo] which will help you to create a repository and check in your code on github.

### Step 2 - Go To Actions Tab and select appropriate workflow depending on your programming language

Once the Github repository is created and all the code is checked-in. You will have to click on the `Actions` Tab as shown in the pic below.

<p>&nbsp;</p>
{%asset_img actions_1.PNG Action Step one%}
<p>&nbsp;</p>

Next, you need to select the workflow depending on the programming language of your project. Here, I would be selecting `Node.js` since my project is Nodejs based. I have selected the third option in the first row and clicked on Configure.

{%asset_img workflow_selection_2.PNG Workflow section%}

Once you click on `Configure` your will be taken to a page where github will auto create a workflow file as per the option you selected and accordingly if you want, you can make the changes in the file as per your needs to install packages, build the project and run the tests. I liked this as it is totally customizable and provides all the implementation in your hands to choose and add the steps as per your requirement.

<p>&nbsp;</p>
{%asset_img workflow_creation_3.PNG Workflow Yml%}
<p>&nbsp;</p>

Once you have updated all the steps and the commands you want to run within the workflow you can commit the changes by clicking on Green button called `Start Commit` button on top right hand.
It will take you to the `Actions` Tab where you will be able to see the Workflow in Action which you just created.

On the left hand side you will be shown the workflow name, in my case it is `Node.js CI` and on the right hand side in the Tab you should be seeing the workflow running check `a yellow dot with commit message` in the below screenshot:

<p>&nbsp;</p>
{%asset_img workflow_run_4.PNG Workflow Run%}
<p>&nbsp;</p>

Now, if you click on the workflow run, it will take you deeper in the run and you should be able to checkout what all things are cooking inside in the run. In my case, it shows 3 jobs running, i.e.

1. build (12.x)
2. build (14.x)
3. build (16.x)

This is because as per my workflow, I have selected to run my build and tests on 3 different versions of `Nodejs`, namely, 12, 14 and 16.

<p>&nbsp;</p>
{%asset_img workflow_run_5.PNG Workflow Jobs%}
<p>&nbsp;</p>

On the right hand pane, if you click the `show all jobs` tab, you will be further taken into the job details. Below is the screenshot of jobs running in Node 16:

<p>&nbsp;</p>
{%asset_img workflow_run_node16.PNG Workflow Jobs node 16%}
<p>&nbsp;</p>

Once all the jobs are complete, Test Report will be published as a part of next step. I have configured `mocha-json` reporter since I am using mocha to run the tests:

<p>&nbsp;</p>
{%asset_img testreport.png  Workflow Test Report%}
<p>&nbsp;</p>

## Understanding the workflow yml file

Checkout the Worflow file in the [Github repository here][supertestpoc]
Workflow yml file can be found inside the `.github/workflows` folder, filename is `node.js.yml`.

Let me explain you what the content of the yml file looks like and what it means.

<p>&nbsp;</p>
<img src="yaml_1.png" width="800" height="500" alt="Workflow Yml Part 1" title="Workflow Yml Part 1">
<p>&nbsp;</p>

First of all in the workflow you need to assign a name to the workflow which can be done using the `name` tag on the first line of the file.
Next, we need to tell workflow when it should run, for that `on` tag can be used alongwith telling the workflow when to get started.

Here, I have set to run whenever a code is **pushed to _main_** branch and also when code is **pushed to a brach having name like _issue-_** since I create the fix branches name with _issue-_ following the issue number.

Also, it will run whenever a Pull Request is raised on _*main*_ or _*issue-*_ branches.

<p>&nbsp;</p>
<img src="yaml_2.png" width="800" height="500" alt="Workflow Yml Part 2" title="Workflow Yml Part 2">
<p>&nbsp;</p>

Next, we need to setup the jobs to be run. For this we need to provide the machine image we would be using for running the build. In our case, it will be running on `ubuntu-latest`. The next is the strategy to run the build. We need to set the node version on which we need to run the tests. I found this very flexible and amazing, it was because we can provide multiple node versions. In our case, it will be following 3 node versions:

1. Node 12.x
2. Node 14.x
3. Node 16.x

This is amazing as it helps us run the build on multiple environments thereby allowing us to leverage our build and test startegy on multiple node verions which is a kind of complex thing to do on local machine.

Once the machine and nde versions are set. We can continue to set the `steps` we need to run the build and test stage.

<p>&nbsp;</p>
<img src="yaml_3.png" width="800" height="500" alt="Workflow Yml Part 3" title="Workflow Yml Part 3">
<p>&nbsp;</p>

1. `actions/checkout@v2` >> This action checks-out your repository under `$GITHUB_WORKSPACE`, so your workflow can access it.
2. `actions/setup-node@v2` will setup the node versions as defined in the `build: strategy` where we have mentioned the different node versions we need for this build. These node version will be taken in consideration using `node-version: ${{ matrix.node-version }}`. In this Step, your github repository will be checked out, and node will be setup in the build machine. Now, your machine is setup and is ready to install and build what all npm packages you need for your project to build and run the tests.
3. In the next step, we will install `superTest`, `chai` and `mocha` since these packages are needed to build and run the tests.
4. Once packages are installed, we would be running the following commands, to package, build and run the tests.
   1. _npm ci_
   2. _npm run build --if-present_
   3. _npm test_

One point to mention here is, I have already added the script in the `package.json` to run the script with npm test. Here is the snippet of the `test` script from `package.json`, so when I run `npm test` it will call the following script:

```
"scripts": {
		"test": "mocha **/*.spec.js && mocha --reporter json > reports/test-results.json"
	}
```

<p>&nbsp;</p>
<img src="yaml_4.png" width="800" height="500" alt="Workflow Yml Part 4" title="Workflow Yml Part 4">
<p>&nbsp;</p>

In the end, I have added a `Test-Report` step which will be shown once all the tests are run in the workflow and will show the results so any user can checkout and see the summary of the tests run with its actual outcome. It uses `dorny/test-reporter@v1`, report will be generated if tests pass or fail,i.e everytime the workflow is run. It will pick data from the json file in the `/reports/test-results.json` file and `mocha-json` reporter format would be used to show the report.

## Running the Workflow

As mentioned earlier, Workflow would be run once the code is pushed to branch having name like `issue` or `main` branch.

### This is how it looks:

<p>&nbsp;</p>
{%asset_img workflow_run_1.png Workflow all jobs%}
<p>&nbsp;</p>

### Here is the how the workflow jobs are shown in case a PR is raised:

<p>&nbsp;</p>
{%asset_img raisingpr.PNG Workflow when PR Raised%}
<p>&nbsp;</p>

## Conclusion

I hope you would have got a better understanding of how the Github Actions workflow works and how to set it up. Its a good way of setting up your own customized pipeline and automate the tedious tasks which eats up most of your time.
If you have any queries or doubt about this please feel free to reach me and discuss the same.

-- Faisal Khatri

<p>&nbsp;</p>

[githubactions]: https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions
[createagithubrepo]: https://docs.github.com/en/get-started/quickstart/create-a-repo
[supertestpoc]: https://github.com/mfaisalkhatri/SuperTest_poc
