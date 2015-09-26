# oEmbed API

------

## oEmbed API Description

------

### Definition
Base: https://embedkit.com/api/v1/oembed/

Our Embed API offers a simple API that allows a website to display embedded content (such as photos or videos) when a user gives a link to that resource, without having to parse the resource directly. This API can handle any link you give us, and we’ll statistically get the best information. Unlike competitors, we return embed codes over SSL by default.


There are three goals of the oEmbed API endpoint:

- It will return a simple and consistent JSON response for any url
- It will always return secure (SSL) HTML5 embeds
- It will follow the oEmbed specification


Here are all the things you need to know to get going:

- Requests can be made using either GET or POST
- Your API key is required in every request (`api_key`)
- A URL is required in every call (`url`)
- You will receive either a response type of link, rich, video, or photo.
- You can access this endpoint via GET or POST (JSON encoded or form encoded).

------

### oEmbed Authentication & API Keys

Other than sending `api_key` as a parameter on every request, it is important to note that every single teammate on your account gets a separate API key. This was done by design to allow you to manage the permissions of API keys. By removing a teammate from your account, you will have disabled access to the API from their key.

In conclusion: the dashboard shows your personal API key to you. For most use cases, this is a really good thing.

------

### oEmbed Error Handling

There are two ways to check for an error:
- A non `200` response code
- A JSON response containing `"object":"error"`

------


### Multiple URLs / Batching Requests

Batching calls is extremely simple, just include multiple URLs in the `url` parameter separated by commas.

##### Request

```
{
  "api_key": "SECRET",
  "url": "http://www.pinterest.com/pin/455004368576205909/,https://twitter.com/snellingio/status/537298127628566528"
}
```

##### Response

```
[
    {
        "type": "rich",
        "version": "1.0",
        "title": "Neat Stuff",
        "provider_name": "Pinterest",
        "thumbnail_url": "https://s-media-cache-ak0.pinimg.com/736x/66/7b/ea/667beafde8a6e70f19c7d5342b1a0022.jpg",
        "thumbnail_width": 736,
        "thumbnail_height": 986,
        "html": "<a data-pin-do=\"embedPin\"\n   href=\"https://www.pinterest.com/pin/455004368576205909/\"\n   \n   \n   \n></a>\n<script type=\"text/javascript\" src=\"//assets.pinterest.com/js/pinit.js\"></script>"
    },
    {
        "type": "rich",
        "version": "1.0",
        "title": "Sam Snelling on  : \"I am fairly certain that cats are aliens.\"",
        "author": "Sam Snelling",
        "author_url": "https://twitter.com/snellingio",
        "provider_name": "Twitter",
        "html": "<blockquote class=\"twitter-tweet\"><p>I am fairly certain that cats are aliens.</p>&mdash; Sam Snelling (@snellingio) <a href=\"https://twitter.com/snellingio/status/537298127628566528\">November 25, 2014</a></blockquote>\n<script async src=\"//platform.twitter.com/widgets.js\" charset=\"utf-8\"></script>"
    }
]
```

------

## oEmbed API Responses

------

### 200 OK

```
{
  "type": "The resource type",
  "version": "The oEmbed version number (1.0)",
  "title": "A title describing the resource",
  "author_name": "The author of the resource",
  "author_url": "A URL for the author of the resource",
  "provider_name": "The name of the resource provider",
  "provider_url": "The url of the resource provider",
  "cache_age": "The suggested cache lifetime for this resource (in seconds)",
  "thumbnail_url": "A URL to a thumbnail image for the resource",
  "thumbnail_width": "The width of the thumbnail",
  "thumbnail_height": "The height of the thumbnail",
  "url": "A URL to the image embed (type : photo)",
  "html": "The HTML embed (type : video, rich)",
  "width": "The width of the embed (type: photo, video, rich)",
  "height": "The height of the embed (type: photo, video, rich)"
}
```

------

### 401 Invalid API Key

```
{
    "object":"error",
    "details":"Unfortunately the API key is invalid"
}
```

------

### 400 Invalid URL

```
{
    "object":"error",
    "details":"Unfortunately the URL is invalid"
}
```

