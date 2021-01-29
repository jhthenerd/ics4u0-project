# functions

This folder contains the serverless functions hosted on Firebase.

## How to call functions

The simplest way is to install the Firebase package for the platform you're developing on. Refer to Firebase's documentation for how to install the package and call functions.
https://firebase.google.com/docs/functions/callable

There are 3 functions exposed to the public:

1. getQualifications
1. getMatchingJobListings
1. getMatchingJobs

## getQualifications

getQualifications takes a single string as the data parameter, and returns an object with qualifications that include the string. This was originally intended to be used as an autocomplete function internally, but proved to be unnecessary. Thus, it was never developed further. The recommend way to use this function is to call it with the data object defined as null, to retrieve all the qualifications, and use a local autocomplete library.

### Example output

```js
output = {
  1: {
    name: "Sample Qualification 1",
    description: "The first sample qualification",
  },
  2: {
    name: "Sample Qualification 2",
    description: "The second sample qualification",
  },
  3: {
    name: "Sample Qualification 3",
    description: "The third sample qualification",
  },
}
```

## getMatchingJobListings

This function takes a data object containing a job id and an optional location object. This function uses serverside filtering to find the matching listings within a job within a certain radius.

### Example input

```js
input = {
  job: "1",
  location: {
    center: {
      latitude: 43.5598,
      longitude: -79.7164,
    },
    radius: 0,
  },
}
```

### Example output

```js
output = [
  {
    exists: true,
    id: "1",
    data: {
      coordinates: {
        _latitude: 43.5598,
        _longitude: -79.7164,
      },
      name: "Sample Job Listing 1",
      description: "The first sample job listing",
      links: ["https://jfss.ca"],
      salary: "100000",
      g: {
        geohash: "dpxrf1b61p",
        geopoint: {
          _latitude: 43.5598,
          _longitude: -79.7164,
        },
      },
    },
    distance: 0,
  },
]
```

## getMatchingJobs

This function takes in an array of qualification ids, and finds jobs that will suit the qualifications provided. The searching tool will only select a job if all the qualifications that the job requires are met.

### Example input

```js
input = { qualifications: ["1", "2"] }
```

### Example output

```js
output = {
  1: {
    name: "Sample Job 1",
    description: "The first sample job",
    avgSalary: 100000,
    qualifications: ["1"],
    qualificationsData: [
      {
        description: "The first sample qualification",
        name: "Sample Qualification 1",
      },
    ],
  },
  2: {
    name: "Sample Job 2",
    description: "The second sample job",
    avgSalary: 200000,
    qualifications: ["1", "2"],
    qualificationsData: [
      {
        description: "The first sample qualification",
        name: "Sample Qualification 1",
      },
      {
        description: "The second sample qualification",
        name: "Sample Qualification 2",
      },
    ],
  },
}
```