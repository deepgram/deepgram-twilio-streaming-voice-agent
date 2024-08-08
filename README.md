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


### Set environment variables

 You will need to set environment variables for your shell session. These environment variables are used to store the API keys required for authentication when accessing the OpenAI and Deepgram APIs.

 Open a Terminal and run:

```sh
export OPENAI_API_KEY=xxx
export DEEPGRAM_API_KEY=xxx
```

To verify the environment variables are set, run the following commands in your Terminal:

```sh
echo $OPENAI_API_KEY
echo $DEEPGRAM_API_KEY
```

### Installation

**Requires Node >= v12.1.0**

Run `npm install`

npm dependencies (contained in the `package.json`):
* httpdispatcher
* websocket

#### Running the server

Start with `npm run start`

## Demo Setup

### Configure the environment using the Ngrok UI & CLI

1. Install ngrok:
- If on MacOS: `brew install ngrok/ngrok/ngrok`
- If on Windows/Linux: Follow the [instructions on ngrok's site](https://ngrok.com/docs/getting-started/)

2. Sign up for a ngrok account:
- If you haven't already, sign up for an [ngrok account](https://dashboard.ngrok.com/get-started/setup/macos)
- Copy your ngrok authtoken from your [ngrok dashboard](https://dashboard.ngrok.com/get-started/your-authtoken)
- Run the following command in your terminal to install the auth token and connect the ngrok agent to your account.

```sh
ngrok config add-authtoken <TOKEN>
```

### Twilio Phone Number Setup

- You will need to provide a valid phone number from Twilio. Please see these [instructions](https://help.twilio.com/articles/223135247-How-to-Search-for-and-Buy-a-Twilio-Phone-Number-from-Console) to do so.

### Configure with the Twilio CLI

> Refer to this [Repository](https://github.com/twilio/media-streams/tree/master/node/connect-basic) for more information on this section.

1. Install the [Twilio CLI](https://www.twilio.com/docs/twilio-cli/quickstart) and login to Twilio to run these commands.

2. Find an available phone number

```sh
twilio api:core:available-phone-numbers:local:list --country-code="US" --voice-enabled --properties="phoneNumber"`
```

3. Purchase the phone number (where `+123456789` is a number you found)

```sh
twilio api:core:incoming-phone-numbers:create --phone-number="+123456789"`
```

### Set the webhook url to your ngrok url

4. Start ngrok

On a separate terminal (not the one where you have run `npm run start`):

```sh
ngrok http 8080
```
> If you restart your ngrok server your `ngrok url` will change.

You will see a url under the `Forwarding`row that --> to your localhost. Copy this as the `<ngrok url>`

5. Edit the [templates/streams](templates/streams.xml) file to replace `<ngrok url>` with your ngrok host. Example: `wss://abcdef.ngrok.io/streams`. Remember to use `wss://` and include `/streams` in the url 

6. Go to your Twilio page where you manage your phone number. Under the Configure tab, replace the webhook URL where the call comes in to your ngrok url. Example: `https://abcdef.ngrok-free.app/twiml` . Remember to use `https://` and include `/twiml`

### Call the number and chat to your bot.

7. You can call your Twilio phone number directly. Alternatively, make the call with the following, where `+123456789` is the Twilio number you bought and `+19876543210` is your phone number and `abcdef.ngrok.io` is your ngrok host.

```sh
twilio api:core:calls:create --from="+123456789" --to="+19876543210" --url="https://abcdef.ngrok.io/twiml"
```
