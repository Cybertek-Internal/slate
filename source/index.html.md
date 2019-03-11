---
title: bookit api reference

toc_footers:
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
```

This endpoint retrieves a campus by location.

### HTTP Request

`GET /api/campuses/{campus_location}`

### Path Parameters

Parameter | Pattern |Description
--------- | ------- |-----------
campus_location | [A-Z]{2} | the location of a specific campus

Campus location is a state acronym in which campus is located (`VA`, `IL`).

## Get your Campus

```json
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
```

This endpoint retrieves a campus by name.

### HTTP Request

`GET /api/clusters/{cluster-name}`

### Path Parameters

Parameter | Description
--------- | ----------
cluster-name | the name of a cluster

## Get your Cluster

```json
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
{
    "id": integer,
    "name": "string",
    "description": "string",
    "capacity": integer,
    "withTV": boolean,
    "withWhiteBoard": boolean
}
```

This endpoint retrieves a room by name.

### HTTP Request

`GET /api/rooms/{room-name}`

### Path Parameters

Parameter | Description
--------- | ----------
room-name | the name of a room

## Get a Room by id

```json
{
    "id": integer,
    "name": "string",
    "description": "string",
    "capacity": integer,
    "withTV": boolean,
    "withWhiteBoard": boolean
}
```

This endpoint retrieves a room by id.

### HTTP Request

`GET /api/rooms/{id}`

### Path Parameters

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

We interpret `timeline` as a 30 minutes timeframe which has a specific `start` and `finish` times. Imagine, you want to book a room from 2:30pm to 3:30pm, you will have to block two timelines (2:30pm - 3:00pm and 3:00pm - 3:30pm) within room availability schedule.

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
```

This endpoint retrieves a timeline by id.

### HTTP Request

`GET /api/timelines/{id}`

### Path Parameters

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

