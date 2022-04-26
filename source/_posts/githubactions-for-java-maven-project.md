---
title: How to setup Github Actions for Java with Maven project?
date: 2022-04-26 10:25:36
tags: [Test Automation, QA, Tutorial, Github Actions, Java, CICD, Maven]
---

<p>&nbsp;</p>
{%asset_img cover_image.png Cover Image%}
<p>&nbsp;</p>

## Introduction

I had recently completed a POC on Selenium 4 latest features where I have all the example codes of How to use Selenium WebDriver with Java. I was running the tests in my local, however there was a need to have a CI/CD pipeline in place to keep a check that the new code merges are not breaking any existing code.
And since my repository is setup on Github, it was easy to configure the pipeline using Github Actions.

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
{%asset_img action_select_workflow.PNG Action Step one%}
<p>&nbsp;</p>

Next, you need to select the workflow depending on the programming language of your project. Here, I would be selecting `Java with Maven` since my project is Maven based. I have selected the second option in the first row and clicked on Configure.

<p>&nbsp;</p>
{%asset_img create_workflow.PNG Workflow section%}
<p>&nbsp;</p>

Once you click on `Configure` you will be taken to a page where github will auto create a workflow file as per the option you selected and accordingly if you want, you can make the changes in the file as per your needs to install packages, build the project and run the tests. It is fully customizable and provides all the implementation in your hands to choose and add the steps as per your requirement.

Once you have updated all the steps and the commands you want to run within the workflow you can commit the changes by clicking on Green button called `Start Commit` button on top right hand.
It will take you to the `Actions` Tab where you will be able to see the Workflow in Action which you just created.

On the left hand side you will be shown the workflow name, in my case it is `Java CI with Maven` and on the right hand side in the Tab you should be seeing the workflow running and it would be `a yellow dot with commit message`. My workflow is successful and complete hence in the image below you see a green tick beside the workflow run.

<p>&nbsp;</p>
{%asset_img workflow_run.PNG Workflow Run%}
<p>&nbsp;</p>

Now, if you click on the workflow run, it will take you deeper in the run and you should be able to checkout what all things are cooking inside in the run. In my case, it shows 3 jobs running, i.e.

**1. Build and Test**
**2. Code Analysis**
**3. Test Results**

Lets discuss about these jobs in details:

## Build and Test Job

As mentioned earlier, this repository has the code related to web automation and we are using selenium to test the website, hence it is necessary to have the setup accordingly so our tests run successfully without any hiccups.
The important thing to note here is we need to have the browsers installed in the workflow machine to make our tests work.

<p>&nbsp;</p>
{%asset_img build_test_job.PNG Build and Test Job%}
<p>&nbsp;</p>

This job performs the following steps:

1. Checkout the current Repository.

```
 - name: checkout Git repository
   uses: actions/checkout@v2
```

2. Install Java and Maven in the workflow machine and setup the environment to run the tests.

```
 - name: Install Java and Maven
        uses: actions/setup-java@v2
        with:
          java-version: '15'
          distribution: 'adopt'
          cache: maven
```

Let me talk about the next step in detail here as it is related to the end to end tests running in the pipeline. I have configured the end to end tests to run on `localhost`. The tests are related to [OWASP juice-shop website][owaspjuiceshop] and since its code is available on github, I decided to use it and pull the docker image and run the website locally inside the container. Github Actions has a cool feature to run the docker containers easily as its workflow machines comes bundled with [docker][docker]. You can just type docker commands in the `run` step and it will work like a charm.

So, In Step 3, I am running docker commands to pull the image of Juice Shop website and in Step 4, using `docker run`, I am running the juice shop website in detached mode so it keeps on running in background and we would be able to run the tests using `localhost`.

3. Pull the juice Shop docker image - This step will pull the docker image of juice-shop website which we can use in next step to run the website.

