# gradle-marathon-deployer

[![Build Status](https://travis-ci.org/alenkacz/gradle-marathon-deployer.svg?branch=master)](https://travis-ci.org/alenkacz/gradle-marathon-deployer) [ ![Download](https://api.bintray.com/packages/alenkacz/maven/gradle-marathon-deployer/images/download.svg) ](https://bintray.com/alenkacz/maven/gradle-marathon-deployer/_latestVersion)

Gradle plugin that can deploy your application to Marathon (https://mesosphere.github.io/marathon/).

Apart from deploying your application to Marathon it also verifies, that the deployment is actually finished by querying the Marathon event stream.

If you want to deploy your application using canary deployments, *deployCanaryToMarathon* task takes the marathon.json and deploys it under the same name, just with "-canary" as a suffix. Thanks to this, you can have both old and new version deployed in parallel.

Usage
====================

	buildscript {
		repositories {
			jcenter()
		}
		dependencies {
			classpath 'cz.alenkacz.gradle:gradle-marathon-deployer:1.2.3'
		}
	}

	apply plugin: 'marathon-deploy'
    
    marathon {
    	url = "http://path-to-your-marathon-instance.com"
    }

Properties
====================
- *url* - url to your marathon application
- *dockerImageName* (OPTIONAL) - full image name that will be provided to marathon, use this only if you want to override the image name provided in your json (e.g. you want to deploy a testing version as a part of your CI/CD pipeline)
- *pathToJsonFile* (OPTIONAL) - project relative path to your json file, default is *deploy/marathon.json*
- *verificationTimeout* (OPTIONAL) - timeout when querying Marathon to verify that there are no pending deployments left, default is 90 seconds
- *deploymentRequestTimeout* (OPTIONAL) - timeout for the initial deployment request, default is 5 seconds

Tasks
====================
Tasks are added under the publishing group in yous gradle project.

- *deployToMarathon* - deploys your application to Marathon
- *deployCanaryToMarathon* - canary deployment on top of Marathon. Creates new application id (just the original application id with postfix "-canary") and pushes that to Marathon in one instance. Read more about [canary deployment](http://martinfowler.com/bliki/CanaryRelease.html)
