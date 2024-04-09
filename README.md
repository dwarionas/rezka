
## HDREZKA Parser & Grabber

Package provides an complete access for HDrezka servers

> **Use this package only in NodeJS environment because of CORS policy. (Server or SSR usage)**

## Features

- Extracting series or movie info
- Extracting direct links to video
- Searching API
- Emulating authorization on HDrezka to avoid geo restrictions


## Usage

First, you must initialize main instance:
```js

import API from 'rezka';

const api = new API('https://rezka.ag/series/comedy/1154-teoriya-bolshogo-vzryva-2007.html');
await api.init();

```

After initialization, API contain some initial properties:

```js

//initial data of content
api.initData: { 
    thumbnail: string;
    id: string;
    release: string;
    title: string;
    orig_title: string;
    type: 'video.tv_series' | 'video.movie';
    otherParts: Record<string, string>[];
    isContentBlocked: boolean;
}  

//list of translations and MovieSettings (required object with settings for correct working movie mode, more info below)
api.trData: { 
    tr: {
        [key: string]: string;
    };
    movieSettings: {
        [key: string]: MovieSettings;
    };
} 

//list of seasons, where key it is naming of translation
api.seasons: { 
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

## Additional features

### Settings object
As a second parameter you can provide a settings object:
```js

const api = new API('url', {
    setHeaders?: {
        [key: string]: string;
    };
    setCookies?: string;
    rezkaURL?: string;
    emulateAuth?: {
        login_name: string;
        login_password: string;
        clearTime?: number;
    }
});

```

As you see, you can set own data to API:
- setHeaders
- setCookies
- rezkaURL - it is main url to HDrezka website. By default it's 'https://standby-rezka.tv'

### Geo blocking bypass

If you need to bypass geo blocking, you can set up property emulateAuth.
First you must register account on HDrezka from country where you can access to player.
Then you can set the emulateAuth object. 
- clearTime it is the time when auth cookies clears, by default it's 18_000_000ms (5 hours)