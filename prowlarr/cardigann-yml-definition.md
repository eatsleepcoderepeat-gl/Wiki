---
title: Prowlarr YML Definition
description: 
published: true
date: 2021-08-14T18:45:56.061Z
tags: prowlarr, needs-love, development
editor: markdown
dateCreated: 2021-08-14T18:19:59.428Z
---

## General

Using definitions files it's possible to support most trackers without having to write native C# code.
All you need is a little knowledge about HTML and CSS selectors.

In order to add support for a new Cadigann/YML tracker submit a pull request on our [Indexer Repository](https://github.com/Prowlarr/indexers)

The best way to get started is to look at existing definitions files. If you know a tracker which is similar to the one you want to add and is already supported just use it's definition as a base for your new definition file.
Many sites often have a `Powered by` logo at a footer on their pages, and we try to tag our yaml indexers with a comment at the bottom to make finding similar engines a little easier. If you find a matching engine then you can use that indexer as a base for your new site, which will save you a lot of time and effort.

## Format

- Cardigann is quite fussy about indentation. Ensure you maintain the 2 space indentation per level as shown in the examples here. Getting it wrong could lead to errors during a run, or perhaps worse, a silent ignore of the clause altogether!
- Text following a `#` (Hash) is a comment, and the ones in the examples below do not have to be included in your code.

## Header

Each definition must start with a header like this:

```yaml
---
# [REQUIRED] Internal name of the indexer, must be unique. Usually its the name of the
# web site, in lower case, stripped of any special characters and space
id: thepiratebay
  
# [REQUIRED]  Display name (The full name of the tracker)
name: The Pirate Bay

# [REQUIRED] displayed in the tooltip on the add-indexer page and in the config panel
description: "Pirate Bay (TPB) is the galaxy’s most resilient Public BitTorrent site"

# [REQUIRED] Language code of the main language used on the tracker
# See http://www.lingoes.net/en/translator/langcode.htm
# usually you load this with the value from the sites <html lang="en_US"> tag.
language: en-us 

# [REQUIRED] Indexer type:
# public (no registration required)
# semi-private (registration required, but always open)
# private (registration required. Invite/application needed)
type: public 

# [REQUIRED] Website encoding used by the tracker
# usually you get this from the sites html <meta charset=utf-8"> tag.
encoding: UTF-8

# [OPTIONAL] Can be true or false (default is false)
# Enable/Disable automatic update of the URL in case of a redirect to a different domain
followredirect: false

# [REQUIRED] List of known domains
# (the first one is the default, must end with /)
links: 
  - https://thepiratebay.org/
  - https://thepiratesbay.pw/
  - https://tproxy.pro/

# [OPTIONAL] List of old domains which no longer work
# If one of these URLs is configured it will be automatically replaced with the default one
legacylinks: 
  - https://thepiratebay.sw/

# [OPTIONAL] If the tracker uses untrusted HTTPS certificates (self signed, expired, etc)
# you can specify a list of thumbprint hashes which should be accepted as valid anyway.
# This shouldn't be needed in most cases.
certificates:
  - D40789207A75EA36B02E255BF7162C8DF9637751
```

## Caps

Next you've to specify the capabilities of the indexer.

```yaml
# Capabilities of the indexer:
# Mapping between the tracker categories and the Newznab categories.
# - id:      [REQUIRED] The tracker specific category ID.
#            Can be a string too.
# - cat:     [REQUIRED] The corresponding newznab predefined category.
#            See this list for valid options:
#            https://wiki.servarr.com/en/prowlarr/cardigann-yml-definition#categories
# - desc:    [OPTIONAL] The tracker category name.
#            If provided it will be used for a 1:1 mapping between
#            tracker and newznab categories.
# - default: [OPTIONAL] default flag, can be true or false (default is false)
#            Specify if this category should be used as default (if the search query doesn't
#            contain any categories).
caps:
  categorymappings:
    - {id: 101, cat: Audio, desc: "Music", default: false}
    - {id: 201, cat: Movies, desc: "Movies", default: false}
    - {id: 299, cat: Movies/Other, desc: "Video Other", default: false}
    - {id: 302, cat: PC/Mac, desc: "Mac", default: false}
    - {id: 901, cat: XXX, desc: "Porn SD", default: false}
    - {id: 902, cat: XXX, desc: "Porn HD", default: false}
   
  # Specify one or more torznab search modes and attributes that are supported by the indexer.
  # Implementation note: Prowlarr doesn't care very much about this, but you should still
  # specify the correct modes, as most apps calling Prowlarr via the Torznab API depend on them.
  # The q attribute is the absolute minimum default, and you should only add the others if
  # the tracker supports searching with them, especially imdbid, tvdbid, tmdbid and rid (TVRage).
  modes:
    search: [q]
    tv-search: [q, season, ep, imdbid, tvdbid, rid]
    movie-search: [q, imdbid, tmdbid]
    music-search: [q, album, artist, label, year]
    book-search: [q, author, title]
```

### Categories

```none
---- --------------------
id   title  
---- --------------------
1000 Console
1010 Console/NDS
1020 Console/PSP
1030 Console/Wii
1040 Console/XBox
1050 Console/XBox 360
1060 Console/Wiiware
1070 Console/XBox 360 DLC
1080 Console/PS3
1090 Console/Other
1110 Console/3DS
1120 Console/PS Vita
1130 Console/WiiU
1140 Console/XBox One
1180 Console/PS4
 
2000 Movies 
2010 Movies/Foreign
2020 Movies/Other
2030 Movies/SD
2040 Movies/HD
2045 Movies/UHD
2050 Movies/BluRay
2060 Movies/3D
2070 Movies/DVD
2080 Movies/WEB-DL
 
3000 Audio 
3010 Audio/MP3
3020 Audio/Video
3030 Audio/Audiobook
3040 Audio/Lossless
3050 Audio/Other
3060 Audio/Foreign
 
4000 PC 
4010 PC/0day
4020 PC/ISO
4030 PC/Mac
4040 PC/Mobile-Other
4050 PC/Games
4060 PC/Mobile-iOS
4070 PC/Mobile-Android
 
5000 TV 
5010 TV/WEB-DL
5020 TV/Foreign
5030 TV/SD
5040 TV/HD
5045 TV/UHD
5050 TV/Other
5060 TV/Sport
5070 TV/Anime
5080 TV/Documentary
 
6000 XXX 
6010 XXX/DVD
6020 XXX/WMV
6030 XXX/XviD
6040 XXX/x264
6045 XXX/UHD
6050 XXX/Pack
6060 XXX/ImageSet
6070 XXX/Other
6080 XXX/SD
6090 XXX/WEB-DL
 
7000 Books 
7010 Books/Mags
7020 Books/EBook
7030 Books/Comics
7040 Books/Technical
7050 Books/Other
7060 Books/Foreign
 
8000 Other 
8010 Other/Misc 
8020 Other/Hashed
 
100000- Custom   
```

## Settings

Optionally you can specify which config options should be available for the indexer. If the settings block is not specified, the defaults are used (username and password).
Some examples:

```yaml
settings:
  # internal variable name
  - name: username
    # input type
    type: text
    # Display name
    label: Username

  - name: password
    type: password
    label: Password

  - name: pin
    type: text
    label: Pin

  - name: itorrents-links
    type: checkbox
    label: Add download links via itorrents.org
    # [OPTIONAL] set the following to true if you want the checkbox to be ticked. 
    # The checkbox is un-ticked by default
    default: false

  - name: info
    type: info
    label: ITorrents Note
    default: Without the itorrents option only magnet links will be provided.

  - name: category-id
    type: select
    label: Category
    default: 0_0
    options:
      0_0: "All categories"
      1_0: Movies
      1_1: Movies/HD

  - name: quality
    type: multi-select
    label: Select one or more quality
    options:
      480p: 480p
      720p: 720p
      1080p: 1080p
      2160p: 2160p
      4K: 4K
    defaults:
      - 1080p
      - 720p
```

If it's a public tracker and no config settings are needed then set `settings: []` to disable all options.

## Login

If the tracker requires a login you've to include a login block. First you've to pick one of the following login methods:

- post: The input values are transmitted as a HTTP POST request. This will work for many trackers which require only static login information (username, password, ...).
- get: Same as post but HTTP GET is used
- form: The input values are transmitted as a HTTP POST request. But instead of sending them directly, the specified path is retrieved first and the corresponding HTML form is extracted. This allows login to most trackers which require dynamic login information (e.g. CAPTCHAS or CSRF tokens). For google ReCaptchas no special configuration is required, they're detected automatically. In case the the tracker is using "simplecaptcha" (Messages like "click on the Bug" and "Click on the "X") it's automatically solved.
- cookie: the cookies provided via the `cookie` setting will be used.

After sending the actual login request the resulting HTML document is checked for error messages (`error` section). If one of the specified selectors matches the login is considered as failed and the matching text is returned as error message.

After checking for error messages a login test is performed (`test` section). The specified path will be requested. If a redirect is returned the login is considered as failed. Optionally it's possible to specify an selector which must match for a successful login. Typically the `path` is set to the same path as the torrent search path. Most trackers will redirect users to the login page if a login is required. If a tracker will just show the login form (no redirect) you'll have to specify a selector too.

Example for a simple POST login:

```yaml
  # use simple post login
  method: post
  # target of the POST request
  path: takelogin.php
  # list of POST parameters
  inputs:
    # using configured username and password from the prior settings block
    username: "{{ .Config.username }}"
    password: "{{ .Config.password }}"
    # example of a fixed parameter
    keeplogged: 1
  # [OPTIONAL] error message handling
  error:
    - selector: #errormessage > span.warning
  # [OPTIONAL] using a simple redirect based login detection
  test:
    path: browse.php
```

Example for a very complex form based login (Real world form logins won't need most of the options):

```yaml
login:
  # Using a form based login
  method: form
  # Location of the document containing the form
  path: login.php
  # location of the following POST request.
  # Only needed of it's not the same as the action specified in the form element.
  submitpath: takelogin.php
  # Selector for the HTML form element (default: form)
  form: form[action="takelogin.php"]
  captcha:
    # image based captcha (can be image or text)
    type: image
    # selector for the captcha HTML element
    selector: img[alt="Security code"]
    # name of the target form element (captcha value)
    input: code
  inputs: 
    # use configured username and password from the settings
    username: "{{ .Config.username }}"
    password: "{{ .Config.password }}"
    # example of a fixed parameter
    keeplogged: 1
  # [OPTIONAL] Only needed in case of dynamic input element names (very rare)
  # If it's set to true the keys/names from the 'input' section will be
  # interpreted as CSS selectors
  # example: https://github.com/Prowlarr/Indexers/blob/84349de6f7e2208caeab1d52d31169830dbbda01/definitions/v1/spiritofrevolution.yml
  selectors: false
  # [OPTIONAL] Only needed in very limited cases.
  # Can be used to include values based on a result of a selector.
  # e.g. if a CSRF token is hidden in JavaScript). 
  selectorinputs:
    # name of the required key-name,  for example: securitytoken
    securitytoken:
      # selector for the value
      selector: "script:contains(\"stKey: \")"
      # [OPTIONAL] further filters for the value
      filters:
        - name: regexp
          args: "stKey: \"(.+?)\","
  # additional arguments for the URL
  # Send as part of the query string, not in the POST body
  getselectorinputs:
    c:
      selector: "script:contains(\"login.php\")"
      filters:
        - name: regexp
          args: "login.php\\?c=(.*?)&rhash="
    rhash:
      text: 123
  # multiple selectors for errors
  error:
    - selector: tbody:has(td.colhead > span:contains("Error"))
    - selector: tbody:has(td.colhead > span:contains("failed"))
    # example of a complex error message handler
    # e.g. to deal with javascript based error messages
    # if this selector matches => login failed
    - selector: body[onLoad^="makeAlert('"]
      # [OPTIONAL] selector to change the error message which will be displayed.
      message:
        selector: body[onLoad^="makeAlert('"]
        attribute: onLoad
        filters:
          - name: replace
            args: ["makeAlert('Error' , '", ""]
          - name: replace
            args: ["');", ""]
  test:
    path: browse.php
    # [OPTIONAL] check this selector (must match)
    selector: a#logout
```

If the FORM or POST method does not work for the web site you can resort to using the cookie method,
which uses the session cookie when accessing the web site's pages

Example for a typical COOKIE definition:

```yaml
settings:
  - name: cookie
    type: text
    label: Cookie
  - name: info
    type: info
    label: How to get the Cookie
    default: "<ol><li>Login to this tracker with your browser<li>Open the <b>DevTools</b> panel by pressing <b>F12</b><li>Select the <b>Network</b> tab<li>Click on the <b>Doc</b> button (Chrome Browser) or <b>HTML</b> button (FireFox)<li>Refresh the page by pressing <b>F5</b><li>Click on the first row entry<li>Select the <b>Headers</b> tab on the Right panel<li>Find <b>'cookie:'</b> in the <b>Request Headers</b> section<li><b>Select</b> and <b>Copy</b> the whole cookie string <i>(everything after 'cookie: ')</i> and <b>Paste</b> here.</ol>"

login:
  method: cookie
  inputs:
    cookie: "{{ .Config.cookie }}"
  test:
    path: index.php
    selector: a[href="logout.php"]
```

## Search

The search block contains all the information on how to search and how to extract the necessary information from the various trackers.

It's possible to do some optional pre-processing of the search keywords first using the `keywordsfilters` list (e.g. to remove short search words or replace special characters with wildcards. After that the search URLs will be constructed based on the provided `paths` and `inputs`. All resulting paths will be requested. Each result is checked for error messages based on the `error` selector list. After that the rows are extracted based on the selector, etc. provided in the `rows` block. Finally each row is parsed based on the `fields` list.

Example of a complex search block explaining all available options:

```yaml
search:
  # list of paths which should be searched
  # For the most trackers just a single path is needed. But some trackers use
  # different pages for e.g. porn or scene and non scene releases.
  paths:
    - path: torrents.php
      # [OPTIONAL] HTTP method (get or post) (default is get)
      method: post
      # [OPTIONAL] Enable/Disable following of search redirects
      # can be true or false (default is false)
      # If you enable this make sure you specify a selector in the login/test section
      followredirect: false
      # [OPTIONAL] list of tracker categories
      # If specified the path will be only used if at least one category from the list is included in
      # the search categories list. A "!" as first entry negates the matching logic (include the path 
      # in any other than the specified categories is in the search categories list)
      categories: ["!", 901, 902]
      # [OPTIONAL] list of (extra) arguments which should be added for this path
      inputs:
        scene: 0 
      # [OPTIONAL] boolean option to disable input inheritance from the search level inputs list.
      # If set to true the "inputs" from the search level list will be used as the base for the path specific inputs
      # Default is true
      inheritinputs: true
    - path: torrents.php
      # don't use this path if we're only searching for porn
      categories: ["!", 901, 902]
      inputs:
        scene: 1
    - path: xxx.php
      # only use it if we're searching for porn
      categories: [901, 902]
  # list of HTTP arguments which are used by all paths
  inputs: 
    # Generate the category[] arguments list
    # The $raw input is special, the result will be included in the HTTP arguments list
    # without further escaping (only variables are escaped).
    $raw: "{{ range .Categories }}category[]={{.}}&{{end}}" 
    # If an IMDB ID has been specified use it. Otherwise use the search keywords.
    search: "{{ if .Query.IMDBID }}{{ .Query.IMDBID }}{{ else }}{{ .Keywords }}{{ end }}" 
    imdb_search: "{{ if .Query.IMDBID }}yes{{ else }}{{ end }}"
    searchin: title
    incldead: 1
  # [OPTIONAL] extra headers which should be included in search requests
  headers:
    x-requested-with: ["XMLHttpRequest"]
  # [OPTIONAL] list of filters which will be applied to the search string.
  # The result is available in the .Keywords variable
  keywordsfilters:
    - name: re_replace # remove words <= 3 characters and surrounding special characters
      args: ["(?:^|\\s)[_\\+\\/\\.\\-\\(\\)]*[\\S]{0,3}[_\\+\\/\\.\\-\\(\\)]*(?:\\s|$)", " "]
    - name: re_replace # replace special characters with "*" (wildcard)
      args: ["[^a-zA-Z0-9]+", "*"]
  # [OPTIONAL] list of selectors to check for errors on the search result page
  # (same syntax as in the login block)
  error:
    - selector: div.error
  # [OPTIONAL] list of filters to apply to the search result before doing further HTML parsing
  preprocessingfilters:
    - name: jsonjoinarray
      args: ["$.result", ""]
    - name: prepend
      args: "<table>"
    - name: append
      args: "</table>"

  rows:
    selector: table#sortabletable > tbody > tr:has(a[href*="/details.php?id="])
    # [OPTIONAL] list of row filters
    filters:
      # The andmatch filter will make sure that only torrents which contain all words from the search string are
      # returned. This is helpful if the tracker returns a lot of unrelated search results.
      - name: andmatch 
        # [OPTIONAL] argument, the maximum length of the search string which should be compared. Specify this if the
        # trackers cuts of the torrent name after a certain amount of characters.
        args: 66 
      # [OPTIONAL] dump the HTML of each row to the log (for debugging purposes)
      - name: strdump
    # [OPTIONAL] selector for rows containing dates.
    # Use this if the torrent result rows don't contain a publish date but a previous row contains the date.
    # The indexer will go back and parse the first sibling element matching the selector as date for that torrent.
    dateheaders:
      selector: ":has(td.colhead[title]:contains(\"Torrents from\") > b)"
      filters:
        - name: dateparse
          args: "Mon 02 Jan"
    # [OPTIONAL] row merging. Use this if the tracker uses multiple row elements for each torrent
    # (e.g. hidden tooltip or collapsed rows) The specified number of elements from the rows selector result will be
    # merged into the previous element. In this example (1) two rows will be merged together.
    after: 1

  # [REQUIRED] list of attributes which are extracted for each row
  fields:
    # [OPTIONAL] tracker category id (id field from from caps/categorymappings)
    # While not required, it is usual to return a category for Torznab apps to use,
    # so if the site does not provide one in its results then use category Other. 
    category:
      selector: a[href^="browse.php?cat="]
      attribute: href
      filters:
        # extract the "cat" parameter from the query string
        - name: querystring
           args: cat
    # [REQUIRED] the title of the torrent
    title:
      selector: a[href^="details.php?id="]
    # [REQUIRED] link to the site's details page for the torrent
    details:
      selector: a[href^="details.php?id="]
      attribute: href
    # [OPTIONAL] download link for the torrent file. See the download block documentation for special handling.
    # if a download link is not available you should provide a magnet URI, or if neither is available an infohash.
    download:
      selector: a[href^="download.php?torrent="]
      attribute: href
    # [OPTIONAL] magnet link
      magnet:
        selector: a[href^="magnet:"]
        attribute: href
    # [OPTIONAL] auto-generated magnet URI using the infohash
    # When neither the .torrent link or a magnet URI is available, use the infohash statement to auto-generate a
    # magnet URI from an infohash. The magnet's &dn= will be loaded from the .Result.title, and a set of ten of
    # the currently most useful trackers will be added for the &tr= sequence.
    infohash:
      selector: a[href^="index.php?page=torrent-details&id="][title]
      attribute: href
      filters:
        - name: querystring
          args: id
    # [OPTIONAL] link to a poster image (cover, banner, etc.)
    # If the selector does not match it is ignored.
    poster:
      selector: a[href^="details.php?id="]
      attribute: onmouseover
      filters:
        - name: regexp
          args: src=\\'(.+?)\\'
        # replace dummy image with empty string
        - name: replace
          args: ["./pic/noposter.jpg", ""]
    # [OPTIONAL] id for imdb.com if e.g. a link is returned then the number is extracted automatically
    # If the selector does not match it is ignored.
    imdb:
      selector: a[href*="imdb.com/title/tt"]
      attribute: href
    # [OPTIONAL] id for tvrage.com if a link is returned then the number is extracted automatically
    # If the selector does not match it is ignored.
    rageid:
      selector: a[href*="tvrage.com/"]
      attribute: href
    # [OPTIONAL] id for themoviedb.org if a link is returned then the number is extracted automatically
    # If the selector does not match it is ignored.
    tmdbid:
      selector: a[href*="themoviedb.org/movie/"]
      attribute: href
    # [OPTIONAL] id for thetvdb.com if a link is returned then the number is extracted automatically
    # If the selector does not match it is ignored.
    tvdbid:
      selector: a[href*="thetvdb.com/"]
      attribute: href
    # [OPTIONAL] publish date (if the site does not provide a date for all results, then a default of "now" is preferred)
    date:
      selector: td:nth-child(4) > span[title]
      attribute: title
      filters:
        # append the timezone used by the tracker
        - name: append
          args: " +08:00"
        - name: dateparse 
          args: "2006-01-02 15:04:05 -07:00"
    # [OPTIONAL] size of the torrent (units are handled automatically). if the site does not provide a size for all
    # results then a default of "512 MB" is preferred. If the site occasionally has a missing size then "0 B" is usual.
    size:
      selector: td:nth-child(7)
    # [OPTIONAL] number of files
    files:
      selector: td:nth-child(4)
    # [OPTIONAL] number of completed downloads
    grabs:
      selector: td:nth-child(8)
      filters:
        - name: regexp
          # get the first number from the result
          args: (\d+)
    # [OPTIONAL] number of seeders (if the site does not provide seeders for all results,
    # then a default of "1" is preferred). 
    seeders:
      selector: td:nth-child(9)
    # [OPTIONAL] number of leechers (if the site does not provide leechers for all results,
    # then a default of "1" is preferred).
    leechers:
      selector: td:nth-child(10)
    # [OPTIONAL] Factor for the download volume. In most cases it should be set to "1"
    # Set to "0" if a torrent is freeleech, "0.5" if only 50% is counted, "0.75" if only 75% is counted.
    # if a site states that the download is 75% free then the DLVF is 0.25 (only 25% is counted). 
    downloadvolumefactor:
      case:
        img.pro_free: 0
        img.pro_neutral: 0
        img.pro_50pctdown: 0.5
        # default to 1
        "*": 1
    # [OPTIONAL] Factor for the upload volume, in most cases it should be set to "1"
    # Set it to "0" for a torrent that is a neutral leech (upload is not counted), set to "2" for a double upload
    uploadvolumefactor:
      case:
        img.pro_neutral: 0
        img.pro_2up: 2
        # default to 1
        "*": 1
    # [OPTIONAL] minimum ratio the torrent client must seed
    minimumratio:
      text: 1.0
    # [OPTIONAL] minimum number of seconds the client must seed
    minimumseedtime:
      # 1 day (as seconds = 24 x 60 x 60)
      text: 86400
    # [OPTIONAL] description (any other available/relevant information)
    #            This will show up (on the Jacket dashboard search page) as info on a tooltip when you hover over the title
    description:
      selector: td:nth-child(2)
      # remove a and img elements to get rid of spurious text
      remove: a, img
```

Each field starts with the HTML row as value.
If the `text` keyword is specified the keys value is used. This can be used for fixed values (e.g. minimumratio and minimumseedtime).
If a fixed text value is not specified then the presence of the selector keyword is checked. If it's found then it's applied to the row. This allows you to extract more specific details such as the title or download link using CSS selectors.
After that the selector specified in the `remove` keyword is applied. With this, it's possible to remove unwanted elements (See the `description` example above). Any removed elements will be removed for good, they won't be available to following fields. Due to that you should put fields using the remove keyword at the end of the list.
Now it's possible to set the value based on the existence of elements using the `case` keyword. If the corresponding selector matches the field value is set to the specified case value. Processing ends after the first case selector matches. This is commonly used for `downloadvolumefactor` and `uploadvolumefactor`.
Finally the resulting value will be processed by the template engine and filter engine (see below).

### Providing the category field with a default value

In the event that you need to provide a default category due to the possibility that a site may not provide one consistently, you can use the `noappend` modifier, as shown in this example:

```yaml
    category:
      text: "Other"
    category|noappend:
      # try category=
      optional: true
      selector: a.label[href*="category="]
    category|noappend:
      # try type=
      optional: true
      selector: a.label[href*="type="]
```

## Download

The download block is needed in the following cases:

- The torrent download link can't be extracted from the search results (e.g. if it's only available from the details page of the torrent)
- The download request must be done via HTTP POST instead of GET
- You've to access another page first before downloading the file (e.g. you've to click on the "Thank you" button first).

