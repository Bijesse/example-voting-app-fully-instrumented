# Example Voting App

A simple distributed application instrumented with New Relic APM running across multiple Docker containers.

Follow the instructions below to have a version of this application running locally and sending data to your New Relic account.

## Architecture

![Architecture diagram](architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time

## Requirements
In order to spin up this application with Docker, you will need the following software:  

* A free account with [New Relic](https://newrelic.com)
* Free access to [GitHub Codespaces](https://github.com/features/codespaces) from your GitHub account

## Getting started - Open a Codespace
1. On this page, click the green "Code" button and select the Codespaces tab.
2. Click the plus sign to generate a new Codespace


## Run this app locally
In order to get this application up and running in your Codespace, you only need to run one command: 
```shell
docker compose up
```

The `vote` app will run at localhost:5100, and the `results` will be at localhost:5001. Codespace will generate a custom URL for each of these apps. Be sure to click the pop-up provided to you once the build is complete.


After experimenting with the app, stop the local instance by pressing `ctrl-C` in the Codespace Terminal window.

*Please note: Although you have a functioning app, at this moment, you are not sending any data to New Relic yet.*

## Instrumentation
This version of the application already includes much of the New Relic instrumentation code needed to send data to New Relic from the following services:

* voting-app-frontend
* example-voting-app-worker
* voting-results  

In order to send data to your New Relic account you must complete the two steps below:

1. In your terminal, change directory to the result directory `cd result` and run the command `npm install newrelic --save`. After the download is complete, navigate back into the root directory `cd ..`
2. Locate the text `<YOUR-NR-INGEST-LICENSE-KEY>` in the 3 files listed below and replace the text with the [New Relic ingest license key](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#ingest-keys) associated with your account.

* /vote/newrelic.ini
* /worker/Dockerfile
* /result/Dockerfile  
  
*If you are interested in instrumenting each of these services yourself, you can find instructions to do so in [this repo](https://github.com/Bijesse/example-voting-app)*

## Generate and view data in New Relic
* Restart your application by running `docker compose up` in your terminal window.
* Exxercise the application at localhost:5100 (the URL previously provided to you by Codespaces) and locate the 3 services in your New Relic account. Data should appear within 5 minutes. 

## Instrument your infrastructure
Extend the instrumentation of this application by instrumenting the Docker containers hosting your application. 

To do this, click the `Add Data` button in the New Relic Explorer on the left navigation pane, Search for `Docker`, and follow the guided install instructions. 

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a client has already submitted a vote.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.
