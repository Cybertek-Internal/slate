---
title: bookit api reference

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the bookit api reference! You can use our api to access bookit endpoints, which can get information on rooms, conferences, students and more in our database.

# Authentication


We operate on the JWT [RFC 7519](https://tools.ietf.org/html/rfc7519). Sipmly put, we expect for the authorization token to be included in all api requests to the server in a header that looks like the following:

`Authorization: Bearer your-token`

<aside class="notice">
Make sure to replace <code>your-token</code>  with your personal authorization token.
</aside> 

## Get Sign

This endpoint returns newly generated autorization token for provided email and password.

```json
{
    "accessToken": "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiI4MiIsImF1ZCI6InN0dWRlbnQtdGVhbS1tZW1iZXIifQ.zIcFXhVng5REMvXmUGrJRSPMp87ImMqxVoM6ofeDpZA",
    "refreshToken": "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiI4MiIsImF1ZCI6InN0dWRlbnQtdGVhbS1tZW1iZXIifQ.zIcFXhVng5REMvXmUGrJRSPMp87ImMqxVoM6ofeDpZA"
}
```

### HTTP Request

`GET /api/sign`

### Query Parameters

Parameter | Demand | Description
--------- | -----------  | -----------
email | required | your email address
password | required | corresponding password



# Campus

## Get all Campuses

```json
[
    {
        "id": integer,
        "location": "string",
        "clusters": [
            {
                "id": integer,
                "name": "string",
                "rooms": [
                    {
                        "id": integer,
                        "name": "string",
                        "description": "string",
                        "capacity": integer,
                        "withTV": boolean,
                        "withWhiteBoard": boolean
                    },
                    ...
                ]
            },
            ...
        ]
    },
    ...
]
```

This endpoint retrieves all campuses with enclosed cluster and room information.

### HTTP Request

`GET /api/campuses`


## Get a Campus by location

```json
[
    {
        "id": integer,
        "location": "string",
        "clusters": [
            {
                "id": integer,
                "name": "string",
                "rooms": [
                    {
                        "id": integer,
                        "name": "string",
                        "description": "string",
                        "capacity": integer,
                        "withTV": boolean,
                        "withWhiteBoard": boolean
                    },
                    ...
                ]
            },
            ...
        ]
    }
]
```

This endpoint retrieves a campus by location.

### HTTP Request

`GET /api/campuses/{campus_location}`

### URL Parameters

Parameter | Pattern |Description
--------- | ------- |-----------
campus_location | [A-Z]{2} | the location of a specific campus

Campus location is a state acronym in which campus is located (`VA`, `IL`).

## Get your Campus

```json
[
    {
        "id": integer,
        "location": "string",
        "clusters": [
            {
                "id": integer,
                "name": "string",
                "rooms": [
                    {
                        "id": integer,
                        "name": "string",
                        "description": "string",
                        "capacity": integer,
                        "withTV": boolean,
                        "withWhiteBoard": boolean
                    },
                    ...
                ]
            },
            ...
        ]
    }
]
```

This endpoint retrieves your campus.

### HTTP Request

`GET /api/campuses/my`

# Cluster

We define `cluster` to be a separate room collection within a campus. For instance `VA` campus clustered on the two room collections: `light-side` and `dark-side`, but `IL` campus has only one `il` cluster.

## Get all Clusters

```json
[
    {
        "id": integer,
        "name": "string",
        "rooms": [
            {
                "id": integer,
                "name": "string",
                "description": "string",
                "capacity": integer,
                "withTV": boolean,
                "withWhiteBoard": boolean
            },
            ...
        ]
    },
    ...
]
```

This endpoint retrieves all clusters with enclosed room information.

### HTTP Request

`GET /api/clusters`


## Get a Cluster by name

```json
[
    {
        "id": integer,
        "name": "string",
        "rooms": [
            {
                "id": integer,
                "name": "string",
                "description": "string",
                "capacity": integer,
                "withTV": boolean,
                "withWhiteBoard": boolean
            },
            ...
        ]
    }
]
```

This endpoint retrieves a campus by name.

### HTTP Request

`GET /api/clusters/{cluster_name}`

### URL Parameters

Parameter | Description
--------- | ----------
cluster_name | the name of a cluster

## Get your Cluster

```json
[
    {
        "id": integer,
        "name": "string",
        "rooms": [
            {
                "id": integer,
                "name": "string",
                "description": "string",
                "capacity": integer,
                "withTV": boolean,
                "withWhiteBoard": boolean
            },
            ...
        ]
    }
]
```

This endpoint retrieves your campus.

### HTTP Request

`GET /api/clusters/my`

# Room

## Get all Rooms

```json
[
    {
        "id": integer,
        "name": "string",
        "description": "string",
        "capacity": integer,
        "withTV": boolean,
        "withWhiteBoard": boolean
    },
   ...
]
```

This endpoint retrieves all rooms.

### HTTP Request

`GET /api/rooms`


## Get a Room by name

```json
[
    {
        "id": integer,
        "name": "string",
        "description": "string",
        "capacity": integer,
        "withTV": boolean,
        "withWhiteBoard": boolean
    }
]
```

This endpoint retrieves a room by name.

### HTTP Request

`GET /api/rooms/{room_name}`

### URL Parameters

Parameter | Description
--------- | ----------
room_name | the name of a room

## Get a Room by id

```json
[
    {
        "id": integer,
        "name": "string",
        "description": "string",
        "capacity": integer,
        "withTV": boolean,
        "withWhiteBoard": boolean
    }
]
```

This endpoint retrieves a room by id.

### HTTP Request

`GET /api/rooms/{id}`

### URL Parameters

Parameter | Description
--------- | ----------
id | The id of a room

## Get available Rooms

```json
[
    {
        "id": integer,
        "name": "string",
        "description": "string",
        "capacity": integer,
        "withTV": boolean,
        "withWhiteBoard": boolean
    },
    ...
]
```

This endpoint retrieves a list of all available rooms. Suppose, you want to book a room for a conference, so you have to find out what rooms are available for defined conference type at specific date and time within your cluster.

### HTTP Request

`GET /api/rooms/available`

### Query Parameters

Parameter | Demand | Description
--------- | -------- | ----------
year | required | the year of a potential conference
month | required | the month of a potential conference
day | required |the day of a potential conference
timeline-id | required | the id of a [timeline](#timelines) of a potential conference. You might want to look up a rooms availability for a more then 
cluster-name | required | the name of a cluster in which you are look for an available rooms
conference-type | required | type of a conference 

<aside class="notice">
You might want to look up a rooms availability for a more then a 30 minutes (one timeline), in that case include all timeline ids in you request
<code>/api/rooms/available?...timeline-id=8&timeline-id=9...</code>
</aside>

# Timeline

We interpret `timeline` as a 30 minutes timeframe which has a specific `start` time and `finish` times. Imagine, you want to book a room from 2:30pm to 3:30pm, you will have to block two timelines (2:30pm - 3:00pm and 3:00pm - 3:30pm) within room availability schedule.

## Get all Timelines

```json
[
  
    {
        "id": integer,
        "start": {
            "hour": integer,
            "minute": integer,
            "second": integer,
            "nano": integer
        },
        "finish": {
            "hour": integer,
            "minute": integer,
            "second": integer,
            "nano": integer
        }
    },
   ...
]
```

This endpoint retrieves all timelines.

### HTTP Request

`GET /api/timelines`


## Get a Timeline by id

```json
[
  
    {
        "id": integer,
        "start": {
            "hour": integer,
            "minute": integer,
            "second": integer,
            "nano": integer
        },
        "finish": {
            "hour": integer,
            "minute": integer,
            "second": integer,
            "nano": integer
        }
    }
]
```

This endpoint retrieves a timeline by id.

### HTTP Request

`GET /api/timelines/{id}`

### URL Parameters

Parameter | Description
--------- | ----------
id | the id of a room

## Get your Timelines counts

Timelines counts is an amount of timelines you've utilized for a conference booking today. Let's say you booked `2 hour` within a room schedule for an `iffy` conference today. Means your `iffy` timelines counter should be equal `4`.  

```json
{
    "solid": integer,
    "iffy": integer
} 
```

This endpoint retrieves your timeline counts.

### HTTP Request

`GET /api/timelines/counts/my`

# Lock

Do you find any connection between [room](#rooms) and `lock`? You got it! Nobody can get into a locked room. We use `locks` for an exactly same purpose, suppose, an access to the room have to be limited to a certain batch or team at specific date and time, we can lock the room and share a key whether with a team or batch. Even hide the key, so nobody can book a room!


## Get all Locks

```json
[
    {
        "id": integer,
        "room": {
            "id": integer,
            "name": "stiring",
            "description": "string",
            "capacity": integer,
            "withTV": boolean,
            "withWhiteBoard": boolean
        },
        "dates": [
            {
                "year": integer,
                "month": integer,
                "day": integer
            },
            ...
        ],
        "timelines": [
            {
                "id": integer,
                "start": {
                    "hour": integer,
                    "minute": integer,
                    "second": integer,
                    "nano": integer
                },
                "finish": {
                    "hour": integer,
                    "minute": integer,
                    "second": integer,
                    "nano": integer
                }
            },
            ...
        ],
        // depending on the lock type, response will include whether batch or team object or neither
        "team": {
            "id": integer,
            "name": "string",
            "members": [
                {
                    "id": integer,
                    "firstName": "string",
                    "lastName": "string",
                    "role": "string"
                },
                ...
            ]
        },
        "batch": {
            "number": integer,
            "isGraduated": boolean,
            "teams": [
                {
                    "id": integer,
                    "name": "string",
                    "members": [
                        {
                            "id": integer,
                            "firstName": "string",
                            "lastName": "string",
                            "role": "string"
                        },
                        ...
                    ]
                },
                ...
            ]
        }
    },
    ...
]
```

This endpoint retrieves all locks.

### HTTP Request

`GET /api/locks`



## Get a Lock by id

```json
[
    {
        "id": integer,
        "room": {
            "id": integer,
            "name": "stiring",
            "description": "string",
            "capacity": integer,
            "withTV": boolean,
            "withWhiteBoard": boolean
        },
        "dates": [
            {
                "year": integer,
                "month": integer,
                "day": integer
            },
            ...
        ],
        "timelines": [
            {
                "id": integer,
                "start": {
                    "hour": integer,
                    "minute": integer,
                    "second": integer,
                    "nano": integer
                },
                "finish": {
                    "hour": integer,
                    "minute": integer,
                    "second": integer,
                    "nano": integer
                }
            },
            ...
        ],
        // depending on the lock type, response will include whether batch or team object or neither
        "team": {
            "id": integer,
            "name": "string",
            "members": [
                {
                    "id": integer,
                    "firstName": "string",
                    "lastName": "string",
                    "role": "string"
                },
                ...
            ]
        },
        "batch": {
            "number": integer,
            "isGraduated": boolean,
            "teams": [
                {
                    "id": integer,
                    "name": "string",
                    "members": [
                        {
                            "id": integer,
                            "firstName": "string",
                            "lastName": "string",
                            "role": "string"
                        },
                        ...
                    ]
                },
                ...
            ]
        }
    },
    ...
]
```

This endpoint retrieves a lock by id.

### HTTP Request

`GET /api/locks/{id}`

### URL Parameters

Parameter | Description
--------- | ----------
id | the id of a lock


## Add a Lock


```json
there is no key to unlock #{lock-id}. {room-name} is secured
```

This endpoint generates a new lock.

### HTTP Request

`POST /api/locks/lock`

### Query Parameters

Parameter | Demand | Description
--------- | -----------  | -----------
room-name | required | name of a room which you plan to lock
timeline-id | required | id of a timeline when room won't be accessible
every | required | most of the time you will generate repetative lock. Let's say, you want room to be locked every Tuesday, so set `every` to `tuesday.` [`day`, `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday`, `sunday`]
end-year | required | end year of a repetative lock. Works together with `every`, repetative locks will be generated until date set via `end-year`, `end-month`, `end-day`
end-month | required | end month of a repetative lock.
end-day | required | end day of a repetative lock.
batch-number | optional | the only batch which will have access to the room
team-name | optional | the only team which will have access to the room

<aside class="notice">
You might want to lock a room for a more then a 30 minutes (one timeline), in that case include all timeline ids in you request
<code>/api/locks/lock?...timeline-id=8&timeline-id=9...</code>
</aside>

### Response Message Parameters

Parameter | Description
--------- | ----------
id | The id of a new lock
room-name | The name of a locked room

## Delete a Lock by id

This endpoint removes a lock by id.

### HTTP Request

`DELETE /api/locks/{id}`

### URL Parameters

Parameter | Description
--------- | ----------
id | The id of a lock

# Conference

## Get all Conferences

## Get a Conference by id

## Get Conferences by room

## Get Conferences by cluster

## Get my Conferences

## Book a Conference

## Cancel a Conference

# Batch

## Get all Batches

## Get a Batch by number

## Get my Batch

## Add a Batch

## Gratuate a Batch

# Team

## Get all Teams

## Get a Team by id

## Get my Team

## Add a Team

# User

## Get all Users

## Get a User by id

## Get Me

# Student

## Get all Students

## Get a Student by id

## Get Me

## Add a Student

## Update a Student

## Exclude a Student

# Teacher

## Get all Teachers

## Get a Teacher by id

## Get Me

## Add a Teacher

## Update a Teacher

# Schedule Preferences

## Get a Schedule Preferences



