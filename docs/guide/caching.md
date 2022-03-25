# Caching

## Express Cache on Demand

Apostrophe uses [express-cache-on-demand](https://www.npmjs.com/package/express-cache-on-demand) to cache content under high load. This feature allows for similar and concurrent requests to compute the result only once. If multiple visitors request the same information at the same time, we load the content once and send it to both visitors. This works for regular page requests, and also for REST API GET requests when no user or session are involved.

Enabled by default, this feature can be disabled by setting `enableCacheOnDemand` to `false` in the options of the [`@apostrophecms/express` module](/reference/modules/express.html#options).

## Specifying a Cache Lifetime

While cache-on-demand is enabled by default, Apostrophe also supports sending cache headers to retain pages and REST API GET request responses for a specified amount of time. Since this means a user could see outdated content, up to the specified cache lifetime, it is not enabled by default.

Just as with `express-cache-on-demand`, Apostrophe always sends `Cache-Control: no-store` in the case of a logged-in user or a user with a populated `req.session`.

To enable the feature for ordinary page responses, set the `cache` option of the `@apostrophecms/page` module:

```javascript
// modules/@apostrophecms/page/index.js
options: {
  cache: {
    page: {
      maxAge: 6000
    }
  }
}
```

To also enable it for GET REST API responses for pages, set the `api` subproperty as well:

```javascript
// modules/@apostrophecms/page/index.js
options: {
  cache: {
    page: {
      // Specified in seconds
      maxAge: 6000
    },
    api: {
      // Specified in seconds
      maxAge: 3000
    }
  }
}
```

To enable it for GET REST API responses for a particular piece type, you can set the `cache` option for that module. Here the example is a project-level module named `article`:

```javascript
// modules/article/index.js
extend: '@apostrophecms/piece-type',
options: {
  cache: {
    api: {
      // Specified in seconds
      maxAge: 3000
    }
  }
}
```

You can also enable it for all piece types by enabling it for `@apostrophecms/piece-type`:

```javascript
// modules/@apostrophecms/piece-type/index.js
options: {
  cache: {
    api: {
      // Specified in seconds
      maxAge: 3000
    }
  }
}
```

Since custom piece types extend this module, configuring it at project level configures all of them with a new default. It is still possible to override the `cache` option in the individual modules.