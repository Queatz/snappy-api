# Village - Public API

## Introduction

Village provides a public API.  You can use this to embed real-time, relevant information into your website.
For example, if you host an acting workshop, it may be useful to your visitors to contain a real-time list of nearby actors embedded in your site.

You need an account to use the API, and then can use the auth token generated to make additional requests.

All dates are described as strings and in the following format: `"June 30, 2009 7:03:47 AM PDT"`

## Endpoint and Authentication

The most basic call is getting information about yourself:

`http://queatz-snappy.appspot.com/api/me?auth=token`

```json
{
    "firstName": "Tara",
    "lastName": "Planas",
    "imageUrl": "https://lh3.googleusercontent.com/-g1XcJERenNY/AAAAAAAAAAI/AAAAAAAAADs/Qz35FKz9CSk/photo.jpg?sz=50",
    "auth": "286ba50c-f0bd-43d0-8d5b-3c3453a511c27c878eac-a349-4dd2-acbf-926755635e16f15e3129-fdc6-487d-bbf2-0f07a0a7446b",
    "id": "-1783730365808043612",
    "about": null,
    "infoFollowers": 0,
    "infoFollowing": 0,
    "infoHosted": 0,
    "followers": null,
    "updates": [],
    "offers": []
}
```

All API calls are structured in this way.

You can use this token for experimentation:

`ya29.GAK0vfmiAKwDf8zPm0-oRINjOSlRpwREfdiY2VWWCUx1Y8w_1DS8-LaNL7j19dPzgCGWNes`

## Getting nearby People, Meets, Locations, and Quests

#### `GET /api/here?latitude=float&longitude=float`

This will return a json object with things of interest near the provided location.

```json
{
    "parties": [],
    "people": [],
    "locations": [],
    "quests": []
}
```

See below for more information on each of these types of objects.


## Getting information about yourself

#### `GET /api/me`

Get yourself.

#### `GET /api/me/buy`

Get information on your current subscription.

#### `POST /api/me?about=string`

Update the text on your profile.

#### `POST /api/me/upto`

Add a new photo to your profile feed with a `photo` parameter in the `multipart/form-data` body and an optional `message` string parameter.

#### `POST /api/me/offers?details=string&price=number[&localId=string]`

Add a new offer to your profile.

#### `DELETE /api/me/offers/:id`

Remove an offer from your profile.

## People

#### `GET /api/people/:id`

Get information about a specific person.

#### `GET /api/people/:id/followers`

Get a person's followers.

#### `GET /api/people/:id/following`

Get people a person is following.

#### `GET /api/people/:id/parties`

Get meets hosted by a person.

#### `POST /api/people/:id?seen=true`

Mark messages from this person as seen by you.

#### `POST /api/people/:id?follow=true`

Follow this person.

#### `POST /api/people/:id?follow=false`

Stop following this person.

#### `POST /api/people/:id?message=string`

Send a message to this person.


## Meets

*Note: Parties are an alias for Meets.*

```json
{
    "name": "Betty Boop",
    "date": "October 19, 2015 2:00:00 PM UTC",
    "details": "Boop boop a doop",
    "host": {
        "firstName": "Jacob",
        "lastName": "Ferrero",
        "imageUrl": "https://lh3.googleusercontent.com/-_u9tF59VLug/AAAAAAAAAAI/AAAAAAADgIQ/OfkFt0gC2ec/photo.jpg?sz=50",
        "id": "5172910820877699443"
    },
    "location": {
        "name": "Betty's Abode",
        "latitude": 37.803898,
        "longitude": -122.44766,
        "id": "4449438823980254259"
    },
    "id": "-1209517190784309308",
    "full": false,
    "people": []
}
```

#### `POST /api/parties?...`

Query parameters:

 * `id` The id of a previous meet if hosting the same meet again
 * `name` The meet name
 * `details` The meet details
 * `date` The meet date
 * `location` The id of the meet location or json defining a new location `{ name, latitude, longitude[, address] }`
 * `localId` Optionally provide a localId that will be returned with the response object

Create a new meet.

#### `GET /api/party/:id`

Get a specific meet.

#### `POST /api/party/:id?join=true`

This is the action endpoint hit when tapping the "I'm Interested" button on a meet.

#### `POST /api/party/:id?cancel_join=true`

Remove interest in joining.

#### `POST /api/:id?full=true`

As a host, remove a meet from the public listing and stop accepting new join requests.


## Quests

