# Mpesa Callback Listener

A listener for MPesa post requests from Safaricom's gateway API.

The app uses [CloudFunctions for Firebase](https://firebase.google.com/docs/functions/) to expose a publicly accessible endpoint, stores the transaction details in [Firebase Realtime Database](https://firebase.google.com/docs/database/) and sends a noticiation to a user's device using [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/).

Related Android app: [LNMOnlineAndroidSample](https://github.com/MarkNjunge/LNMOnlineAndroidSample).

## Installation

```
git clone
cd MpesaCallbackHandler
yarn install
```

## Testing locally

Serve using  
`firebase serve --only functions`

Firebase will give you an address for the endpoints. For example,  
`http://localhost:5000/mpesahandler/us-central1/callback_url`

To make the endpoints reachable outside your network, you wil need to use a http tunneling client such as [Ngrok](https://ngrok.com/).

### Using Ngrok

Expose port 5000 (Firebase's default port) using  
`Ngrok http 5000`

Ngrok will give you an address such as  
`http://f2ca75f0.ngrok.io`

The resulting url for the endpoint will therefore be  
`http://f2ca75f0.ngrok.io/mpesahandler/us-central1/callback_url`

## Deploy to Firebase

Run `firebase init functions`, choosing not to overwrite existing files.

Deploy using  
`firebase deploy --only functions`

## Endpoints

### /callback_url/:tokenId

Receives a callback for Lipa na MPesa transactions.  
It stores the transaction details in the Realtime Database under `LNM/success` for successful transactions and `LNM/failed` for failed transactions.

#### Url Parameters

| Name    | Required | Description                                                                                                               |
| ------- | -------- | ------------------------------------------------------------------------------------------------------------------------- |
| tokenId | Yes      | A Firebase Cloud messaging token from a device. It is used to send a notification with the transaction result to the user. |