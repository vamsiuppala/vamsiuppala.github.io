# Snowflake VS Code Extension

I did not know this was possible. Most of my dev work is split into writing Python code in VS Code and SQL in Snowflake’s Snowsight UI. Turns out you could connect to your Snowflake warehouse using a [<u>Snowflake Extension for VS Code</u>](https://docs.snowflake.com/en/user-guide/vscode-ext).

Steps to get you there:

## Fetch Account Details

Go to your Snowsight account, at the bottom left you should see an account indicator to get account information. Get the account URL from there

<img src="/images/2024-01-09-snowflake-vs-code-extension/image1.png" style="width:6.5in;height:4.02778in" />

The URL should look like this:

https://\<account-identifier\>.snowflakecomputing.com

Copy the account-identifier.

## Install Extension
Next, open VS Code and search for Snowflake in Extensions. Install it.

<img src="/images/2024-01-09-snowflake-vs-code-extension/image5.png" style="width:4.85938in;height:2.95923in" />

## Add Account Information

Open the extension and add Account Info.

<img src="/images/2024-01-09-snowflake-vs-code-extension/image2.png" style="width:2.99015in;height:3.54688in" />

Click continue and pick a way to login. I prefer single-sign on to keep the sign-in process secure and free from storing passwords.

<img src="/images/2024-01-09-snowflake-vs-code-extension/image3.png" style="width:3.81917in;height:4.16146in" />

## Use Extension!

Write your queries, save them, check history, play with the output tables, download into CSV, use CSV integration to edit results - endless possibilities.

<img src="/images/2024-01-09-snowflake-vs-code-extension/image4.png" style="width:5.21354in;height:3.33366in" />

Integrate git, make code repositories and make sure your whole team is on the same page with you with version controlling.

## Life is great

It was great after I started using VS Code for all work in Jupyter Notebooks. It just got better!