README

# Good Day Slack Bot

Good Day is a Slack bot that pings users every day and asks how their day was. It saves the results in a GitHub repository of the user's choice, within a `good-day.csv` file in the repo.

<img src="assets/form.png" alt="drawing" width="500"/>

It also provides a series of visualizations to help users understand their data over time. For a preview of what this looks like check out: [https://github.com/githubocto/good-day-demo](https://github.com/githubocto/good-day-demo)

<img src="assets/visualization.png" alt="drawing" width="500"/>

## How it Works

<img src="assets/flow.png" alt="drawing"/>

### Slack App Express server

The repo contains the code for the core Good Day Slack bot, which is a Slack server that performs a few functions:

1. Stores a user's GitHub repo and time preference in a database by collecting info from the Slack app home panel.
2. Messages a user every day on the time they have specified with a new Good Day form.
3. Stores user's daily data into a `good-day.csv` file in the repo of their choice.
4. Messages a user every week when new visualization charts have been generated from their data.

### Azure functions

This Slack Bot depends on two Azure functions: [https://github.com/githubocto/good-day-azure](https://github.com/githubocto/good-day-azure)

1. **Timed Notify**: This function runs every hour, checks the database for users who need a new form, and hits an endpoint on the express server to trigger a new Good Day form for users daily. 
2. **Generate Charts**: This function runs every week and generates visualization charts for users in their GitHub repos. It also hits an endpoint on the express server to notify users new charts are ready.

## Development

### Local setup

1. Install ngrok and authenticate

2. Create a `.env` file with:

```
GH_API_KEY=
SLACK_SIGNING_SECRET=
SLACK_BOT_TOKEN=
PG_CONN_STRING=
```

3. Start the server and ngrok

`yarn install`

`yarn dev`

In a new tab: `ngrok http 3000 --hostname octo-devex.ngrok.io`

### Slack app configuration

1. Change the endpoint for interactive messages at: [interactive messages config](https://api.slack.com/apps/A0212TEULJU/interactive-messages?) to `https://octo-devex.ngrok.io/interactive`

2. Change the endpoint for events at: [events config](https://api.slack.com/apps/A0212TEULJU/event-subscriptions?) to `https://octo-devex.ngrok.io/events`

## Building / Releasing

### Deployment

Deployment to the production app happens automatically when pushing to main by using a GitHub Action specified in `.github/workflows/main_octo-good-day-bot.yaml`.

### Slack app configuration

1. Change the endpoint for interactive messages at: [interactive messages config](https://api.slack.com/apps/A0212TEULJU/interactive-messages?) to `PRODUCTION_URL/interactive`

2. Change the endpoint for events at: [events config](https://api.slack.com/apps/A0212TEULJU/event-subscriptions?) to `PRODUCTION_URL/events`

## License

[MIT](LICENSE)
