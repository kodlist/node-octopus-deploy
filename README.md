# node-octopus-deploy
Node script to create a release within Octopus Deploy, and optionally also deploys that release.  
This package uses the Octopus Deploy REST API.

	https://github.com/OctopusDeploy/OctopusDeploy-Api/wiki

This module was specifically created in order to initiate a release and deploy from a linux machine.

# Installation

	npm install node-octopus-deploy
	
# Usage

This module tries to use promises whenever possible, specifically [bluebird](https://github.com/petkaantonov/bluebird) promises.

## Setup Client

	var OctoDeployApi = require('octopus-deploy');
	
	var config = {
		host: 'https://deploy.mycompany.com',
		apiKey: 'ABC-123' // This is used to authorize against the REST Api
	};
	
	var client = new OctoDeployApi(config);

## Helper
 
### Simple - Create Release And Deploy

	// Release Information
	var projectIdOrSlug = 'my-project-name';
	var version = '1.0.0-rc-3';
	var releaseNotes = 'Release notes for testing';
	
	// Deployment Information
	var environmentName = 'DEV-SERVER';
	var comments = 'Deploy releases-123 to DEVSERVER1';
	// Form Value Example: Source Directory
	var variables = {
		'SourceDir': '\\\\SOURCESERVER\\MyProject\\1.0.0-rc-3'
	};
	
	// Create Deployment
	var deploymentPromise = client.helper.simpleCreateReleaseAndDeploy(
		projectIdOrSlug, version, releaseNotes, environmentName, comments, variables);
		
	// Print out deployment
	deploymentPromise.then(function(deployment) {
		console.log('Deployment Created...');
		console.log(deployment);
	});

## Release

### Create

	var projectIdOrSlug = 'my-project-name';
	var version = '1.0.0-rc-3';
	var releaseNotes = 'Release notes for testing';
	
	// Note: you can pass 
	releasePromise = client.release.create(projectIdOrSlug, version, releaseNotes);
	
	releasePromise.then(function (release) {
		console.log('Release Created...');
		console.log(release);
	});
	
## Other

Other methods are exposed but I didn't want to take the time to document them right now.
You can find them in the `.\lib` folder. 
Notice not all resource/methods are implemented 
(Feel free to fork and submit pull request for your needs).

- deployment
- environment
- helper (custom mashup of multiple calls)
- project
- release
- variable

# Testing

This project uses gulp for running tests.  All tests reside in the `.\test` folder.

To run tests...

	gulp test
	
To run tests in a TDD mode with a watch...

	gulp dev
	
# TODO

There are a few error cases that do not have test cases right now.

# Contributing

If there are other API functions you need, feel free to fork the project,
submit a pull request, and I'll try to keep up to date.

# License

The MIT License (MIT)

Copyright (c) 2014 Isaac Johnson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.