------

### 408 URL Timeout

```
{
    "object":"error",
    "details":"Unfortunately the URL timed out"
}
```

------

## oEmbed API Example Requests

------

### CURL

```
curl --get --include "https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>" \
  -H "Accept: application/json"
```

------

### Java

```
// These code snippets use an open-source library. http://unirest.io/java
HttpResponse<JsonNode> response = Unirest.get("https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>")
.header("Accept", "application/json")
.asJson();
```

------

### Node && io.js

```
// These code snippets use an open-source library. http://unirest.io/nodejs
unirest.get("https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>")
.header("Accept", "application/json")
.end(function (result) {
  console.log(result.status, result.headers, result.body);
});
```

------

### PHP

```
// These code snippets use an open-source library. http://unirest.io/php
$response = Unirest\Request::get("https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>",
  array(
    "Accept" => "application/json"
  )
);
```

------

### Python

```
# These code snippets use an open-source library. http://unirest.io/python
response = unirest.get("https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>",
  headers={
    "Accept": "application/json"
  }
)
```

------

### Objective-C

```
// These code snippets use an open-source library. http://unirest.io/objective-c
NSDictionary *headers = @{@"Accept": @"application/json"};
UNIUrlConnection *asyncConnection = [[UNIRest get:^(UNISimpleRequest *request) {
  [request setUrl:@"https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>"];
  [request setHeaders:headers];
}] asJsonAsync:^(UNIHTTPJsonResponse *response, NSError *error) {
  NSInteger code = response.code;
  NSDictionary *responseHeaders = response.headers;
  UNIJsonNode *body = response.body;
  NSData *rawBody = response.rawBody;
}];
```

------

### Ruby

```
# These code snippets use an open-source library. http://unirest.io/ruby
response = Unirest.get "https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>",
  headers:{
    "Accept" => "application/json"
  }
```

------

### .Net

```
// These code snippets use an open-source library. http://unirest.io/net
Task<HttpResponse<MyClass>> response = Unirest.get("https://embedkit.com/api/v1/embed?url=<required>&api_key=<SECRET>")
.header("Accept", "application/json")
.asJson();
```

------

## oEmbed API Response Types

------

### Embed - type : Link

The link type response allows a provider to return any generic embed data (such as `title` and `author_name`), without providing either the ~~url~~ or ~~html~~ parameters. This API endpoint gracefully degrades.

------

#### Wordpress (No Degradation)

##### Request

```
{
  "api_key": "SECRET",
  "url": "https://wordpress.org/news/2014/10/wcsf-livestream/"
}
```

##### Response

```
{
    "type": "link",
    "version": "1.0",
    "title": "Watch WordCamp San Francisco Livestream",
    "author": "WordPress",
    "author_url": "https://wordpress.org/news/author/nbachiyski/",
    "provider_name": "WordPress News",
    "thumbnail_url": "//i0.wp.com/wordpress.com/i/blank.jpg",
    "thumbnail_width": 200,
    "thumbnail_height": 200,
    "url": "https://wordpress.org/news/2014/10/wcsf-livestream/"
}
```

------

#### Yahoo.com (Partial Degradation)

##### Request

```
{
  "api_key": "SECRET",
  "url": "http://www.yahoo.com"
}
```

##### Response

```
{
    "type": "link",
    "version": "1.0",
    "title": "Yahoo",
    "provider_name": "Yahoo",
    "thumbnail_url": "https://s.yimg.com/dh/ap/default/130909/y_200_a.png",
    "thumbnail_width": 200,
    "thumbnail_height": 200,
    "url": "https://www.yahoo.com/"
}
```

------

#### Hacker News Homepage (Full Degradation)

##### Request

```
{
  "api_key": "SECRET",
  "url": "https://news.ycombinator.com/"
}
```

##### Response

```
{
    "type": "link",
    "version": "1.0",
    "title": "Hacker News",
    "url": "https://news.ycombinator.com/"
}
```

------

### Embed - type : Photo

The photo type response will potentially return the additional following parameters: `url`, `width`, and `height`.

------

#### Flickr

##### Request

