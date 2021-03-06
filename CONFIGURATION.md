# Configuration

## `config/config.scss`

This file allow you to overwrite Sass variables.

The colors variables are the following:

```
$palette-primary-dark: #455a64 !default;
$palette-primary-color: #607d8b !default;
$palette-primary-light: #cfd8dc !default;
$palette-primary-icon: #fff !default;
$palette-primary-text: #212121 !default;
$palette-primary-background: white !default;
$palette-accent-color: #009688 !default;
$palette-secondary-text: #727272 !default;
$palette-divider-color: #eaeaea !default;
```

You can also overwrite Ionic variables: <http://ionicframework.jp/tutorials/customizing-ionic-with-sass/>

To know all WPHC internal varaibles checkout ```lib/scss/_variables.scss```

## `config/templates`

You can now create your own custom posts/taxonomies templates or overwrite any template of the app by registering them in the `config/templates/index.js` file.

### Create custom posts templates

Custom posts need custom templates because WPHC cannot know in advance the properties of the returned object.

After installation you will find a `config/templates` folder. This is your own folder, you can modify it in any way you want.

In that folder you will find an example of custom templates for the custom post type `movie`.

the `movie` folder contains `list.html` (list all movies) and `item.html` (particular movie).

#### Register your own templates

In `index.js` you will find the following line of code:

```
$templateCache.put('customPosts/movie/item.html', require('!html!./customPosts/movie/item.html'));
$templateCache.put('customPosts/movie/list.html', require('!html!./customPosts/movie/list.html'));
```

What it does is to register custom templates following the naming convention `customPosts/<customPostType>/item.html` for items and `customPosts/<customPostType>/list.html` for lists.

The `require('!html!./customPosts/movie/item.html')` part is just the path to your custom templates in your filesystem.

If you want to add a new custom post type, for instance `countries`, you will end up adding those two lines:

```
$templateCache.put('customPosts/countries/item.html', require('!html!<pathToTheItemTemplate>'));
$templateCache.put('customPosts/countries/list.html', require('!html!<pathToTheListTemplate>'));
```

Now navigating to `#/public/customPosts/countries` will list all the countries the WP-API returns and `#/public/customPosts/countries/<id>` will display one particular country.

### Overwriting existing templates

If you want to modify an existing template since v2 it is possible.

WPHC templates are all located in `lib/templates`. These should not be touched. If you want to overwrite one just copy it and past it into your personal `config/templates` folder.

For instance if you want to modify the way the menu is displayed, you will need to copy `lib/templates/directive/menu.html` and past it wherever you want in `config/templates` folder.

After that you will need to register your new template in `lib/templates/index.js` (read the comments for help)

## `config/config.js`

Here is a simple view of what you can configure:

* title
* ionicConfig
* api
    * baseUrl
    * timeout
    * maxAttempt
* menu
    * exposeAsideWhen
    * list
* settings
* cache
    * views
    * forward
    * img
    * data
* bookmark
* post
    * comments
* syntaxHighlighter
* taxonomies
* search
* pages
* authors
* posts
* translation
    * available
    * prefered
* analytics
* cordova
    * statubar
    * appRate
    * pushNotifications


### api [Object]

```
{
    "baseUrl": "http://YourDomain/wp-json", // This is where you put the WP-API webservice entry
    "timeout": 3000, // The time in ms when a http request is considered as failed
    "maxAttempt": 3 // The number of attempt the app will try before giving up. A button "Retry" will therefor be display
}
```

### title [String]

Your website title. The title will appear on the top of the left navigation menu

### ionicConfig [Object]

allow overwriting ionic configuration <http://ionicframework.com/docs/api/provider/$ionicConfigProvider/>

Setting ```null``` will keep ionic default value.

### menu [Object]

The media query used to determine when to always display the left menu.

```
"exposeAsideWhen": "(min-width:900px)"
```

They are four types of menu item (internal|external|folder|separator).

* ***internal***    Can be any page of the application (home|category|tag|parameters)
* ***external***    Any www website
* ***folder***      Create a menu sublevel (you can add up to six levels)
* ***separator***   Separate menu items

NB: The icons must be from ionicons.com
NB: For now the homepage is mandatory and cannot be a specific page.

### Settings configuration.

```
"settings": {
    "parameters": {
        "defaultFontSize": "medium" // "small" | "medium" | "large" | "x-large" | "xx-large"
    }
}
```

### cache [Object]

### img

* ***localCacheFolder***    ```String```    name of the cache folder
* ***useDataURI***          ```Boolean```   use src="data:.."? otherwise will use src="filesystem:.."
* ***chromeQuota***         ```Number```    allocated cache space : here 10MB
* ***usePersistentCache***  ```Boolean```   false = use temporary cache storage
* ***cacheClearSize***      ```Number```    asize in MB that triggers cache clear on init, 0 to disable
* ***headers***             ```Object```    HTTP headers for the download requests -- e.g: headers: { 'Accept': 'application/jpg' }
* ***skipURIencoding***     ```Boolean```   enable if URIs are already encoded (skips call to sanitizeURI)

### data

* ***capacity***            ```Number```    Maximum number of items a cache can hold.
* ***maxAge***              ```Number```    The number of milliseconds until a newly inserted item expires.
* ***deleteOnExpire***      ```String```    Determines the behavior of a cache when an item expires. Possible values: none | passive | aggressive
* ***recycleFreq***         ```Number```    Determines how often a cache will scan for expired items when in aggressive mode.
* ***cacheFlushInterval***  ```Number```    If set, remove all items from a cache on an interval after the given number of milliseconds. Default: null.
* ***storageMode***         ```String```    Determines the storage medium used by a cache.

### post

```
"comments": {
    "enabled": true,    // display or not the comments
    "depth": 2,         // the thread depth you want to display. More than 2 on mobile devices may be too much
    "per_page": 50      // The max number of comments you want to display
},
"cache": { // Overwrite global cache. Can be empty

}
```

### Bookmark

The bookmark cache has a difference from others. maxAge is infinite and cannot be overwritten.

```
"bookmark": {  // Overwrite global cache. Can be empty
    "cache": {
        "capacity": 10
    }
},
```

### taxonomies, posts, authors, pages, search

Similar configuration, contains a way to overwrite the list query and the cache.

### translation

The languages you want to see in the parameters. So far we only support English and French.

### analytics

Enable Google Analytics

### cordova

#### StatuBar

#### appRate

* ***enabled***                 ```Boolean```   Enable/Disable appRate
* ***language***                ```String```    Language of Dialog - eg 'en', 'fr', 'it'
* ***appName***	                ```String```    Custom application title
* ***openStoreInApp***	        ```Boolean```   Leave app or not
* ***usesUntilPrompt***	        ```Integer```   Number of runs of app before dialog is displayed
* ***promptForNewVersion***	    ```Boolean```   Show dialog again if new app version
* ***useCustomRateDialog***	    ```String```    Use custom view for rate dialog
* ***iosURL***	                ```String```    Application id in AppStore
* ***androidURL***	            ```String```    Application URL in GooglePlay
* ***windowsURL***	            ```String```    Application URL in WindowsStore

### syntaxHighlighter

Programming language auto detection for tech blogs

* ***tabReplace***: a string used to replace TAB characters in indentation.
* ***useBR***: a flag to generate <br> tags instead of new-line characters in the output, useful when code is marked up using a non-<pre> container.
* ***classPrefix***: a string prefix added before class names in the generated markup, used for backwards compatibility with stylesheets.
* ***languages***: an array of language names and aliases restricting auto detection to only these languages.
