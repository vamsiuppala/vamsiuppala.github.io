# Use pymsteams to keep track of your metrics through Teams

## Why track metrics?

My team at ServiceNow builds and tracks predictions / forecasts for some of the key revenue metrics - new business, renewal subscriptions, new logos signed up, customer retention etc. Given the importance and the need for an often-updated forecast, these predictions are updated hourly through a job deployed through Azure AI, with the predicted metrics being written into a table in Snwoflake, consumed through Tableau dashboards. The later in the quarter we are, the more important the metric prediction gets, attracting lots of eyes and lots of questions.

Monitoring a KPI prediction is similar to monitoring an ever changing KPI. A business user might rely on dashboards for this as needed, but as the backend builders, we found it tedious to log into / refresh dashboards frequently 24x7. 

## Eureka

We realized that it would be easier for us to be served the predictions through a more accessible channel - Teams, and went around looking for ways to automatically send an alert in Teams through webhooks in case of a big change in the KPI / prediction. The pymsteams package does this efficiently just in two lines of code.

## Tools

Steps to get started with using Teams to track a metric:

### Install pymsteams

Since my workplace uses Microsoft Teams, we relied on the pymsteams package. It’s a Python Wrapper Library to send requests to Microsoft Teams Webhooks.

We got started by installing the package using [pip](https://pypi.org/project/pymsteams/). The instructions were straightforward.

### Setup Teams Incoming Webhook

An [Incoming Webhook](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook?tabs=dotnet) lets external applications share content in Microsoft Teams channels.

1.  Get your Teams Connectors webhook for the channel you setup to track the metric(s)

<img src="/images/2024-01-04-predictions-pymsteams/image1.png" style="width:1.08205in;height:2.13115in" />

2.  Configure an Incoming Webhook<img src="/images/2024-01-04-predictions-pymsteams/image2.png" style="width:6.5in;height:0.69167in" />

3.  Give it a name and create the webhook

<img src="/images/2024-01-04-predictions-pymsteams/image3.png" style="width:4.43181in;height:4.2784in" />

4.  Copy the URL it creates and save it to use it later.

### Code to send a simple Teams Alert

<img src="/images/2024-01-04-predictions-pymsteams/image4.png" style="width:6.5in;height:3.85833in" />

```python

import pymsteams
def get_current_and_previous_predictions():

    # write the code to fetch these two from your database or prediction scoring run process
    previous prediction = 200
    current prediction = 300

    return previous_prediction, current_prediction

# create the function to send teams alert
def teams_alert_content (previous_pred, current_pred, url):
    # Create webhook
    myTeamsMessage = pymsteams. connectorcard (url)
    # Create Text
    myTeamsMessage.title("Prediction Change Notification")
    myTeamsMessage.text("Prediction has changed from $" + str(previous_pred) + " to $" + str(current_pred))
    myTeamsMessage.send()

#copy the webhook url here from Teams
webhook_url = "<webhook-url-from-teams-connectors>"

#send the alert in Teams
previous prediction, current prediction = get_current_and_previous_predictions()
teams_alert_content (previous_prediction, current_prediction, webhook_url)

```

## Result

We first deployed this simple example code into a length production-run process to automatically dump the previous and latest predictions into a Teams Channel dedicated to tracking the most important prediction owned by my Team.

### Teams alert content

<img src="/images/2024-01-04-predictions-pymsteams/image5.png" style="width:6.5in;height:1.88472in" />