```
 - name: Pull Juice Shop Image
   run: docker pull bkimminich/juice-shop
```

4. Run the Juice Shop Website using docker run - This step will run the juice shop website in detached mode and keep it running in background.

```
 - name: Run Juice Shop
   run: docker run -d --rm -p 3000:3000 bkimminich/juice-shop
```

5. Install Chrome - This step will install latest chrome version inside the workflow machine.

```
- name: Install Chrome
  uses: browser-actions/setup-chrome@latest
```

6. Install Firefox - This step will install latest firefox version inside the workflow machine.

```
- name: Install Firefox
  uses: browser-actions/setup-firefox@latest
```

7. Build the Project - Here it will install all the dependencies and build the project. Note, I am not running the tests in this step as it will be done in next step.

```
- name: Build the Project
  run: mvn clean install -DskipTests
```

8. Coverage Per Test Execution

```
- name: Coverage per Test Execution
  run: mvn org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test
```

I have integrated Sonarqube with this repository where it performs static analysis of the code and also checks for the coverage per test in this step.

9. Upload Target Folder - This step uploads target folder so it could be used in further steps for generating reports.

```
- name: Upload target folder
  uses: actions/upload-artifact@v2
  with:
    name: target
    path: |
      ${{ github.workspace }}/target
      ${{ github.workspace }}/reports
```

This concludes the steps for Build and Test Job, next we move on to the next job - Code Analysis.

## Code Analysis Job

I have integrated this repository with [SonarQube][sonarqube] which helps in performing static analysis of the code and helps in writing quality code and use best practices. This job will mostly focus on those steps. In this job we would again have to install everything from scratch as it is a different job. So, lets begin with the details on the step this job does, I will be skipping the setup and installation steps and fcocus on the important and unique steps in this job:

<p>&nbsp;</p>
{%asset_img code_analysis_job.PNG Code Analysis Job%}
<p>&nbsp;</p>

1.  Sonar Cloud Analysis

```
  - name: Sonar Code Analysis
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      SONAR_KEY: ${{ secrets.SONAR_KEY }}
    run: |
      mvn -B verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
        -Dsonar.projectKey=$SONAR_KEY
```

This step will perform the sonar code analysis. It requires Github Token, Sonar Token and Sonar Key for the analysis to run, I have stored these values as Github Secret and hence calling it as a variable here in this step. There are other configuration setting as well which is required for sonar code analysis which you can find in `pom.xml`.

## Test Report Job

This is the final step in the process where the summary of the tests would be taken in to account and a test report would be generated which would show the summary of the tests that ran.

```
  - name: Test Report
    uses: dorny/test-reporter@v1
    if: success() || failure()
    with:
      name: Test Results
      path: ${{ github.workspace }}/target/surefire-reports/TEST-TestSuite.xml
      reporter: java-junit
      java-version: 11
```

Checkout the Test Result image, it shows 46 tests ran successfully in 76 seconds and also provides the summary of tests and its respective test methods that were run.

<p>&nbsp;</p>
{%asset_img test_result_job.png Test Report%}
<p>&nbsp;</p>

## Github Repository

<p>&nbsp;</p>
{%asset_img github_repo_selenium4poc.PNG Github Selenium4poc repository%}
<p>&nbsp;</p>

You can find all the above discussed workflow files as well as other configuration settings in this [github repository][selenium4poc].
Do mark a ⭐ and make the project popular so it reaches to the people who want to learn and upgrade.

Please ping me in case you need any assistance or if you have any query.
Happy Learning!!

<p>&nbsp;</p>

[githubactions]: https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions
[createagithubrepo]: https://docs.github.com/en/get-started/quickstart/create-a-repo
[owaspjuiceshop]: https://github.com/juice-shop/juice-shop
[sonarqube]: https://www.sonarqube.org/
[selenium4poc]: https://github.com/mfaisalkhatri/selenium4poc
[docker]: https://www.docker.com/
