# gve_devnet_report_issue_macro
This is an update to the report-issue webex Macro created by William Mills. This update allows users to send the issues to a list of webex emails. The emails are seperated into 3 catagories: Mailer1, Mailer2, Mailer3.

**Mailer1**: will be notified when "Technical Issue with Incoming Audio/Video" Issues are submitted
**Mailer2**: will be notified when "Can't connect to my meeting" Issues are submitted
**Mailer3**: will be notified when "Request for a technician" Issues are submitted


## Contacts
* Charles Llewellyn

## Solution Components
* Webex
*  Webex Room Devices
*  Javascript


## Prerequisites
- **Webex API Personal Token**:
1. To use the Webex REST API, you need a Webex account backed by Cisco Webex Common Identity (CI). If you already have a Webex account, you're all set. Otherwise, go ahead and [sign up for a Webex account](https://cart.webex.com/sign-up).
2. When making request to the Webex REST API, the request must contain a header that includes the access token. A personal access token can be obtained [here](https://developer.webex.com/docs/getting-started).

> Note: This token has a short lifetime - only 12 hours after logging into this site - so it shouldn't be used outside of app development.

- **Webex Bot**: To create a Webex bot, you need a token from Webex for Developers.
1. Log in to `developer.webex.com`
2. Click on your avatar and select `My Webex Apps`
3. Click `Create a New App`
4. Click `Create a Bot` to start the wizard
5. Following the instructions of the wizard, provide your bot's name, username, and icon
6. Once the form is filled out, click `Add Bot` and you will be given an access token
7. Copy the access token and store it safely. Please note that the API key will be shown only once for security purposes. In case you lose the key, then you have to revoke the key and generate a new key

> For more information about Webex Bots, please see the [documentation](https://developer.webex.com/docs/bots)

> [This blog](https://developer.webex.com/blog/from-zero-to-webex-teams-chatbot-in-15-minutes) gives more step by step instructions.


## Installation/Configuration
Download the report-issue.js and GMM_LIB.js file and upload them to your Webex Room device.
Configure the Report Issue Macro by changing the initial values, there are comments explaining each one.
```
const config = {
  webexBotToken: 'EXAMPLE_BOT_TOKEN', // WEBEX BOT TOKEN (learn more: https://developer.webex.com/bots)
  name: 'Report Issue',          // Name of the Button and Panel
  submitText: 'Submit Issue',       // Text displays on the submit button
  waitingText: 'Sending Feedback',
  showAlert: true,                  // Show success and error alerts while true. One waiting alert is shown while false
  allowInsecureHTTPs: true,         // Allow insecure HTTPS connections to the instant connect broker for testing
  panelId: 'feedback',
  start: {
    options: [
      'Technical Issue with Incoming Audio/Video',
      'Technical Issue with Outgoing Audio/Video',
      'Can\'t connect to my meeting',
      'Request for a technician',
    ]
  },
  form: {
    category: {
      type: {
        Text: {
          prefix: '',
          options: 'size=2;fontSize=normal;align=left'
        },
        Button: {
          name: ['Select Category', 'Change Category'],
          options: 'size=2'
        }
      },
      action: 'Options',
      placeholder: 'eg. Please select category',
      promptText: 'Please enter the problem description',         // Text input message
      inputType: 'SingleLine',   // Type of input field. SingleLine = alphanum, other options (Numeric, Password, PIN)
      showPlaceholder: true,
      visiable: true,     // True = field is visable | False = field is removed from UI
      modifiable: true   // If false, placeholder will be used always
    },
    name: {
      requires: ['category'],
      type: {
        Text: {
          prefix: 'Name:',
          options: 'size=2;fontSize=normal;align=left'
        },
        Button: {
          name: ['Enter Name', 'Change Name'],
          options: 'size=2'
        }
      },
      action: 'TextInput',
      placeholder: 'eg. John Smith (optional)',
      promptText: 'Please enter your name',
      inputType: 'SingleLine',
      showPlaceholder: true,
      visiable: true,
      modifiable: true
    },
    submit: {
      requires: ['category'],
      visiable: true,
      modifiable: true,
      action: 'Submit',
      value: 'active',
      type: {
        Button: {
          name: ['Submit Issue'],
          options: 'size=2'
        }
      }
    }
  }
}
/*********************************************************
 * Create Webex Mailer Groups
**********************************************************/
const myUserGroup1 = ["example@email.com", "examle2@email.com"];
const myUserGroup2 = ["example3@email.com", "example@email.com"];
const myUserGroup3 = ["example3@email.com", "example4@email.com"];
const mailer1 = new GMM.Message.Webex.User(config.webexBotToken, myUserGroup1) // Technical Issue with Incoming/outgoing Audio/Video Mailer
const mailer2 = new GMM.Message.Webex.User(config.webexBotToken, myUserGroup2) // Can't connect to my meeting Mailer
const mailer3 = new GMM.Message.Webex.User(config.webexBotToken, myUserGroup3) // Request for a technician Mailer

```

Enable the Macro.

## Usage
Install this report-issue.js Macro into your codec:

More details on how to install Macros for the [Board, Desk, and Room Series](https://help.webex.com/en-us/article/gj962f/Configure-macros-and-user-interface-extensions-for-Board,-Desk,-and-Room-Series)

# Screenshots

![/IMAGES/report-issue-gif.gif](/IMAGES/report-issue-gif.gif)
![/IMAGES/0image.png](/IMAGES/0image.png)

### LICENSE

Provided under Cisco Sample Code License, for details see [LICENSE](LICENSE.md)

### CODE_OF_CONDUCT

Our code of conduct is available [here](CODE_OF_CONDUCT.md)

### CONTRIBUTING

See our contributing guidelines [here](CONTRIBUTING.md)

#### DISCLAIMER:
<b>Please note:</b> This script is meant for demo purposes only. All tools/ scripts in this repo are released for use "AS IS" without any warranties of any kind, including, but not limited to their installation, use, or performance. Any use of these scripts and tools is at your own risk. There is no guarantee that they have been through thorough testing in a comparable environment and we are not responsible for any damage or data loss incurred with their use.
You are responsible for reviewing and testing any scripts you run thoroughly before use in any non-testing environment.
