# Use pymsteams to keep track of your metrics through Teams

## Why track metrics?

Usually businesses keep an eye out on metrics / KPIs using dashboards that they are surfaced to. When it comes to important metrics, the teams that create these metrics have to regularly check the dashboards, or other backend srevices that output these metrics on a regular basis for monitoring.
For a Data Team that owns some KPIs, it gets tedious to keep an eye out on them through multiple dashboards, frequently, and 24x7. Dashboards are meant for business users. They are never flexible / deep enough for a data scientist behind the data. It pretty soon gets tedious to log into / refresh dashboards frequently 24x7. 

## Monitor KPIs through Teams

It is easier for data teams to be served the predictions through a more accessible channel - Teams. Using the [pymsteams](https://pypi.org/project/pymsteams/) package lets us automatically send an alert in Teams through webhooks whenever the monitored KPIs see a noticebale shift (defined by business / data team). Since it is an open source python library, it is easy to integrate into any automated production processes.

## Tools

Steps to get started with using Teams to track a metric:

#### Install pymsteams

Get started by installing the package using [pip](https://pypi.org/project/pymsteams/). The instructions are straightforward.

#### Setup Teams Incoming Webhook

An [Incoming Webhook](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook?tabs=dotnet) lets external applications share content in Microsoft Teams channels.

1.  Get your Teams Connectors webhook for the channel you setup to track the metric(s)

<img src="/images/2024-01-04-predictions-pymsteams/image1.png" style="width:1.08205in;height:2.13115in" />

2.  Configure an Incoming Webhook<img src="/images/2024-01-04-predictions-pymsteams/image2.png" style="width:6.5in;height:0.69167in" />

3.  Give it a name and create the webhook

<img src="/images/2024-01-04-predictions-pymsteams/image3.png" style="width:4.43181in;height:4.2784in" />

4.  Copy the URL it creates and save it to use it later.

#### Code to send a simple Teams Alert

<!-- <img src="/images/2024-01-04-predictions-pymsteams/image4.png" style="width:6.5in;height:3.85833in" /> -->

```python
import pymsteams
def get_current_and_previous_values():

    # write the code to fetch the two values from your upstream process
    previous_val = 200
    current_val = 300

    return previous_val, current_val

# create the function to send a Teams connect card as an alert
def teams_alert_content (previous_val, current_val, url):
    # Create webhook
    myTeamsMessage = pymsteams.connectorcard(url)
    # Create Text
    myTeamsMessage.title("Value Change Alert")
    myTeamsMessage.text("Value has changed from $" + str(previous_val) + " to $" + str(current_val))
    myTeamsMessage.send()

#copy the webhook url here from Teams
webhook_url = "<webhook-url-from-teams-connectors>"

#send the alert in Teams
previous_value, current_value = get_current_and_previous_values()
teams_alert_content(previous_value, current_value, webhook_url)
```

## Result

When I first deployed this simple example code into a lengthy Azure AI run pipeline job, it started to dump the exact changes I'd wanted to monitor in KPIs directly into a Teams Channel dedicated for monitoring KPIs. It significantly reduced the stress of having to monitor KPIs actively through various dashboards.

#### Teams alert content example

<img src="/images/2024-01-04-predictions-pymsteams/image5.png" style="width:6.5in;height:1.88472in" />