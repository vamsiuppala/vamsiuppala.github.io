## Why track predictions?

One of these important forecasts my team at ServiceNow owns is how much new business the company will likely end up bringing in the next four quarters. Given the importance and the need for an often-updated forecast, we predict these metrics each hour, with the most important one being overall new business dollars we might make in the current quarter. The later in the quarter we are, the more important the metric gets, attracting more eyes and more questions.

Given its importance, we had to track the metric movement every hour 24x7 and report on big swings. This gets incredibly annoying if it needs to be tracked on a dashboard that has to be logged into through your work machine – resulting in having to carry it around everywhere.

The solution we came up with is to put some of these metrics programmatically on an access-restricted Teams channel. Once we started sending the top metrics to the channel every hour at the end of the production job run, we did not need to keep a track of them directly through slowly refreshing dashboards built for different purposes. It got as easy as checking our Teams for the latest messages on the phone; only this channel started to contain all the important metric notifications. What’s even better is that all the messages made up for a nice history to rely on easily.

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

## Result

We first deployed this simple example code into a length production-run process to automatically dump the previous and latest predictions into a Teams Channel dedicated to tracking the most important prediction owned by my Team.

### Teams alert content

<img src="/images/2024-01-04-predictions-pymsteams/image5.png" style="width:6.5in;height:1.88472in" />
