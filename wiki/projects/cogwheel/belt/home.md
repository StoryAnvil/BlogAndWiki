# Cogwheel Engine Belt
Cogwheel Engine has feature called `belt protocol`. Belt protocol can be used to edit CogScript files remotly using external editors.

This page is talking about techincal usage of the protocol.

## Connecting
Cogwheel Engine Belt Protocol or CEBP for short built on top of http/https.

To connect your CEBP server to Cogwheel Engine use `@storyanvil cogwheel-belt start belt://example.com` chat command. *(You can replace `example.com` with any IP or domain. Also you can specify port. Using `belt` or `sbelt` url scheme is mandotory)*

#### belt url scheme
You could notice URL in example command starting with `belt://` scheme. This is used by CEBP to make sure this URL is supposted to be connected via CEBP. Using this sheme will make Cogwheel Engine use `http://` protocol.

#### sbelt url scheme
You can use `sbelt://` instead of `belt://` to make Cogwheel Engine use `https://` instead of `http://`

### Handshake
First step that Cogwheel Engine makes while connecting to CEBP server is a handshake.

Your server should expect handshakes at any time as HTTP GET request on `/~belt` endpoint.

Server must reponse with valid json. In case of succsesful connection:
```json
{
    "successful": true,
    "server": "<insert you CEBP server name here>",
    "token" : "<Random token provided by your CEBP server>"
}
```

Or if handshake was unsuccsesful server should return JSON like this:
```json
{
    "successful": false,
    "message": "<insert any message explaining why handshake failed>"
}
```
After unsuccsesful handshake, Cogwheel Engine will stop sending any future requests to this server util `@storyanvil cogwheel-belt start` with this server's address used again.

CEBP server can verify that handshake was sent by Cogwheel Engine by checking `User-Agent` and `X-StoryAnvil` headers because they always will be `Cogwheel-Engine` for requests send to CEBP server by Cogwheel Engine.

If all requests to your server after handshake, Cogwheel Engine will add `X-StoryAnvil-Secret` with provided token. So you can verifiy that only this one Cogwheel Engine instance sents requests to you.

## Player Auth
To authoirize and authenicate players you should required player to type `/@storyanvil cogwheel-belt getLink` in minecraft chat so they can get link to auth on your CEBP server.

This link will lead to your servers `/~auth/<URL Base64 of Player Username>` get endpoint

When user visits this link your server should generate some kind of one-time auth code and prompt user to run `/@storyanvil cogwheel-belt auth <you code>` in minecraft.

#### Reciving one-time codes from Cogwheel Engine to CEBP
When some player will run `/@storyanvil cogwheel-belt auth <code>` you will recieve `AUTH_CODE` type update package (see about packages below).


# Update packages
Cogwheel Engine will constantly send you POST requests at `/~belt/update` endpoint in following format:
```json
{
    "list": [ // this list can contain any amount of objects in it
        {
            "type": "<SOME TYPE>",
            "data": [] // any amount of strings
        }
    ]
}
```

Each of objects in the json `list` property called `update package`. Here's quick list of type of update packages:

| Package Type | Description | Data Array Values | Side |
|--------------|-------------|-------------------|------|
| AUTH_CODE    | Sent when player runs `/@storyanvil cogwheel-belt auth` | ["<auth code>", "<player name>"] | `CG --> CEBP` |
| RUN_COMMAND  | Used to run chat commands on Cogwheel Engine server side | ["<command without />"] | `CG <-- CEBP` |

## You also can send update your own packages to Cogwheel engine
To do this return json on requests on POST requests at `/~belt/update` in following format:
```json
{
    "list": [ // this list can contain any amount of objects in it
        {
            "type": "<SOME TYPE>",
            "data": [] // any amount of strings
        }
    ]
}
```