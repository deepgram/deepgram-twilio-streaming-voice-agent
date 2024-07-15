# Callable Deepgram <> Twilio Streaming Voice Agent

This is a basic server application that show cases end to end streaming voice agent

* Callable Phone Number
* Streaming Twilio - Inbound Audio
* Streaming Deepgram - Speech to Text
* Streaming OpenAI - LLM
* Streaming Deepgram - Text to Speech
* Streaming Twilio - Outbound Audio

It's a good starting point to develop your own application logic.

## App sever setup

###

```
export OPENAI_API_KEY=xxx
export DEEPGRAM_API_KEY=xxx
```

### Installation

**Requires Node >= v12.1.0**

Run `npm install`

npm dependencies (contained in the `package.json`):
* httpdispatcher
* websocket

#### Running the server

Start with `npm run start`

## Setup

You can setup your environment to run the demo by using the CLI.

## Configure using the UI
1. Install ngrok and sign up for a ngrok account 
If on MacOS: 
`brew install ngrok/ngrok/ngrok`

If on Windows/Linux: 
Follow instructions on ngrok's site: https://ngrok.com/docs/getting-started/

2. Sign up for a ngrok account 
- If you haven't already, sign up for an ngrok account: https://dashboard.ngrok.com/get-started/setup/macos 
- Copy your ngrok authtoken from your ngrok dashboard: https://dashboard.ngrok.com/get-started/your-authtoken 
- Run the following command in your terminal to install the authtoken and connect the ngrok agent to your account.
`ngrok config add-authtoken <TOKEN>`


Make sure ngrok is running so the url does not change:
* Purchase a number
* Set the webhook url to your ngrok url (eg. https://<NGROK_URL>/twiml)
* Call the number and chat to your bot

### Configure using the CLI

1. Find available phone number

```bash
twilio api:core:available-phone-numbers:local:list --country-code="US" --voice-enabled --properties="phoneNumber"`
```

2. Purchase the phone number (where `+123456789` is a number you found)

```bash
twilio api:core:incoming-phone-numbers:create --phone-number="+123456789"`
```

3. Start ngrok
On a separate terminal (not the one where you have run `npm run start`): 

```bash
ngrok http 8080
```

You will see a url under the `Forwarding`row that --> to your localhost. Copy this as the `<ngrok urk>`

4. Edit the `templates/streams` file to replace `<ngrok url>` with your ngrok host.


5. Make the call where `+123456789` is the Twilio number you bought and `+19876543210` is your phone number and `abcdef.ngrok.io` is your ngrok host.

```
twilio api:core:calls:create --from="+123456789" --to="+19876543210" --url="https://abcdef.ngrok.io/twiml"
```