```
{
  "api_key": "SECRET",
  "url": "https://www.flickr.com/photos/latitudes/15749776960/"
}
```

##### Response

```
{
    "type": "photo",
    "version": "1.0",
    "title": "Frosty Forest",
    "author": "Todd Klassy",
    "author_url": "https://www.flickr.com/photos/latitudes/",
    "provider_name": "Flickr",
    "thumbnail_url": "//farm8.staticflickr.com/7572/15749776960_61e59069fc_z.jpg",
    "thumbnail_width": 640,
    "thumbnail_height": 427,
    "width": 1024,
    "height": 683,
    "url": "//farm8.staticflickr.com/7572/15749776960_61e59069fc_b.jpg"
}
```

------

#### Imgur

##### Request

```
{
  "api_key": "SECRET",
  "url": "http://imgur.com/gallery/bCmyCXT"
}
```

##### Response

```
{
    "type": "photo",
    "version": "1.0",
    "title": "MRW I see new Star Wars and Jurassic Park trailers, and a new AC/DC album coming out",
    "author": "Death13",
    "author_url": "http://imgur.com/user/Death13",
    "provider_name": "Imgur",
    "thumbnail_url": "//i.imgur.com/bCmyCXT.jpg",
    "thumbnail_width": 666,
    "thumbnail_height": 570,
    "width": 666,
    "height": 570,
    "url": "//i.imgur.com/bCmyCXT.jpg"
}
```

------

### Embed - type : Video

The video type response will potentially return the additional following parameters: `html`, `width`, and `height`.

------

#### Youtube

##### Request

```
{
  "api_key": "SECRET",
  "url": "https://www.youtube.com/watch?v=9bZkp7q19f0"
}
```

##### Response

```
{
    "type": "video",
    "version": "1.0",
    "title": "PSY - GANGNAM STYLE (강남스타일) M/V",
    "author": "officialpsy",
    "provider_name": "YouTube",
    "thumbnail_url": "https://i.ytimg.com/vi/9bZkp7q19f0/maxresdefault.jpg",
    "thumbnail_width": 1280,
    "thumbnail_height": 720,
    "html": "<div style=\"left: 0px; width: 100%; height: 0px; position: relative; padding-bottom: 56.2493%;\"><iframe src=\"https://www.youtube.com/embed/9bZkp7q19f0?rel=0&#x26;showinfo=1\" frameborder=\"0\" allowfullscreen=\"true\" webkitallowfullscreen=\"true\" mozallowfullscreen=\"true\" style=\"top: 0px; left: 0px; width: 100%; height: 100%; position: absolute;\"></iframe></div>"
}
```

------

#### Vimeo

##### Request

```
{
  "api_key": "SECRET",
  "url": "http://vimeo.com/112306285"
}
```

##### Response

```
{
    "type": "video",
    "version": "1.0",
    "title": "RIVAGES",
    "author": "SOCIAL ANIMALS",
    "author_url": "http://vimeo.com/socialanimals1",
    "provider_name": "Vimeo",
    "thumbnail_url": "http://i.vimeocdn.com/video/498531268_1280.jpg",
    "thumbnail_width": 1280,
    "thumbnail_height": 720,
    "html": "<div style=\"left: 0px; width: 100%; height: 0px; position: relative; padding-bottom: 56.2493%;\"><iframe src=\"//player.vimeo.com/video/112306285?byline=0&#x26;badge=0\" frameborder=\"0\" allowfullscreen=\"true\" webkitallowfullscreen=\"true\" mozallowfullscreen=\"true\" style=\"top: 0px; left: 0px; width: 100%; height: 100%; position: absolute;\"></iframe></div>"
}
```

------

### Embed - type : Rich

The rich type response will potentially return the additional following parameters: `html`, `width`, and `height`.

------

#### Twitter

##### Request

```
{
  "api_key": "SECRET",
  "url": "https://twitter.com/snellingio/status/537298127628566528"
}
```

##### Response

