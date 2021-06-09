# Hyperledger Aries Demo : Connecting two agents through AriesToolbox<!-- omit in toc -->

- [Overview](#overview)
- [How to Run](#how-to-run)
- [Instructions](#instructions)
  - [Cloning the repository on your local machine](#cloning-the-repository-on-your-local-machine)
  - [Running ACA-Py Agents](#running-aca-py-agents)
  - [Running AriesToolbox and connecting the agents to AriesToolbox](#running-ariesToolbox-and-connecting-the-agents-to-ariestoolbox)
  - [Creating a connection between the Factory and the Company agents](#creating-a-connection-between-the-factory-and-the-company-agents)
  - [Registering to Sovrin StagingNet](#registering-to-sovrin-stagingNet)
  - [Creating the report schema](#creating-the-report-schema)
  - [Stopping the Lab](#stopping-the-lab)
- [Bibliography](#bibliography)


## Overview

In this demo, we connect two agents, Factory and Company, using Hyperledger Aries ecosystem to secure the authentification and communication process. 

## How to Run

Since Aries Toolbox is an [Electron](https://www.electronjs.org/) app, you must run this lab on your own system—you can’t use Play with Docker. We’ll still be using [Docker](https://docs.docker.com/engine/install/ubuntu/) for some ACA-Py agents that we’ll connect to the Aries Toolbox, but the Toolbox itself must be run on a local system.

The need to run Aries Toolbox locally will add a new dependency to our list. To run this lab you need to have [nodejs](https://nodejs.org/) and [npm](https://www.npmjs.com/) (node package manager) installed. If you don’t have those installed on your system already, follow the instructions [here](https://www.npmjs.com/get-npm) to install them.

## Instructions

This [Video of the demonstration](https://www.youtube.com/watch?v=wlFwevC4IMM) can help you with the instructions. But it has no sound, so make sure you follow the written instructions as well.

Please open up two terminals running bash, and we’ll start in the first.

### Cloning the repository on your local machine

```
git clone https://github.com/FlowBigby/cassiopee-decentralized-identity
cd cassiopee-decentralized-identity

```

### Running ACA-Py Agents

Create the two agents Factory and Company :

```
./CreateAgents.sh

```
If you don't have the read/write/execute rights, use chmod command.

If all goes well, you should see on the **terminal** information in a square box framed in colons (“:”) listing info about the agents for Factory and Company, and below that, invitation URLs. Don't close this terminal and keep these URLs for the following section.

### Running AriesToolbox and connecting the agents to AriesToolbox

```
source LaunchAriesToolbox.sh

```
If all goes well, you should have an Aries Toolbox running on your screen, with a place to paste new invitations.

Find one of the invitation URLs, highlight and copy the text to your clipboard, paste it into the Aries Toolbox invitations field and click ‘Connect.’ You should see a new connection in the Aries Toolbox window, and after you click "Open", a new window for the newly connected agent. Repeat for the second agent (you may have to scroll quite a way in your terminal window).

You should see a long menu in the left side menu of each agent. If you don't, although the agent did get a connection with the Toolbox, it is not running. If that happens, you should stop (see instructions below) and start the Alice and Bob agents again.

Now, you have two Aries agents, Factory and Company, connected to the Toolbox.


### Creating a connection between the Factory and the Company agents

There are not currently a lot of instructions on what you can do with the Aries Toolbox, but here are a few ideas:

*   Use the “Invitations” menu item in the Factory window to create and copy an invitation, and paste it into the appropriate field in the “Connections” menu item of Company’s window. That should enable a connection between Factory and Company.
*   Find the DIDs that Factory and Company are using for connections to each other and “Activate” those connections from the “Connections” menu item.

### Registering to Sovrin StagingNet

To enable your agents to create schema and write in the Sovrin ledger, you need to make them both as Endorsers. To do so, follow these steps :

*   Connect to https://selfserve.sovrin.org/
*   Select "StagingNet" as the Network
*   Enter the DID and the Verkey of your agent relative to Toolbox. 

You can get your own agent DID and Verkey by browsing in the "Agent by Agent" section, and clicking on the "DIDs" subsection. Make sure to have done these instructions for both Factory and Company agents.

<details>
    <summary>Show me a screenshot!</summary>
    <img src="https://cdn.discordapp.com/attachments/776834563950379038/851123712773324830/RegisterStagingNet.png">
</details>

### Creating the report schema

We want the Company to generate a report after its intervention, following a template created previously by the Factory.

#### 1.   The Factory creates the template (schema),

Go to the "Schema Insuance" section, click on "Create a new schema" and fill it with the relevant attributes (we chose date, company, description and price in our demo).
<details>
    <summary>Show me a screenshot!</summary>
    <img src="https://cdn.discordapp.com/attachments/776834563950379038/851130972530671626/Schema.png">
</details>

#### 2.   The Company retrieves the schema ID from the Sovrin StagingNet on the [Domain ledger](https://indyscan.io/home/SOVRIN_STAGINGNET), or as below, from the Factory agent window.

<details>
    <summary>Show me a screenshot!</summary>
    <img src="https://cdn.discordapp.com/attachments/776834563950379038/851133314614296606/SchemaId1.png">
</details>

Once you get the schema ID, copy and paste it on the Credential Issuance tab of the Company agent window, on the schema Retrieve search box.

You should have successfully retrieved the schema, you can now create a new Credential Definition. It will take less than a minute for it to appear, so just be patient.
<details>
    <summary>Show me a screenshot!</summary>
    <img src="https://cdn.discordapp.com/attachments/776834563950379038/851133330094686228/CredidentialDefinition.png">
</details>

#### 3.   The Company fills the report and sends it to the Factory

Now, issue a new credential. You have to choose the Factory as the Connection and select the credential definition you have just created.
<details>
    <summary>Show me a screenshot!</summary>
    <img src="https://cdn.discordapp.com/attachments/776834563950379038/851133344497270814/IssueCredidential.png">
</details>

#### 4.   The Factory receives the report

In the "My Credentials" tab of the Factory agent window, it can be seen that the Factory has successfully received the report from the Company.

<details>
    <summary>Show me a screenshot!</summary>
    <img src="https://cdn.discordapp.com/attachments/776834563950379038/851133358511751168/ReceivedCredidential.png">
</details>


### Stopping the Lab

To stop the Aries Toolbox, go to one of the screens and choose the top menu item “File/Quit.” In the first terminal, you will be back at the command line, and you can exit.

To stop the ACA-Py agents, go to the second terminal and:


*   Hit Ctrl-C to terminate the agents.
*   To cleanup the docker sessions run:

```
docker-compose -f docker-compose.factory-usecase.yml down

```

Exit out of the second terminal session.


## Bibliography

This demo was inspired by the Hyperledger Aries lab on [AriesToolbox](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173x/AriesToolboxLab.md).

The project was developped during the Cassiopée 2021 program runned by Télécom SudParis Institut Polytechnique de Paris.
![](https://www.telecom-sudparis.eu/wp-content/uploads/2020/09/cropped-logo2.png)

