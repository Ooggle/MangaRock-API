# MangaRock-API
A unofficial writing of the MangaRock API documentation

So i decided to write this little documentation with some examples cause i haven't see any documentation related to the new MangaRock API.


## GET requests

query_version = 450

- get info of a manga:<br/>

https://web.mangarockhd.com/query/web{query_version}/info?oid={series_oid}&last=0

example : https://web.mangarockhd.com/query/web450/info?oid=mrs-serie-100266297<br/>
response with good parameters:

```js
json{
	"code":int, // 0
	"data":{
		"mid":int, // this one is used for search in database to accelerate queries
		"oid":string, // this one is used in the manga url
		"name":string,
		"author":string,
		"rank":int,
		"msid":int,
		"completed":bool,
		"last_update":int,
		"removed":bool,
		"direction":int,
		"total_chapters":int,
		"description":string,
		"categories":[int],
		"chapters":[mixed],
		"thumbnail":string,
		"cover":string,
		"artworks":[string],
		"alias":[string],
		"characters":[mixed],
		"authors":[mixed],
		"rich_categories":[mixed], // contains among other things the plain name of the categories
		"extra":{ // all content in that array are optional and non-predictable (all content seems to be strings), below are some examples
			"Published":string,
			"Serialization":string,
			// etc
		},
		"mrs_series":null
	}
}
```
<br/>

- get pages of a chapter (in mri format):<br/>
```diff
! Please note that this request no longer work. check the application API instead.
```

https://web.mangarockhd.com/query/web{query_version}/pages?oid={chapter_oid}

example : https://web.mangarockhd.com/query/web450/pages?oid=mrs-chapter-100364752<br/>
response with good parameters :

```js
json{
	"code":int, // 0
	"data":[string] // a table that contain all mri (encoded images) url of the chapter
}
```
<br/>

- get character information:<br/>

https://web.mangarockhd.com/query/web{query_version}/character?oid={character_oid}

example : https://web.mangarockhd.com/query/web450/character?oid=mrs-character-344901<br/>
response with good parameters:

```js
json{
	"code":int, // 0
	"oid":string,
	"name":string,
	"bio":string,
	"thumbnail":string,
	"artworks":[string], // a table that contain all artwork url
	"extra":{ // all content in that array are optional and non-predictable (all content seems to be strings), below are some examples
		"Age":string,
		"Date of birth":string,
		"DOB":string,
		"Height":string,
		"Symbolizes":string,
		"Weight":string,
		"Zodiac sign":string
		// etc
	}
}
```
<br/>

- get author information:<br/>

https://web.mangarockhd.com/query/web{query_version}/author?oid={author_oid}

example : https://web.mangarockhd.com/query/web450/author?oid=mrs-author-100018057<br/>
<br/>
response with good parameters:

```js
json{
	"code":int, // 0
	"oid":string,
	"name":string,
	"bio":string,
	"thumbnail":string,
	"artworks":[string], // a table that contain all artwork url
	"extra":{ // all content in that array are optional and non-predictable (all content seems to be strings), below are some examples
		"Alternate names":string,
	        "Birthday":string,
		"Family name":string,
		"Given name":string,
		"Website":string
		// etc
	}
}
```
<br/>

- get genre infos:<br/>

https://web.mangarockhd.com/query/web{query_version}/genre?oid={genre_oid}

example : https://web.mangarockhd.com/query/web450/genre?oid=mrs-genre-304070<br/>
<br/>
response with good parameters:

```js
json{
	//to do
}
```
<br/>


## Payload POST requests
These requests use raw POST data in json form. You can use [Postman](https://www.getpostman.com/) to test it.

query_version = 401

- search for mangas:<br/>

https://api.mangarockhd.com/query/web{query_version}/mrs_filter

payload format:

```js
json{
	"status":"all", // completed, ongoing or all
	"genres":{ // list of genre to search for, can be empty
		"mrs-genre-100291868":true, // true to include
		"mrs-genre-304070":false // false to exclude
	},
	"rank":"all", // xx-xx or all
	"order":"rank" // rank or name
}
```

<br/>
response with good parameters:

```js
json{
	"code": 0,
	"data": [string] // array of all mrs-serie corresponding to the search
}
```
<br/>

- manga details:<br/>

https://api.mangarockhd.com/query/web{query_version}/manga_detail

payload format:

```js
json{
	"oids":{ // you can search for multiple oids in one request, way more fast
		"mrs-serie-100317564":0,
		"mrs-serie-200083351":0
		// etc
	},
	"sections":[ // list of sections to get, can be empty
		"basic_info",
		"summary"
		// potentially others sections
	]
}
```

<br/>
response with good parameters:

```js
json{
	"code":0,
	"data":{
		"{series_oid}":{
			"basic_info":{
			"name":string,
			"thumbnail":string,
			"thumbnail_extra":{
				"generated":bool,
				"averageColor":string,
				"textBackgroundColor":string,
				"textColor":string
			},
			"cover":string,
			"alias":[string],
			"cover_extra":{
				"generated":bool,
				"averageColor":string,
				"textBackgroundColor":string,
				"textColor":string
			},
			"rank":int,
			"removed":bool,
			"author":string,
			"completed":bool,
			"direction":int,
			"categories": [int],
			"total_chapters": 15,
			"description":string,
			"release_frequency":{
				"type":string,
				"unit":string,
				"amount":int
			},
			"mrs_series":null
		},
		"summary":{
			"plot_points":[string],
			"key_genres":[string]
		},
		"default": {
			"oid":string,
			"mid":int,
			"msid":int,
			"last_updated":int
		}
	},
	"{series_oid}":{array} // depend on how many oids are in the request header
}
```


## Application API (Android)
These requests use headers. You can use [Postman](https://www.getpostman.com/) to test it.

query_version = 402

- get pages of a chapter (in mri format):<br/>

http://api.mangarockhd.com/query/android{query_version}/pagesv2?oid={chapter_oid}

header :

```js
{
  'qtoken': token(url)
}
```
the qtoken is generated using the following code (nodejs example):
```js
const XXH  = require('xxhashjs');

var url = "https://api.mangarockhd.com/query/android402/pagesv2?oid=mrs-chapter-100350606"

console.log('4'+XXH.h64(`${url}:425bd0ffd40bfaefbd184ea34e85d5042c8e74716f6e9f770cefbadba395782b`,0).toString(16))
// this will output 4c4bb873793d42a73
```

<br/>
response with good parameters:

```js
json{
	"code":0,
	"data":[{ // tab with no min and no max
		"url":string
	}, {
		"url":string
	}, {
		"url":string
	}, {
		"url":string
	... // there can be many more
	}],
}
```


## Thanks

thanks to [@MeatReed](https://github.com/MeatReed) for all the application API tests and [@duyleekun](https://github.com/duyleekun/mangarock-ded) for the reverse enginerring part to get the qtoken.