```
{
    "type": "rich",
    "version": "1.0",
    "title": "Sam Snelling on  : \"I am fairly certain that cats are aliens.\"",
    "author": "Sam Snelling",
    "author_url": "https://twitter.com/snellingio",
    "provider_name": "Twitter",
    "html": "<blockquote class=\"twitter-tweet\"><p>I am fairly certain that cats are aliens.</p>&mdash; Sam Snelling (@snellingio) <a href=\"https://twitter.com/snellingio/status/537298127628566528\">November 25, 2014</a></blockquote>\n<script async src=\"//platform.twitter.com/widgets.js\" charset=\"utf-8\"></script>"
}
```

------

#### Pinterest

##### Request

```
{
  "api_key": "SECRET",
  "url": "http://www.pinterest.com/pin/455004368576205909/"
}
```

##### Response

```
{
    "type": "rich",
    "version": "1.0",
    "title": "Neat Stuff",
    "provider_name": "Pinterest",
    "thumbnail_url": "https://s-media-cache-ak0.pinimg.com/736x/66/7b/ea/667beafde8a6e70f19c7d5342b1a0022.jpg",
    "thumbnail_width": 736,
    "thumbnail_height": 986,
    "html": "<a data-pin-do=\"embedPin\"\n   href=\"https://www.pinterest.com/pin/455004368576205909/\"\n   \n   \n   \n></a>\n<script type=\"text/javascript\" src=\"//assets.pinterest.com/js/pinit.js\"></script>"
}
```

------

# Article API

------

## Article API Description

------

### Definition
https://embedkit.com/api/v1/article

There is one goal of the article extraction API endpoint:

It will return a simple and consistent JSON response for any url while extracting as much information as possible
Here are all the things you need to know to get going:

Requests can be made using either GET or POST
Your API key is required in every request (`api_key`)
A URL is required in every call (`url`)
You can access this endpoint via GET or POST (JSON encoded or form encoded).

Multiple URLs / Batching Requests
To batch requests include multiple URLs in the url parameter separated by commas.

------

### Article Authentication & API Keys

Other than sending `api_key` as a parameter on every request, it is important to note that every single teammate on your account gets a separate API key. This was done by design to allow you to manage the permissions of API keys. By removing a teammate from your account, you will have disabled access to the API from their key.

In conclusion: the dashboard shows your personal API key to you. For most use cases, this is a really good thing.

------

### Article Error Handling

There are two ways to check for an error:
- A non `200` response code
- A JSON response containing `"object":"error"`

------

## Article API Responses

------

### 200 OK

```
{
    "meta": {
        "property": "Value of meta property",
    },
    "links": {
        "image": [
            {
                "href": "Link to image",
                "type": "Image format",
                "media": {
                    "width": "Width of image",
                    "height": "Height of image"
                }
            }
        ],
        "player": [
            {
                "href": "Link to player",
                "type": "Player format",
                "media": {
                    "aspect-ratio": "Aspect ratio of the player"
                },
                "html": "HTML embed of the player"
            }
        ],
        "thumbnail": [
            {
                "href": "Link To Image",
                "type": "Image Format",
                "media": {
                    "width": "Width of image",
                    "height": "Height of image"
                }
            }
        ],
        "icon": [
            {
                "href": "Link to icon image",
                "type": "Image format"
            }
        ]
    },
    "context": {
        "meta": {
            "property": "Value of meta property"
        },
        "oembed": {
            "property": "Value of oEmbed property"
        }
    },
    "html": "HTML of embed",
    "content": "Full text extracted from the page"
}
```

------

### 401 Invalid API Key

```
{
    "object":"error",
    "details":"Unfortunately the API key is invalid"
}
```

------

### 400 Invalid URL

```
{
    "object":"error",
    "details":"Unfortunately the URL is invalid"
}
```

------

### 408 URL Timeout

```
{
    "object":"error",
    "details":"Unfortunately the URL timed out"
}
```

------

## Article API Example Requests

------

### CURL

```
curl --get --include "https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>" \
  -H "Accept: application/json"
```

------

### Java

```
// These code snippets use an open-source library. http://unirest.io/java
HttpResponse<JsonNode> response = Unirest.get("https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>")
.header("Accept", "application/json")
.asJson();
```

------

### Node.js / io.js