```json
{
    "name": "Find",
    "details": "Cubs",
    "reward": "Cos",
    "status": "open",
    "time": "Anytime",
    "opened": "October 21, 2015 6:45:06 AM UTC",
    "teamSize": 1,
    "team": [],
    "host": {
        "firstName": "Jacob",
        "lastName": "Ferrero",
        "imageUrl": "https://lh3.googleusercontent.com/-_u9tF59VLug/AAAAAAAAAAI/AAAAAAADgIQ/OfkFt0gC2ec/photo.jpg?sz=50",
        "id": "5172910820877699443"
    },
    "id": "-1345780518205787420"
}
```

#### `GET /api/quest/:id`

Get information on a specific quest.

#### `POST /api/quest?...`

Query parameters:

* `team_size` The size of the team.  Between 1 and 6.
* `reward` The offered reward upon quest completion
* `name` The quest's name
* `details` The full details of the quest
* `time` The time of day the quest should be started. One of:
  * `"Anytime"`
  * `"Early Morning"`
  * `"Morning"`
  * `"Day"`
  * `"Evening"`
  * `"Late Evening"`
  * `"Night"`
* `localId` Optionally associate with a local optimistically created object that will be returned in the response

Open a new quest for public consumption.

#### `POST /api/quest/:id?start=true`

Start or join a quest.

#### `POST /api/quest/:id?complete=true`

As the quest poster, mark the quest as successfully completed.

#### `DELETE /api/quest/:id`

As the poster, delete a quest.


## Locations

```json
{
    "name": "Jeff's",
    "address": "1914 Pine St, San Francisco, CA 94109, USA",
    "latitude": 37.788975,
    "longitude": -122.427826,
    "id": "-4211781517460926285"
}
```

#### `GET /api/locations?latitude=float&longitude=float&name=string`

Get a list of nearby locations matching a partial string near a location.

#### `GET /api/location/:id/photo`

Get the active set photo for a location.

#### `PUT /api/location/:id/photo`

Set the active photo for a location with a `photo` parameter in the `multipart/form-data` body.


## Messages

```json
{
    "date": "October 31, 2015 8:04:38 PM UTC",
    "message": "😍",
    "from": {
        "firstName": "Jacob",
        "lastName": "Ferrero",
        "imageUrl": "https://lh3.googleusercontent.com/-_u9tF59VLug/AAAAAAAAAAI/AAAAAAADgIQ/OfkFt0gC2ec/photo.jpg?sz=50",
        "auth": "ya29.HQJ81m6oe9KlLjaou3wXNAHrF5xF74-I4isymARmhD5F4q95HHpnBR_gb0JHKrcqs1ilM70",
        "id": "5172910820877699443"
    },
    "to": {
        "firstName": "Mai",
        "lastName": "Pham",
        "imageUrl": "https://lh3.googleusercontent.com/-9nIN7iIAeaQ/AAAAAAAAAAI/AAAAAAAAeGM/X5Q5vNv0Jgc/photo.jpg?sz=50",
        "id": "8978866469173779169"
    },
    "id": "5556952325539699801"
}
```

#### `GET /api/messages`

Get a list of your recent conversations.

#### `GET /api/messages/:id`

Get a specific message, either sent to or from you.


## Joins

Joins are links between meets and people.

```json
{  
    "status":"in",
    "person":{  
        "firstName":"Jacob",
        "lastName":"Ferrero",
        "imageUrl":"https://lh4.googleusercontent.com/-l9z6DmPXgLg/AAAAAAAAAAI/AAAAAAAAACI/4KVod6SR324/photo.jpg?sz=50",
        "id":"-948149880526857821"
    },
    "party":{  
        "name":"Kristin's Bistro",
        "date":"October 22, 2015 5:00:00 PM UTC",
        "details":"French style",
        "host":{  
            "firstName":"Jacob",
            "lastName":"Ferrero",
            "imageUrl":"https://lh3.googleusercontent.com/-_u9tF59VLug/AAAAAAAAAAI/AAAAAAADgIQ/OfkFt0gC2ec/photo.jpg?sz=50",
            "auth":"ya29.HQJ81m6oe9KlLjaou3wXNAHrF5xF74-I4isymARmhD5F4q95HHpnBR_gb0JHKrcqs1ilM70",
            "id":"5172910820877699443"
        },
        "location":{  
            "name":"San Francisco",
            "latitude":37.800137,
            "longitude":-122.44262,
            "id":"-9083606478026860229"
        },
        "id":"-1729087152833550576"
    },
    "id":"2993998879583657639"
}
```

#### `GET /api/join/:id`

Returns information on a specific join object.  People join meets by tapping "I'm Interested".  The host then taps "Accept" to invite them to the meet.

#### `POST /api/join/:id?accept=true`

As a host, accept a join request.

#### `POST /api/join/:id?hide=true`

As a host, hide a join request.


# Updates

#### `GET /api/update/:id/photo`

Get the photo for an update, or `404 Not Found` if there isn't any associated photo.