Note: Some trackers just omit the download link from the search results but it still can be easily generated from the available information (e.g. use the details link and replace "details.php" with "download.php"). In this case the download block isn't needed.

Example of the download block explaining all options:

```yaml
download:
  # [OPTIONAL] use HTTP POST instead of GET to download the torrent file
  method: post 
  # [OPTIONAL] HTTP request which needs to be done before downloading the file
  before:
    # request target
    path: thanks.php
    # send via HTTP POST
    method: post
    # [OPTIONAL] if the before link requires a query separator other than the default "&" then use this
    queryseparator: ";"
    # list of HTTP arguments which will be included
    inputs:
      # extract the "id" parameter from the search result download URL query string
      infohash: "{{ .DownloadUri.Query.id }}"
      thanks: 1
  selectors:
    # [OPTIONAL] If a list of selectors is defined, the search result download URL will be retrieved and parsed as HTML.
    # The first selector is then applied to get the actual download URL.
    - selector: a[href^="download.php?id="]
      attribute: href
      # [OPTIONAL] a list of filters which should be applied to the result of this selector
      filters:
        - name: querystring
          args: url
        - name: urldecode
    # [OPTIONAL] As many other selectors as you need, to be used as a fallback for when the prior selector fails to download.
    - selector: a[href^="magnet:?xt="]
      attribute: href
      # [OPTIONAL] a list of filters which should be applied to the result of this selector
      filters:
        - name: toupper
```

