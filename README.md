# Example Voting App

A simple distributed application instrumented with New Relic APM running across multiple Docker containers.

Follow the instructions below to have a version of this applicaiton running locally and sending data to your New Relic account.

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
* [Git](https://github.com/git-guides/install-git) - You can verify installation with `git –version`
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) - You can verify installation with `docker run hello-world`

*Note: the above verification commands should be ran in Terminal on Mac or Command Prompt on Windows*

## Getting started - Clone this repository
1. From a new Terminal or Command Prompt window, clone this repository using Git `git clone https://github.com/Bijesse/example-voting-app-fully-instrumented.git`
2. Navigate into your new workspace using `cd example-voting-app-fully-instrumented`


## Run this app locally
In order to get this application up and running on your machine, you only need to run one command: 
```shell
docker compose up
```

The `vote` app will be running at [http://localhost:5100](http://localhost:5100), and the `results` will be at [http://localhost:5001](http://localhost:5001).


After experimenting with the app, stop the local instance by pressing `ctrl-C` in your Terminal window.

*Please note: Although you have a functioning app, at this moment, you are not sending any data to New Relic yet.*

## Instrumentation
This version of the application already includes all of the New Relic instrumentation code needed to send data to New Relic from the following services:

* voting-app-frontend
* example-voting-app-worker
* voting-results  

The only thing you need to do to send data to your New Relic account is to input your Ingest License key into each service. *If you are interested in instrumenting each of these services yourself, you can find instructions to do so in [this repo](https://github.com/Bijesse/example-voting-app)*

Locate the text `<YOUR-NR-INGEST-LICENSE-KEY>` in the 3 files listed below and replace the text with the [New Relic ingest license key](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#ingest-keys) associated with your account.

* /vote/newrelic.ini
* /worker/Dockerfile
* /result/Dockerfile  

## Generate and view data in New Relic
* Restart your applicaiton by running `docker compose up` in your terminal window.
* Exxercise the application at [http://localhost:5100](http://localhost:5100) and locate the 3 service in your New Relic account. Data should appear within 5 minutes. 

## Instrument your infrastructure
Extend the instrumentation of this application by instrumenting the Docker containers hosting your application. 

To do this, click the `Add Data` button in the New Relic explorer on the left hand navigation pane, Search for `Docker` and follow the guided install instructions. 

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.
