

# About

Package provides an complete API for HDrezka webservers

> **Supports client fetching and server fetching**

## Features

- Extracting series or movie info
- Extracting direct links to video
- Searching API
- Emulating authorization on HDrezka to avoid geo restrictions


## Usage

First, you must initialize main instance:
```js

import API from 'rezka';

const api = new API('url') // Paste the url to movie or series from official Rezka website
await api.init();

```

Also as a second parameter you can provide a settings object:
```js

const api = new API('url', {

})

```

After initialization, API contain some initial properties:

```js

api.html: string //raw html of page 

api.$: cheerio.CheerioAPI //parsed html by cheerio (cheerio object)

api.initData: { //initial data of content
    thumbnail: string;
    id: string;
    release: string;
    title: string;
    orig_title: string;
    type: 'video.tv_series' | 'video.movie';
    otherParts: Record<string, string>[];
    isContentBlocked: boolean;
}  

api.trData: { //list of translations and MovieSettings (required object with settings for correct working movie mode, more info below)
    tr: {
        [key: string]: string;
    };
    movieSettings: {
        [key: string]: MovieSettings;
    };
} 

api.seasons: { //list of seasons, where key it is naming of translation
    [key: string]: {
        translator_id: string;
        seasons: { [key: string]: string; } | { [key: string]: { [key: string]: string; }; };
        episodes: { [key: string]: any };
    }
} 

```

And then you can use available methods:

```js

const streams = await api.getStream({
    season?: string;
    episode?: string;
    translation?: string;
    index?: string;
    settings?: MovieSettings;
});

const seasonStreams = await api.getSeasonStream(season: string, index: string, translation?: string);

```