## Template engine

The template engine is very basic, and supports the following statements.

### re_replace

A simple regex replace operation.

Syntax: `{{ re_replace .Variable "regex-term" "replace-term"}}`

Example:

```yaml
# Replace any non alphanumeric character in the keywords with the wildcard character
"{{ re_replace .Keywords \"[^a-zA-Z0-9]+\" \"*\" }}"
```

### if ... else ... end

A basic if/else condition. Only boolean true (non empty)/false (empty) operations on variables are supported.

Syntax: `{{ if .Variable }}on true result{{ else }}on false result{{ end }}`

Example:

```yaml
search:
  paths:
    # when .Keywords contains a value
    # then search.php is used as for the path
    # otherwise latest.php is used
    - path: "{{ if .Keywords }}search.php{{ else }}latest.php{{ end }}"
```

### if or/and ... else ... end

The implementation is based on: [go hdr functions](https://golang.org/pkg/text/template/#hdr-Functions)
These are not true logical OR and AND operators in that they operate on variables that contain a value or are empty.

Example of: if or ... else ... end

```yaml
search:
  paths:
    # when any of the 3 vars in brackets has a value 
    # then set the path to search 
    # otherwise set it to music
    - path: "{{ if or (.Query.Album) (.Query.Artist) (.Keywords) }}search{{ else }}music{{ end }}"
  inputs:
    # when either/both of the two query vars in the brackets have a value 
    # then load whichever vars have a value to the string 
    # and when neither var has a value then load the value from .Keywords to the string
    q: "{{ if or (.Query.Album) (.Query.Artist) }}{{ or (.Query.Album) (.Query.Artist) }}{{ else }}{{ .Keywords }}{{ end }}"
```

Example of: if and ... else ... end

```yaml
    title:
      # when both the vars in brackets have a value 
      # then load the value from title_polish to the string
      # when only one of the vars has a value, or both vars have no value
      # then load the value from title_phase1 to the string
      text: "{{ if and (.Config.lang) (.Result.is_polish) }}{{ .Result.title_polish }}{{ else }}{{ .Result.title_phase1 }}{{ end }}"
```

### if eq/ne ... else ... end

The implementation is based on: [go hdr functions](https://golang.org/pkg/text/template/#hdr-Functions)
This is a string comparison only.
Supports the use of both variables and strings.

Example of: if eq ... else ... end

```yaml
    size:
      # when the variable .Result.cat contains the string "series"
      # then the text string will be set to "512 MB"
      # otherwise it will be set to "2 GB"
      text: "{{ if eq .Result.cat \"series\" }}512 MB{{ else }}2 GB{{ end }}"
```

Nesting is supported.

```yaml
    size:
      # when the variable .Result.cat contains any of the strings "movie", "movie_etc", "movie_eng"
      # then the text string will be set to "2 GB"
      # otherwise it will be set to "512 MB"
      text: "{{ if or (eq .Result.cat \"movie\") (or (eq .Result.cat \"movie_etc\") (eq .Result.cat \"movie_eng\")) }}2 GB{{ else }}512 MB{{ end }}"
```

Special variables .True and .False are available
.True contains "True" (which represents a non-empty variable) and
.False contains null (which represents an empty variable).

### join

A simple loop over a list variable building a concatenated string with items joined by a delimiter.  

Syntax: `{{ join .Variable "<delimiter>"}}{{end}}`

Example:

```yaml
# build a query string by concatenating all the categories with a comma
# input: [101,201,301]
"{{join .Categories \",\"}}{{end}}"
# output: "101,201,301"
```

### range

A simple loop over a list variable building a concatenated string.  

Syntax: `{{ range .Variable }}<prefix>{{.}}<suffix>{{end}}`

Example:

```yaml
# build a query string argument list for the selected categories
# input: [101,201,301]
"{{range .Categories}}&cat{{.}}=1{{end}}"
# output: "&cat101=1&cat201=1&cat301=1"
```

### Variable substitution

The basic variable substitution operation.  

Syntax: `{{ .Variable }}`

## Variables

TODO: more explanation

### Config variables (always available)

Generated based on the settings section

```yaml
.Config.$Name # for example .Config.username , .Config.password , .Config.sitelink
```

### Special variables (always available)

```yaml
.True contains "True" (which represents a non-empty variable)
.False contains null (which represents an empty variable)
.Today.Year contains "2020" (or whatever the current year is)
```

### Variables available during search queries

```yaml
.Query.Type
.Query.Q
.Query.Series      # not supported (Cardigann compatibility)
.Query.Ep
.Query.Season
.Query.Movie       # not supported (Cardigann compatibility)
.Query.Year
.Query.Limit
.Query.Offset
.Query.Extended
.Query.Categories
.Query.APIKey
.Query.TVDBID
.Query.TVRageID
.Query.IMDBID      # e.g. tt12345678
.Query.IMDBIDShort # e.g. 12345678
.Query.TMDBID
.Query.TVMazeID    # not supported (Cardigann compatibility)
.Query.TraktID     # not supported (Cardigann compatibility)
.Query.Album 
.Query.Artist
.Query.Label
.Query.Track
.Query.Episode     # EpisodeSearchString, such as S00E00 or S00 or yyyy.MM.dd
.Query.Author
.Query.BookTitle
.Categories        # MappedCategories
.Query.Keywords    # original keywords
.Keywords          # keywords after applying the keywordsfilters
```

Note: variables that are not supported are provided by Cardigann for compatibility with the Torznab specifications. These variables will always return null.

All field results are available to the following fields via the `.Result.$FieldName` variables too.
For example:

```yaml
  fields:
    title:
      selector: h3 a
    subcat:
      selector: div.box ul li:first-child
    year:
      selector: div.box ul li:contains("Year:")
    quality:
      selector: div.box ul li:contains("Quality:")
    description:
      text: "{{ .Result.subcat }} {{ .Result.year }} {{ .Result.quality }}"
```

### Variables available in the download block

Based on the download search field result the following variables are available:

```yaml
.DownloadUri.AbsoluteUri
.DownloadUri.AbsolutePath
.DownloadUri.Scheme
.DownloadUri.Host
.DownloadUri.Port
.DownloadUri.PathAndQuery
.DownloadUri.Query
```

For each query string argument of the URI a corresponding `.DownloadUri.Query.$Key` variable is generated.
for example, a URI like `https://amigos-share.club/torrents-details.php?id=37346&hit=yes`
would generate the following two variables:
`.DownloadUri.Query.id` with the value `37346` and
`.DownloadUri.Query.hit` with the value `yes`.

## Filters

### querystring

Extract values from URL arguments.

Example:

```yaml
# extract the category ID from a category link
selector: a[href^="browse.php?cat="]
attribute: href
filters:
  # input: browse.php?cat=123
  - name: querystring
    args: cat
  # result: 123
```

### prepend

Inserts a *string* by appending additional characters to the beginning of its current value.
The single parameter in the argument is the *string* to be prefixed.

Example:

```yaml
# prefix InfoHash with a magnet URI header
selector: span > a
attribute: href
filters:
  # input: B21F2A6DB07A8F4F76E2C5E15D28235D356B8D41
  - name: prepend
    args: "magnet:?xt=urn:btih:"
  # result: magnet:?xt=urn:btih:B21F2A6DB07A8F4F76E2C5E15D28235D356B8D41
```

### append

Extends a *string* by appending additional characters to the end.
The single parameter in the argument is the *string* to be appended.

Example:

```yaml
# add a tracker to complete the magnet URI
selector: span > a
attribute: href
filters:
  # input: magnet:?xt=urn:btih:B21F2A6DB07A8F4F76E2C5E15D28235D356B8D41&dn=I.Am.A.Magnet
  - name: append
    args: "&tr=udp://tracker.coppersurfer.tk:6969"
  # result: magnet:?xt=urn:btih:B21F2A6DB07A8F4F76E2C5E15D28235D356B8D41&dn=I.Am.A.Magnet&tr=udp://tracker.coppersurfer.tk:6969
```

### tolower

Converts a *string* to lowercase letters.
Does not require any parameters.

Example:

```yaml
# make the title lowercase
selector: dt a
filters:
  # input: MY MOVIE TITLE 1080P
  - name: tolower
  # result: my movie title 1080p
```

### toupper

Converts a *string* to uppercase letters.
Does not require any parameters.

Example:

```yaml
# make the title uppercase
selector: dt a
filters:
  # input: my movie title 1080p
  - name: toupper
  # result: MY MOVIE TITLE 1080P
```

### replace

If the *pattern string* is matched, then the *pattern* is replaced by a *replacement string*.
The first parameter in the argument is the *pattern string*, and the second is the *replacement string*.

Example:

```yaml
# fix the date field when it contains Y-day
selector: td:nth-child(2)
filters:
  # input: Y-day 12:27
  - name: replace
    args: ["Y-day", "yesterday"]
  # result: yesterday 12:27
```

### split

Divides a *string* into an array of *substrings*, and return the selected *substring*.
The first parameter in the argument is the single character *pattern* used to split the *string*, and the second parameter is the array element *number* of the wanted *substring*, counting from zero for the first element.

Example:

```yaml
# extract the category id
selector:  td[class^="coll-1"] a[href^="sub/"]
attribute: href
filters:
  # input: sub/45/0
  - name: split
    args: ["/", 1]
  # result: 45
```

### trim

Removes all leading and trailing occurrences of a set of specified *characters*.
Used without an argument removes all leading and trailing *white-space characters*.
If a set of *characters* are supplied in an argument then those will be removed from all leading and trailing occurrences.

Example:

```yaml
# fetch the title
selector: td:nth-child(2) a
attribute: title
filters:
  # input: &nbsp;This Is My Title&nbsp;
  - name: trim
  # result: This Is My Title
```

```yaml
# extract the title
selector: td:nth-child(2) a
attribute: title
filters:
  # input: xxxThis Is My Titlexxx
  - name: trim
    args: "x"
  # result: This Is My Title
```

### regexp

Perform pattern-matching and "search-and-replace" functions on a *string* using a[Regular Expression](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference).

Example:

```yaml
# extract the uploaded date and time
selector: td:nth-child(2) font.detDesc
filters:
  # input: Uploaded 09-14 02:31, Size 282.88 MiB, ULed by
  - name: regexp
    args: "Uploaded (.+?),"
  # result: 09-14 02:31
```

### re_replace

Similar to [replace](#replace), but the parameters in the argument are [Regular Expressions](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference).

Example:

```yaml
# normalize to SXXEYY format
selector: td:nth-child(2) a.tab
attribute: href
filters:
  # input: 12x45
  - name: re_replace
    args: ["(\\d{2})x(\\d{2})", "S$1E$2"]
  # result: S12E45
```

### dateparse

Converts a date/time *string* into a DateTime object ("ddd, dd MMM yyyy HH:mm:ss z") using a GoLang layout.
Requires two parameters in its argument, the first is the *string* to be processed into the DateTime, and the second is the *layout* to use for the conversion.
The GoLang layout converts as follows:

```yaml
// year
Replace("2006", "yyyy")
Replace("06", "yy")

// month
Replace("January", "MMMM")
Replace("Jan", "MMM")
Replace("01", "MM")
Replace("1", "M")

// day
Replace("Monday", "dddd")
Replace("Mon", "ddd")
Replace("02", "dd")
Replace("2", "d")

// hours/minutes/seconds
Replace("15", "HH")
Replace("03", "hh")
Replace("3", "h")
Replace("04", "mm")
Replace("4", "m")
Replace("05", "ss")
Replace("5", "s")

// fractional seconds
Replace(".0000", "ffff")
Replace(".000", "fff")
Replace(".00", "ff")
Replace(".0", "f")
Replace(".9999", "FFFF")
Replace(".999", "FFF")
Replace(".99", "FF")
Replace(".9", "F")

// AM/PM
Replace("PM", "tt")
Replace("pm", "tt") 

// timezones
Replace("Z07:00", "'Z'zzz")
Replace("Z07", "'Z'zz")
Replace("-07:00", "zzz")
Replace("-07", "zz")
```

Example:

```yaml
# get the DateTime
selector: td.torrent_table_dateAdded
filters:
  # input: 2017-09-18 19:17:24 UTC
  - name: dateparse
    args: "2006-01-02 15:04:05 -07:00"
  # result: Mon, 18 Sep 2017 19:17:24 GMT
```

### timeparse

Alias for [dateparse](#dateparse)

### timeago

Converts a time-ago *string* into a DateTime object ("ddd, dd MMM yyyy HH:mm:ss z").
Does not require an argument.
Timeago can handle a time-ago *string* such as:

```none
now
2 hours and 1 day
4 years ago
1 week
5 months
9hr,12m,39s
8 days 3 hours 12 minutes 10 seconds
```

Example:

```yaml
# get the DateTime (assuming the current time is Mon, 18 Sep 2017 19:17:24 GMT)
selector: td.torrent_table_dateAdded
filters:
  # input: 2 hours and 1 day
  - name: timeago
  # result: Sun, 17 Sep 2017 17:17:24 GMT
```

### reltime

Alias for [timeago](#dateparse)

### fuzzytime

Converts a fuzzy-time *string* into a DateTime object ("ddd, dd MMM yyyy HH:mm:ss z").
By default fuzzytime renders a USA_Date. But if you supply an argument containing "UK" then it will return a UK_Date.
Fuzzytime can handle a fuzzy-time *string* such as:

```yaml
now
4 years ago (or any other timeago values)
Today
Yesterday
Tomorrow
1505788002 (a UNIX time-stamp Tue Sep 19 02:26:42 2017 UTC)
01-31 (dates without a year value)
1 Jan
Wednesday at 15:30
```

Example:

```yaml
# get the DateTime (assuming the current time is Mon, 18 Sep 2017 19:17:24 GMT)
selector: td.torrent_table_dateAdded
filters:
  # input: Yesterday
  - name: fuzzytime
  # result: Sun, 17 Sep 2017 19:17:24 GMT
```

### urldecode

Converts a *string* that has been encoded for transmission in a URL into a decoded *string*.

Example:

```yaml
# decode the url
selector: td:nth-child(2) a.tab
attribute: href
filters:
  # input: https://zooqle.com/search?q=preacher+s01e10
  - name: urldecode
  # result: https://zooqle.com/search?q=preacher s01e10
```

### urlencode

Encodes a URL *string*.

Example:

```yaml
# encode the url
magfile:
  text: "{{ .Result.title }}"
  filters:
    # input: https://zooqle.com/search?q=preacher s01e10
    - name: urlencode
    # result: https://zooqle.com/search?q=preacher+s01e10
```

### validfilename

Ensures that a *string* comprises only characters that are valid for use in filenames.

Example:

```yaml
# get the filename
text: "{{ .Result.title }}"
filters:
  # input: aFile?Name>With<Invalid*Symbols
  - name: validfilename
  # result: aFileNameWithInvalidSymbols
```

### diacritics

Replace diacritics characters with their base character.

Example:

```yaml
# replace any diacritics
keywordsfilters:
  # input: ŠĐĆŽšđčćž
  - name: diacritics
    args: replace
  # result: SĐCZsđccz
```

### jsonjoinarray

Parse the input string as JSON, apply a JSONPath expression and join the resulting array using the specified separator.

args: [JSONPathExpression, separator]

Example:

```yaml
  # extract HTML code from a JSON response
  preprocessingfilters:
    - name: jsonjoinarray
      args: ["$.result", ""]
```

### hexdump

Dump the HTML of each row to the log in HEX format (for debugging purposes).
You will need to have *Enhanced Logging* enabled to view the results.

Example:

```yaml
date:
selector: div[class="resultdivbotton"] div[class="resulttime"] div[class="resultdivbottontime"]
filters:
  # input: Tue, 19 Sep 2017 21:21:52 +12
  - name: hexdump
  # result in the log: mm-dd hh:mm:ss Debug CardigannIndexer (trackername): strdump: T(54)u(75)e(65),(2C) (20)1(31)9(39) (20)S(53)e(65)p(70) (20)2(32)0(30)1(31)7(37) (20)2(32)1(31):(3A)2(32)1(31):(3A)5(35)2(32) (20)+(2B)1(31)2(32) 
```

### strdump

Dump the HTML of each row or field to the log (for debugging purposes).
You will need to have *Enhanced Logging* enabled to view the results.
If you are using strdump to debug multiple field selectors, you can use the Optional args so that you can uniquely tag the results in the enhanced log.

Example:

```yaml
selector: div[class="resultdivbotton"] div[id^="hideinfohash"]
filters:
  # input: dbbde2fc0c299c1d1aa43280b57dafc3fbf0bd39
  - name: strdump
  # result in the log: mm-dd hh:mm:ss Debug CardigannIndexer (trackername): strdump: dbbde2fc0c299c1d1aa43280b57dafc3fbf0bd39
```

```yaml
fields:
  title:
    selector: a[href*="?p=torrents&pid=10&action=details"]
    filters:
      # input: Star Trek 1080p
      - name: strdump
        args: title 
      # result in the log: # mm-dd hh:mm:ss Debug CardigannIndexer (trackername): strdump(title): Star Trek 1080p
  download:
    selector: a[href^="details.php/?id="]
    attribute: href
    filters:
      # input: http://tracker.btnext.com/details.php/?id=123456
      - name: strdump
        args: dl_href_in
      # result in the log: mm-dd hh:mm:ss Debug CardigannIndexer (trackername): strdump(dl_href_in): http://tracker.btnext.com/details.php/?id=123456
      - name: replace
        args: ["/details.php/", "/download.php/"]
      - name: strdump
        args: dl_href_out
      # result in the log: mm-dd hh:mm:ss Debug CardigannIndexer (trackername): strdump(dl_href_out): http://tracker.btnext.com/download.php/?id=123456
```

## Credit

- This page is based on [Jackett's wiki entry](https://github.com/Jackett/Jackett/wiki/Definition-format) by [garfield69](https://github.com/garfield69).