```
// These code snippets use an open-source library. http://unirest.io/nodejs
unirest.get("https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>")
.header("Accept", "application/json")
.end(function (result) {
  console.log(result.status, result.headers, result.body);
});
```

------

### PHP

```
// These code snippets use an open-source library. http://unirest.io/php
$response = Unirest\Request::get("https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>",
  array(
    "Accept" => "application/json"
  )
);
```

------

### Python

```
# These code snippets use an open-source library. http://unirest.io/python
response = unirest.get("https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>",
  headers={
    "Accept": "application/json"
  }
)
```

------

### Objective-C

```
// These code snippets use an open-source library. http://unirest.io/objective-c
NSDictionary *headers = @{@"Accept": @"application/json"};
UNIUrlConnection *asyncConnection = [[UNIRest get:^(UNISimpleRequest *request) {
  [request setUrl:@"https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>"];
  [request setHeaders:headers];
}] asJsonAsync:^(UNIHTTPJsonResponse *response, NSError *error) {
  NSInteger code = response.code;
  NSDictionary *responseHeaders = response.headers;
  UNIJsonNode *body = response.body;
  NSData *rawBody = response.rawBody;
}];
```

------

### Ruby

```
# These code snippets use an open-source library. http://unirest.io/ruby
response = Unirest.get "https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>",
  headers:{
    "Accept" => "application/json"
  }
```

------

### .Net

```
// These code snippets use an open-source library. http://unirest.io/net
Task<HttpResponse<MyClass>> response = Unirest.get("https://embedkit.com/api/v1/article?url=<required>&api_key=<SECRET>")
.header("Accept", "application/json")
.asJson();
```

------

# Embeddable Widgets

------

## Embeddable Widgets Description

To switch from using Embed.ly Cards to EmbedKit Widgets, just replace your old javascript file with the new EmbedKit one:

##### Old Embed.ly Javascript

```
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>
```

##### New EmbedKit Javascript

```
<script async src="//embedkit.com/hosted/embedkit.card.js" charset="UTF-8"></script>
```

EmbedKit will automatically detect any Embed.ly Cards and replace them with better EmbedKit Widgets.

------

## What does this mean for your API?
Switching from Embed.ly should be as easy as switching the Embed.ly host in your code from `api.embed.ly` to `embedkit.com/api/`. Find and replace is your friend :)

By using advanced route forwarding, parameter matching, and detection of your old Embed.ly assets, switching to EmbedKit will be incredibly easy to use your existing codebase, while benefiting from EmbedKit’s advanced platform.

------

#Switching From Embed.ly

Switching from Embed.ly is quick and easy!

------

## API Compatibility
EmbedKit was designed in to fit with your existing toolset, while providing additional functionality. Below is how Embed.ly corresponds EmbedKit.

| Embed.ly Endpoint | EmbedKit Endpoint |
|---------------------------|-------------------|
| Embed.ly Embed | EmbedKit oEmbed |
| Embed.ly Extract | EmbedKit Article |
| Embed.ly Card | EmbedKit Widget |
| Embed.ly Preview (legacy)  | Not supported with EmbedKit |
| Embed.ly Objectify (legacy)| Not supported with EmbedKit |

------

## Route forwarding
EmbedKit's route forwarding technology was designed in mind to be interchangeable with Embed.ly. Please refer to the below table to see a list of routes, and where they forward to.

| EmbedKit URL | EmbedKit Destination |
|---------------------------|-------------------|
| www.embedkit.com |  |
| http://embedkit.com | https://embedkit.com |
| api.embedkit.com | https://embedkit.com |
| https://embedkit.com/api/1/embed  |  |
| https://embedkit.com/api/v1/embed | https://embedkit.com/api/v1/oembed |
| https://embedkit.com/api/1/extract  |  |
| https://embedkit.com/api/v1/extract | https://embedkit.com/api/v1/article |

------

## Parameter matching
URL parameters are compatible with several variations to also be interchangeable with Embed.ly. Below is a list of URL parameters, and what they correspond with.

| Parameter | Treated As |
|---------------------------|-------------------|
| ?uri= |  |
| ?urls= | ?url= |
| ?key= | ?api_key= |

------

