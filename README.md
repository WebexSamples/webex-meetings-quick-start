# Webex Javascript SDK Meetings Quick Start

This sample application demonstrates the basic usage of the Webex Javascript SDK.
It allows you to create a Webex meeting using the meetings plugin.

## Setup

### Clone the repo to your local machine

```bash
git clone git@github.com:WebexSamples/webex-meetings-quick-start.git
```

### Install Dependencies

```bash
yarn install
```

### Add Personal Access Token

For this example, we'll use your personal access token which can be found in [Getting Started](https://developer.webex.com/docs/api/getting-started#accounts-and-authentication) if you're logged in. This token provides access to your account for testing purposes, and shouldn't be used for anything other than testing. If you don't already have a Webex Teams account, click Sign Up at the top of this page to create a new account.

Open the `index.js` file and add this code to authenticate with Webex Teams, replacing `YOUR_ACCESS_TOKEN` with your development token.

### Start Server

In this example we'll use [Parcel](https://parceljs.org/) to build and serve our web app, including the SDK.

Let's serve the web app with Parcel and start a meeting in the browser!

```bash
yarn start
```

Open the app ([http://localhost:1234](http://localhost:1234/)) in your browser to use your new web app! 
Enter the Person ID or email address of who you want to start a meeting with and click the `start meeting` button. 
Congratulations, you've made your first call in the browser using the Webex Browser SDK!

To see the full example code we have used here to start a meeting, check out the [sample app in GitHub](https://github.com/webex/webex-js-sdk/tree/master/packages/node_modules/samples/browser-single-party-call).
