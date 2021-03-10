# Timb� status report

![Build status](https://travis-ci.com/timbo-learning/dotnet-code-coverage.svg?branch=travis) on [Travis](https://travis-ci.com/timbo-learning/dotnet-code-coverage)

![Coverage Badge](report-generator-coverage/badge_combined.svg) by ReportGenerator

![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=timbo%3Adotnet-code-coverage&metric=code_smells)
![Code Coverage](https://sonarcloud.io/api/project_badges/measure?project=timbo%3Adotnet-code-coverage&metric=coverage)
![Lines of Code](https://sonarcloud.io/api/project_badges/measure?project=timbo%3Adotnet-code-coverage&metric=ncloc)
![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=timbo%3Adotnet-code-coverage&metric=alert_status)
<img src="https://sonarcloud.io/images/project_badges/sonarcloud-white.svg" height="22">
## Graphic Report

#### SonarQube

Results available on [Sonarcloud](https://sonarcloud.io/dashboard?id=timbo%3Adotnet-code-coverage)

1. More diff-like comparison, focused on new code and previous build, but still has history.
2. Coverage History
3. Integrates into build process
4. cross-platform
5. Graphical Report
6. Compatible with xUnit
7. Does not offer Branch Coverage like ReportGenerator

These have been ensured because I run the tools both locally on the Windows machine,
and on Travis with the trusty linux distribution, all with cross-platform python scripts

Overview showing Quality Gate failure due to Code coverage < 80%
and number of new lines to cover:
![Overview](screenshots/sonarcloud/sonarcloud-overview.PNG)

![Sonarcloud Coverage](screenshots/sonarcloud/sonarcloud-coverage.PNG)
Uncovered Lines
![Uncovered Lines](screenshots/sonarcloud/sonarcloud-uncovered-lines.PNG)
Coverage Chart
![Coverage Chart](screenshots/sonarcloud/sonarcloud-coverage-chart.PNG)

#### SonarQube, getting it to work

###### Screenshots (buggy?)
![](screenshots/sonarcloud/bug/sonarcloud-coverage.PNG)
![](screenshots/sonarcloud/bug/sonarcloud-coverage-empty-graph.PNG)
![](screenshots/sonarcloud/bug/sonarcloud-line-coverage.PNG)

###### Crawled through these links

[Apparently sonarscanner does not work for .net projects](https://github.com/SonarSource/sonar-dotnet/issues/1034)

[Analyzing with SonarQube Scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner)

[SonarQube Overview](https://docs.sonarqube.org/latest/analysis/overview/)

[Docker for Windows](https://docs.docker.com/docker-for-windows/install/)

<!-- https://docs.sonarqube.org/display/PLUG/Python+Coverage+Results+Import -->
##### Using Travis-CI integration

1. Set up Travis-ci.
[C# Project](https://docs.travis-ci.com/user/languages/csharp/) |
[Custom Build](https://docs.travis-ci.com/user/customizing-the-build/)
1. Travis-CI does not support dotnet 2.2 yet. Downgraded to 2.1.
2. Following sonarqube instructions.
[SonarCloud + Travis](https://docs.travis-ci.com/user/sonarcloud/)
3. Generated a Token for the app and included `SONAR_TOKEN` on Travis Settings
<!-- 4. Changed some configuration on sonarcloud under `Administration>General Settings`, such as Test files and source files. -->
5. Add configuration to sonar-project.properties 
[List of sonar parameters](https://docs.sonarqube.org/latest/analysis/analysis-parameters/) |
[sonar-project.properties example](https://github.com/SonarSource/sonar-scanning-examples/blob/master/sonarqube-scanner/sonar-project.properties)
6. Changing to SonarScanner for MSBuild [Analyzing with SonarScanner for MSBuild](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+MSBuild)
7. 


#### Jenkins

1. Installed Jenkins locally.
2. Installed github, "SonarQube Scanner for Jenkins" and "Sonar Quality Gates Plugin" plugins on Jenkins
3. [Analyzing with SonarQube Scanner for Jenkins](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Jenkins)
4. Configuration on `Manage System`
![Jenkins Pipeline](screenshots/jenkins/jenkins-pipeline.PNG)

###### Bitbucket+Jenkins integration

1. Install `Bitbucket Plugin` [on Jenkins](https://wiki.jenkins.io/display/JENKINS/Bitbucket+Plugin)
2. `ngrok.exe http <JENKINS_PORT>`
   a. get the exposed URL on [ngrok.com](https://ngrok.com)
3. Create webhook user on Jenkins with Permissions:
   a. Overall Read
   2. Job Build
   3.  Job Configure
   4.  Job Create
   5.  Job Read
4. Disable `Prevent Cross Site Request Forgery exploits`
   a.  so that we dont get the error `no valid crumb was included in the request`
2.   Tick _Build Trigger_ `Build when a change is pushed to BitBucket` on project configuration
6. (TODO) Build based on `Jenkinsfile` Pipeline

#### Grafana

Grafana seems to be more for DevOps Runtime metrics rather than development testing for code coverage.
Skipping

[Grafana Live Demo](http://play.grafana.org/)
![Grafana Live Demo](screenshots/grafana/grafana-demo.PNG)

#### Report Generator 

Generates a very descriptive `index.htm` under `report-generator-coverage`,
showing line coverage, branch coverage, covered lines, build history.

1. Coverage History
3. Integrates into build process
4. cross-platform
    1. Error on Travis(Linux): `Error during rendering summary report (Report type: 'Badges'): Arial could not be found`
5. Graphical Report
6. Compatible with xUnit
1. Cannot see line coverage of past builds

on the project, run:

```
python3 coverlet.py
cd CalculationTests
python3 report-generator.py
```

###### Report Generator Outputs

Branch Coverage ![Branch Coverage Badge](report-generator-coverage/badge_branchcoverage.svg)

Line Coverage ![Line Coverage Badge](report-generator-coverage/badge_linecoverage.svg)

Combined Coverage![Coverage Badge](report-generator-coverage/badge_combined.svg)

    Generated on:	1/3/2019 - 12:29:02 PM
    Parser:	MultiReportParser (2x OpenCoverParser)
    Assemblies:	2
    Classes:	5
    Files:	5
    Covered lines:	47
    Uncovered lines:	8
    Coverable lines:	55
    Total lines:	125
    Line coverage:	85.4%
    Branch coverage:	100%

Includes *build history*:

![Report Generator Coverage](screenshots/report-generator/report-generator-coverage.PNG)

You can click on a class to see its details:
![Report Generator Addition Class](screenshots/report-generator/report-generator-addition.PNG)

And it shows line coverage (as shown before on Visual Studio Code).
![Report Generator Addition Line Coverage](screenshots/report-generator/report-generator-addition-line-coverage.PNG)

Next steps are trying Grafana or SonarQube.
#### Report Generator, getting it to work

[Usage instructions](https://danielpalme.github.io/ReportGenerator/usage.html) (generates the cli command)

``` bash
dotnet add package ReportGenerator
```

Make sure to include reportgenerator in your `.csproj`:

    <ItemGroup>
        <DotNetCliToolReference Include="dotnet-reportgenerator-cli" Version="4.0.4" />
      </ItemGroup>

In this case, I used the `CalculationTests.csproj` to produce the report for both projects (Calculation and PrimeService).
Apparently ReportGenerator just needs the xml files generated by `coverlet`.

#### Graphic Reports options

Following https://medium.com/agilix/collecting-test-coverage-using-coverlet-and-sonarqube-for-a-net-core-project-ef4a507d4b28

Good Candidates:

0. [Grafana](https://grafana.com/)
1. [SonarQube](https://www.sonarqube.org/) (comes in form of a Docker Image also) (can plugin from github).
    SonarQube seems to be microservices centered because
    it launches a web server and sits behind a HTTP API.
2. [ReportGenerator](https://www.nuget.org/packages/ReportGenerator/4.0.4)

## Integrating with Visual Studio

For graphical display, I'm looking for a integration with visual studio,

Following
https://www.hanselman.com/blog/NETCoreCodeCoverageAsAGlobalToolWithCoverlet.aspx

https://dotnetthoughts.net/code-coverage-in-netcore-with-coverlet/
also seems useful

Tried installing [Coverage Gutters](https://marketplace.visualstudio.com/items?itemName=ryanluker.vscode-coverage-gutters&WT.mc_id=-blog-scottha),
Tried renaming .vsix to .zip, extracting and tinkering with extension.vsixmanifest and back, did not work.

Failed on Visual Studio Professional 2017.
Works on Visual Studio **Code**.

![Coverage-Gutter](screenshots/vs-code-coverage-gutters.png)

[codecover](https://marketplace.visualstudio.com/items?itemName=bradleymeck.codecover) as a second option to Coverage Gutters perhaps


[dotCover](https://marketplace.visualstudio.com/items?itemName=JetBrains.dotCover) also seems to be a solution that does not use coverlet, but it is paid.


## Coverlet First run


``` bash
mkdir coverlet-output
bin/coverlet.exe PrimeServiceTests/bin/Debug/netcoreapp2.1/PrimeServiceTests.dll --target dotnet --targetargs "test PrimeServiceTests --no-build" -o coverlet-output/
```

Output:

    Test run for C:\Users\rtimbo\source\repos\samples\core\getting-started\unit-testing-using-dotnet-test\PrimeService.Tests\bin\Debug\netcoreapp2.2\PrimeService.Tests.dll(.NETCoreApp,Version=v2.2)
    Microsoft (R) Test Execution Command Line Tool Version 15.9.0
    Copyright (c) Microsoft Corporation.  All rights reserved.

    Starting test execution, please wait...

    Total tests: 11. Passed: 11. Failed: 0. Skipped: 0.
    Test Run Successful.
    Test execution time: 1.5248 Seconds


    Calculating coverage result...
      Generating report 'coverlet-output\coverage.json'
    +--------------------------------------------------+--------+--------+--------+
    | Module                                           | Line   | Branch | Method |
    +--------------------------------------------------+--------+--------+--------+
    | PrimeService                                     | 100%   | 100%   | 100%   |
    +--------------------------------------------------+--------+--------+--------+
    | xunit.runner.reporters.netcoreapp10              | 1.1%   | 0.5%   | 5.1%   |
    +--------------------------------------------------+--------+--------+--------+
    | xunit.runner.utility.netcoreapp10                | 15.7%  | 9.1%   | 21%    |
    +--------------------------------------------------+--------+--------+--------+
    | xunit.runner.visualstudio.dotnetcore.testadapter | 45.6%  | 35.7%  | 47.8%  |
    +--------------------------------------------------+--------+--------+--------+

    +---------+--------+--------+--------+
    |         | Line   | Branch | Method |
    +---------+--------+--------+--------+
    | Total   | 23.8%  | 18%    | 26.6%  |
    +---------+--------+--------+--------+
    | Average | 5.95%  | 4.5%   | 6.65%  |
    +---------+--------+--------+--------+

Output generated on coverlet-output/coverage.json

To exclude xunit, include flag `--exclude "[xunit.runner.*]*"` as follows

```
bin/coverlet.exe PrimeServiceTests/bin/Debug/netcoreapp2.1/PrimeServiceTests.dll --exclude "[xunit.runner.*]*" --target dotnet --targetargs "test PrimeServiceTests --no-build" -o coverlet-output/
```

    Test run for C:\Users\rtimbo\source\repos\samples\core\getting-started\unit-testing-using-dotnet-test\PrimeService.Tests\bin\Debug\netcoreapp2.2\PrimeService.Tests.dll(.NETCoreApp,Version=v2.2)
    Microsoft (R) Test Execution Command Line Tool Version 15.9.0
    Copyright (c) Microsoft Corporation.  All rights reserved.

    Starting test execution, please wait...

    Total tests: 11. Passed: 11. Failed: 0. Skipped: 0.
    Test Run Successful.
    Test execution time: 1.2332 Seconds


    Calculating coverage result...
      Generating report 'coverlet-output\coverage.json'
    +--------------+--------+--------+--------+
    | Module       | Line   | Branch | Method |
    +--------------+--------+--------+--------+
    | PrimeService | 100%   | 100%   | 100%   |
    +--------------+--------+--------+--------+

    +---------+--------+--------+--------+
    |         | Line   | Branch | Method |
    +---------+--------+--------+--------+
    | Total   | 100%   | 100%   | 100%   |
    +---------+--------+--------+--------+
    | Average | 100%   | 100%   | 100%   |
    +---------+--------+--------+--------+


## dotnet test First Run

Under the project:

``` bash
$ dotnet restore
$ dotnet test
```

Output of the test was:

    $ dotnet test
    Build started, please wait...
    Skipping running test for project C:\Users\rtimbo\source\repos\samples\core\getting-started\unit-testing-using-dotnet-test\PrimeService\PrimeService.csproj. To run tests with dotnet test add "<IsTestProject>true<IsTestProject>" property to project file.
    Build completed.

    Test run for C:\Users\rtimbo\source\repos\samples\core\getting-started\unit-testing-using-dotnet-test\PrimeService.Tests\bin\Debug\netcoreapp2.2\PrimeService.Tests.dll(.NETCoreApp,Version=v2.2)
    Microsoft (R) Test Execution Command Line Tool Version 15.9.0
    Copyright (c) Microsoft Corporation.  All rights reserved.

    Starting test execution, please wait...

    Total tests: 11. Passed: 11. Failed: 0. Skipped: 0.
    Test Run Successful.
    Test execution time: 1.5296 Seconds

#### More sample code

[Calculation](https://andrewlock.net/creating-parameterised-tests-in-xunit-with-inlinedata-classdata-and-memberdata/)

# Original README.MD

# Unit testing using dotnet test sample

This sample is part of the [unit testing tutorial](https://docs.microsoft.com/dotnet/core/testing/unit-testing-with-dotnet-test) for creating applications with unit tests included. See that topic for detailed steps on the code for this sample.

## Key features

This sample demonstrates creating a library and writing effective unit tests that validate the features in that library. The example provides a service that indicates whether a number is prime.

## Restore and test

To run the tests, navigate to the *PrimeServiceTests* directory and type the following commands:

```
dotnet restore
dotnet test
```

`dotnet restore` restores the packages of both projects.
`dotnet test` builds both projects and runs all of the configured tests.
