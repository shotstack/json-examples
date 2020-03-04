# Shotstack JSON examples

### A collection of JSON templates and examples demonstrating common techniques and features of the Shotstack platform.

Shotstack is video automation platform for creating video centric applications. The core of the platform is a highly 
scalable, hosted API that can edit, process, manipulate and personalise 1000's of videos concurrently.

The RESTful API uses JSON to describe the timeline of a video edit which can be posted to the `render` endpoint which then takes care of delivering a rendered mp4 video file.

This is a collection of JSON examples to help users get familiar with the system and demonstrate basic video editing concepts and features of the system.

## Requirements

Before using these examples you will need he following:

- A Shotstack account and API key, register via the [website](https://shotstack.io).
- [Curl](https://curl.haxx.se/), [Postman](https://www.getpostman.com/) or another application or command line tool for making RESTful API requests

Note: This readme provides examples using Curl

## Usage

To run the examples we will provide instructions using Curl. You could easily use an application like Postman or any aother application or tool that can post JSON formatted data to an API endpoint.

This guide assumes Curl is already installed on your environment and you have a basic understanding of posting requests to an API. Even if you are familiar with workiing with API's it is worthwhile checking our [gettng started guide](https://shotstack.gitbook.io/docs/guides/getting-started) before using these examples.

### Post Render Task

To queue and render a video example:

```
curl -X POST \
     -H "Content-Type: application/json" \
     -H "x-api-key: YOUR_KEY_HERE" \
     -d @examples/ken-burn-effect.json \
     https://api.shotstack.io/stage/render
```

- In the example above replace `YOUR_KEY_HERE` with your the staging key provided to you during registration.

- `@examples/ken-burn-effect.json` is the example JSON file from the examples folder you wish to render. See the [Examples](#Examples) table below for a list of examples.

- Notice that we are using the stage endpoint `https://api.shotstack.io/stage/render` which is free for development and testing.


#### Response

If the POST is succesful you will receive a response similar to:

```
{
   "success":true,
   "message":"Created",
   "response":{
      "message":"Render Successfully Queued",
      "id":"d2b46ed6-998a-4d6b-9d91-b8cf0193a655"
   }
}
```

Take a note of the response id `d2b46ed6-998a-4d6b-9d91-b8cf0193a655` which we will use in the next step.

### Status Check

Video rendering takes time, usually several seconds per second of video, so a 30 second video might take 30 seconds to one minute to complete.

To check the status of  render task:

```
curl -X GET \
     -H "Content-Type: application/json" \
     -H "x-api-key: YOUR_KEY_HERE" \
     https://api.shotstack.io/stage/render/d2b46ed6-998a-4d6b-9d91-b8cf0193a655
```

- Replace `YOUR_KEY_HERE` with your staging environment key.

- Notice we have appended the response id to the end of the request URL `https://api.shotstack.io/stage/render/d2b46ed6-998a-4d6b-9d91-b8cf0193a655`; you will need to make sure you use the id returned from the Post Render Task step.

#### Response

You can query the render endpoint above until your video has finished rendering. The response you can expect to see is:

```
{
   "success":true,
   "message":"OK",
   "response":{
      "id":"d2b46ed6-998a-4d6b-9d91-b8cf0193a655",
      "owner":"hckwccw3q3",
      "plan":"sandbox",
      "status":"done",
      "url":"https://s3-ap-southeast-2.amazonaws.com/shotstack-api-stage-output/hckwccw3q3/d2b46ed6-998a-4d6b-9d91-b8cf0193a655.mp4",
      "data":{
         "output":{
            ...
         },
         "timeline":{
            ...
         }
      },
      "created":"2019-04-16T12:02:42.148Z",
      "updated":"2019-04-16T12:02:51.867Z"
   }
}
```

- While the video is rendering the status will be set to `rendering` and there will be no `url` parameter.

- When complete the status will be set to `done` and a url to an mp4 (or gif) file will be available.

- Copy and paste the URL to your browser to preview the video or download via Curl, wget or via your browser.

## Examples

| Example File              | Description | Video |
| ---------------------- | ------------- | ---- |
| [ken-burns-effect.json](./examples/ken-burns-effect.json) | Animate static images using zooming and panning, also know as the Ken Burns effect. | [Preview](https://youtu.be/3OTv1AGwmYM)
| [picture-in-picture.json](./examples/picture-in-picture.json) | Picture in picture demo using layered tracks, clip position and scale. | [Preview](https://youtu.be/qCRNYEwSdDo)
| [overlay-transition.json](./examples/overlay-transition.json) | Use QuickTime mov with alpha transparency to create cool transitions. | [Preview](https://youtu.be/TYacZ9gnoRA)




