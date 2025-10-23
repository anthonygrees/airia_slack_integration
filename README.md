# Airia Integration with Slack
  
This guide walks you through creating a Slack app and linking it to an Airia agent to a Slack channel. The bot responds to user queries when mentioned (@) in a Slack channel.


Maintainer: Alyssa Giuliano: Solutions Consultant @ Airia.ai

https://www.loom.com/share/ee030e45e3bb4afea20d67da357cc2cb?sid=9df0a7b8-c7d0-4980-b8c6-1076a4c76863
  
---

## Creating the Slack App

### Step 1: Navigate to Your Apps
- Go to the Slack API website: [api.slack.com/apps](https://api.slack.com/apps) and log into your workspace.
- From the dashboard, click on **Your Apps** to create a new one.

![alt text](/images/image.png)

### Step 2: Create a New App
- Click on **Create New App**.
- Choose **From scratch** to configure your app manually.
- Enter a name for your app and select the workspace to develop it in.
    - Note: The app workspace cannot be changed later.

![alt text](/images/image-2.png)

### Step 3: Set Up OAuth & Permissions
- Navigate to **OAuth & Permissions**.
- Add the following OAuth Scopes:
  - `app_mentions:read`
  - `chat:write`
  - `commands`

![alt text](/images/image-4.png)

### Step 4: Define Your Webhook

- In your Airia agent interface, add a **Webhook Listener**.
- Set the listener type to **Slack**.
- Set Authentication Type to **Unauthenticated**.
- Save the webhook; you will receive a **Webhook URL**.

![alt text](/images/image-5.png)

---

### Step 5: Link Your Slack App to Your Agent Webhook Interface
- In your Slack App settings, go to **Event Subscriptions**.
- Toggle **Enable Events** on.
- Add the **Webhook URL** you got from the agent to the **Request URL** field.
- Add a subscription to **bot events**:
  - Click on **Subscribe to bot events**
  - Add the event name `app_mention`

![alt text](/images/image-6.png)

---

### Step 6: Deploy Your App to Slack
- Install your app to the Airia workspace.
- Copy the **Bot User OAuth Token** provided after installation. You will need this token in the next step.

![alt text](/images/image-7.png)

---

### Step 7: Create Airia Agent and Add Slack MCP 

#### Slack MCP

- In your Airia project, add the Slack MCP integration.
- When setting up credentials for Slack MCP, use the **Bot User OAuth Token** received in Step 6.
- This token will authenticate your agent to send messages back to Slack from the Slack App.

#### Agent Structure and Prompt

The Agent will be a model connected to input and output. 

The model should have the Slack MCP tool attached, as well as the following prompt:
```
You will receive input similar to this example JSON from Slack when the bot is mentioned:

{
  "Body": "{\"token\":\"******\",\"team_id\":\"******\",\"api_app_id\":\"******\",\"event\":{\"user\":\"******\",\"type\":\"app_mention\",\"ts\":\"******.******\",\"client_msg_id\":\"******\",\"text\":\"<@U09DEJQA0UA> what are the latest news articles today?\",\"team\":\"******\",\"blocks\":[{\"type\":\"rich_text\",\"block_id\":\"4ETeZ\",\"elements\":[{\"type\":\"rich_text_section\",\"elements\":[{\"type\":\"user\",\"user_id\":\"U09DEJQA0UA\"},{\"type\":\"text\",\"text\":\" what are the latest news articles today?\"}]}]}],\"channel\":\"******\",\"event_ts\":\"******.******\"},\"type\":\"event_callback\",\"event_id\":\"******\",\"event_time\":******,\"authorizations\":[{\"enterprise_id\":null,\"team_id\":\"******\",\"user_id\":\"******\",\"is_bot\":true,\"is_enterprise_install\":false}],\"is_ext_shared_channel\":false,\"event_context\":\"******\"}",
  "Headers": {
    "Accept":"*/*",
    "Host":"prodaus.api.airia.ai",
    "User-Agent":"Slackbot 1.0 (+https://api.slack.com/robots)",
    "Content-Type":"application/json",
    "Content-Length":"******",
    "x-slack-request-timestamp":"******",
    "x-slack-signature":"******"
  }
}

Within the body, there is a question - in this example: "what are the latest news articles today?"

Produce a response to that question, and use the Slack MCP tool to write a response using Slack ChatPostMessage
```

![alt text](</images/Screenshot.png>)