Do you find any connection between [room](#rooms) and `lock`? You got it! Nobody can get into a locked room. We use `locks` for an exactly same purpose, suppose, an access to the room have to be limited to a certain batch or team at specific date and time, we can lock the room and share the key whether with a specific team or entire batch. Even possible to hide the key, so nobody can book a room!


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
}
```

This endpoint retrieves a lock by id.

### HTTP Request

`GET /api/locks/{id}`

### Path Parameters

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
every | required | most of the time you will generate repetative lock. Let's say, you want room to be locked every Tuesday, so set `every` to `tuesday`, another possible values: [`day`, `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday`, `sunday`]
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

### Path Parameters

Parameter | Description
--------- | ----------
id | The id of a lock

# Conference

Conference - group of people sit in the room and aim to look serious. We define two types of conferences: `solid` - room is booked with 100% waranty and `iffy` - room is booked without warranty, means another reservator might rebook the room with `solid` conference.

## Get all Conferences

```json
[
    {
        "id": integer,
        "type": "srting",
        "room": {
            "id": integer,
            "name": "string",
            "description": "string",
            "capacity": integer,
            "withTV": boolean,
            "withWhiteBoard": boolean
        },
        "reservator": {
            "id": integer,
            "firstName": "string",
            "lastName": "string",
            "role": "string"
        },
        "date": {
            "year": integer,
            "month": integer,
            "day": integer
        },
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
        }
    },
    ...
]
```

This endpoint retrieves all conferences.

### HTTP Request

`GET /api/conferences`

## Get a Conference by id

```json
{
    "id": integer,
    "type": "srting",
    "room": {
        "id": integer,
        "name": "string",
        "description": "string",
        "capacity": integer,
        "withTV": boolean,
        "withWhiteBoard": boolean
    },
    "reservator": {
        "id": integer,
        "firstName": "string",
        "lastName": "string",
        "role": "string"
    },
    "date": {
        "year": integer,
        "month": integer,
        "day": integer
    },
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
    }
}
```

This endpoint retrieves a conference by id.

### HTTP Request

`GET /api/conferences/{id}`

### Path Parameters

Parameter | Description
--------- | ----------
id | the id of a conference

## Get Conferences by room

```json
[
    {
        "id": integer,
        "type": "srting",
        "room": {
            "id": integer,
            "name": "string",
            "description": "string",
            "capacity": integer,
            "withTV": boolean,
            "withWhiteBoard": boolean
        },
        "reservator": {
            "id": integer,
            "firstName": "string",
            "lastName": "string",
            "role": "string"
        },
        "date": {
            "year": integer,
            "month": integer,
            "day": integer
        },
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
        }
    },
    ...
]
```

This endpoint retrieves conferences only for a specific room.

### HTTP Request

`GET /api/conferences/room`

### Query Parameters

Parameter | Demand | Description
--------- | -----------  | -----------
room-name | required | name of a room

## Get Conferences by cluster


```json
[
    {
        "id": integer,
        "type": "srting",
        "room": {
            "id": integer,
            "name": "string",
            "description": "string",
            "capacity": integer,
            "withTV": boolean,
            "withWhiteBoard": boolean
        },
        "reservator": {
            "id": integer,
            "firstName": "string",
            "lastName": "string",
            "role": "string"
        },
        "date": {
            "year": integer,
            "month": integer,
            "day": integer
        },
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
        }
    },
    ...
]
```

This endpoint retrieves conferences only for a specific [cluster](#cluster).

### HTTP Request

`GET /api/conferences/cluster`

### Query Parameters

Parameter | Demand | Description
--------- | -----------  | -----------
cluster-name | required | name of a cluster

## Get my Conferences

```json
[
    {
        "id": integer,
        "type": "srting",
        "room": {
            "id": integer,
            "name": "string",
            "description": "string",
            "capacity": integer,
            "withTV": boolean,
            "withWhiteBoard": boolean
        },
        "reservator": {
            "id": integer,
            "firstName": "string",
            "lastName": "string",
            "role": "string"
        },
        "date": {
            "year": integer,
            "month": integer,
            "day": integer
        },
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
        }
    },
    ...
]
```

This endpoint retrieves your conferences.

### HTTP Request

`GET /api/conferences/my`

## Book a Conference

```json
{
    "id": integer,
    "type": "srting",
    "room": {
        "id": integer,
        "name": "string",
        "description": "string",
        "capacity": integer,
        "withTV": boolean,
        "withWhiteBoard": boolean
    },
    "reservator": {
        "id": integer,
        "firstName": "string",
        "lastName": "string",
        "role": "string"
    },
    "date": {
        "year": integer,
        "month": integer,
        "day": integer
    },
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
    }
}
```

This endpoint creates new conference.

### HTTP Request

`POST /api/conferences/conference`

### Query Parameters

Parameter | Demand  |Description
--------- | -----------  | -----------
conference-type | required | type of a conference [`solid`, `iffy`]
room-id | required | id of a room which will be booked for a conference
year | required | conference year (four digit number, ex. `2020`)
month | required | conference month [`1`-`12`]
day | required | conference day [`1`-`31`]
timeline-id | required | use timeline id to specify [timeline](#timeline) for a conference
notify-team | required | [true, false] Do you want send out emails to your teamembers with a new conference notification?

<aside class="notice">
You might want to book a room for a more then a 30 minutes (one timeline), in that case include all timeline ids in you request
<code>/api/locks/lock?...timeline-id=8&timeline-id=9...</code>
</aside>

## Cancel a Conference by id

This endpoint removes a conference by id.

### HTTP Request

`DELETE /api/conferences/{id}`

### Path Parameters

Parameter | Description
--------- | ----------
id | the id of a conference

# Batch

## Get all Batches

```json
[
    {
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
    },
    ...
]
```

This endpoint retrieves all batches.

### HTTP Request

`GET /api/batches`

## Get a Batch by number

```json
{
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
```

This endpoint retrieves a batch by batch number.

### HTTP Request

`GET /api/batches/{batch-number}`

### Path Parameters

Parameter | Description
--------- | ----------
batch-number | number of the batch

## Get my Batch

```json
{
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
```

This endpoint retrieves your batch.

### HTTP Request

`GET /api/batches/my`

## Add a Batch

```json
batch {batch-number} has been successfully added to the database
```

This endpoint creates brand new batch.

### HTTP Request

`POST /api/batches/batch`

### Query Parameters

Parameter | Demand | Description
--------- | -------- | ----------
batch-number | required | number of a new batch

## Gratuate a Batch

Mark whole batch as graduated. Note, student after graduation is entirely new person with different set of abbilities. For instance, VA student is able to book a room on the [dark side](#cluster) after graduation only.

```json
batch {batch-number} has been successfully graduated
```

### HTTP Request

`PUT /api/batches/{batch-number}/graduate`

### Path Parameters

Parameter | Demand | Description
--------- | -------- | ----------
batch-number | required | number of a new batch

# Team

## Get all Teams

```json
[
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
```

This endpoint retrieves all teams.

### HTTP Request

`GET /api/teams`

## Get a Team by id

```json
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
}
```

This endpoint retrieves a team by id.

### HTTP Request

`GET /api/teams/{id}`

## Get my Team

```json
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
}
```

This endpoint retrieves your team.

### HTTP Request

`GET /api/teams/my`

## Add a Team

```json
team {team-name} has been added to the batch {batch-number}
```

This endpoint creates new team.

### HTTP Request

`POST /api/teams/team`

### Query Parameters

Parameter | Demand | Description
--------- | -------- | ----------
campus-location | required | name of the campus which team will be added to
batch-number | required | number of the batch which team will be added to
team-name | required | name of the team, should be uniq per campus

# User

## Get all Users

```json
[
    {
        "id": integer,
        "firstName": "string",
        "lastName": "string",
        "role": "string"
    },
    ...
]
```

This endpoint retrieves all users.

### HTTP Request

`GET /api/users`

## Get a User by id

```json
{
    "id": integer,
    "firstName": "string",
    "lastName": "string",
    "role": "string"
}
```

This endpoint retrieves a user by id.

### HTTP Request

`GET /api/users/{id}`

### Path Parameters

Parameter | Demand | Description
--------- | -------- | ----------
id | required | id of the user

## Get Me

```json
{
    "id": integer,
    "firstName": "string",
    "lastName": "string",
    "role": "string"
}
```

This endpoint retrieves you.

### HTTP Request

`GET /api/users/me`

# Student

Most probably you. We group students into two categories, the one who is permitted to book the room and the one who is not, which are `student-team-leader` and `student-team-member` respectfully.

## Get all Students

```json
[
    {
        "id": integer,
        "firstName": "string",
        "lastName": "string",
        "role": "string"
    },
    ...
]
```

This endpoint retrieves all students.

### HTTP Request

`GET /api/students`

## Get a Student by id

```json
{
    "id": integer,
    "firstName": "string",
    "lastName": "string",
    "role": "string"
}
```

This endpoint retrieves a student by id.

### HTTP Request

`GET /api/students/{id}`

### Path Parameters

Parameter | Demand | Description
--------- | -------- | ----------
id | required | id of the student

## Get Me

```json
{
    "id": integer,
    "firstName": "string",
    "lastName": "string",
    "role": "string"
}
```

This endpoint retrieves you (if you're student).

### HTTP Request

`GET /api/students/me`

## Add a Student

```json
user {student-id} has been added to the database
```

This endpoint creates new student.

### HTTP Request

`POST /api/students/student`

### Query Parameters

Parameter | Demand | Description
--------- | -------- | ----------
first-name | required | first name of the student
last-name | required | last name of the student
email | required | email of the student, will be used for an [authentication](#authentication)
password | required | password of the account, will be used for an [authentication](#authentication)
role | required | role of the student, [[`student-team-leader`, `student-team-member`]](#student)
campus-location | required | name of the campus which student will be added to
batch-number | required | number of the batch which student will be added to
team-name | required | name of the team which student will be added to

## Update a Student

```json
user {student-id} has been updated
```

This endpoint updates a student.

### HTTP Request

`PUT /api/students/{id}`

### Path Parameters

Parameter | Demand | Description
--------- | -------- | ----------
id | required | id of the student

### Query Parameters

Parameter | Demand | Description
--------- | -------- | ----------
first-name | optional | first name of the student
last-name | required | last name of the student
email | required | email of the student, will be used for an [authentication](#authentication)
password | required | password of the account, will be used for an [authentication](#authentication)
role | required | role of the student, [[`student-team-leader`, `student-team-member`]](#student)

## Exclude a Student

```json
user {student-id} has been deleted
```

This endpoint removes a student.

### HTTP Request

`DELETE /api/students/{id}`

### Path Parameters

Parameter | Demand | Description
--------- | -------- | ----------
id | required | id of the student

# Teacher

## Get all Teachers

```json
[
    {
        "id": integer,
        "firstName": "string",
        "lastName": "string",
        "role": "string"
    },
    ...
]
```

This endpoint retrieves all teachers.

### HTTP Request

`GET /api/teachers`

## Get a Teacher by id

```json
{
    "id": integer,
    "firstName": "string",
    "lastName": "string",
    "role": "string"
}
```

This endpoint retrieves a teacher by id.

### HTTP Request

`GET /api/teachers/{id}`

### Path Parameters

Parameter | Demand | Description
--------- | -------- | ----------
id | required | id of the teacher

## Get Me

```json
{
    "id": integer,
    "firstName": "string",
    "lastName": "string",
    "role": "string"
}
```

This endpoint retrieves you (if you're teacher). 

### HTTP Request

`GET /api/teachers/me`

## Add a Teacher

```json
user {teacher-id} has been added to the database
```

This endpoint creates new teacher.

### HTTP Request

`POST /api/teachers/teacher`

### Query Parameters

Parameter | Demand | Description
--------- | -------- | ----------
first-name | required | first name of the teacher
last-name | required | last name of the teacher
email | required | email of the teacher, will be used for an [authentication](#authentication)
password | required | password of the account, will be used for an [authentication](#authentication)
campus-location | required | name of the campus which teacher will be added to

## Update a Teacher

```json
user {teacher-id} has been updated
```

This endpoint updates a teacher.

### HTTP Request

`PUT /api/teachers/{id}`

### Path Parameters

Parameter | Demand | Description
--------- | -------- | ----------
id | required | id of the teacher

### Query Parameters

Parameter | Demand | Description
--------- | -------- | ----------
first-name | optional | first name of the teacher
last-name | required | last name of the teacher
email | required | email of the teacher, will be used for an [authentication](#authentication)
password | required | password of the account, will be for an [authentication](#authentication)
campus-location | required | name of the campus which teacher belongs to


# Schedule Preferences

There are a set of defined rules to regulate a room reservation process. Schedule preferences is a part of regulatory configuration which contains a list of dates and timelines when room reservation is allowed.
## Get a Schedule Preferences

```json
{
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
    ]
}
```

This endpoint retrieves a schedule preferences.

### HTTP Request

`GET /api/schedule/preferences`



