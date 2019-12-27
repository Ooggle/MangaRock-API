# MangaRock-API
A unofficial writing of the MangaRock API documentation

So i decided to write this little documentation with some examples cause i haven't see any documentation related to the new MangaRock API.


## GET requests

query_version = 401

- get info of a manga :<br/>

https://api.mangarockhd.com/query/web{query_version}/info?oid={series_oid}&last=0

example : https://api.mangarockhd.com/query/web401/info?oid=mrs-serie-100266297<br/>
response with good parameters :
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
		"extra":{ // all content in that array are optional and non-predictable (all content seems to be string), below are some examples
			"Published":string,
			"Serialization":string,
			// etc
		},
		"mrs_series":null
	}
}
```
<br/>

- get pages of a chapter (in mri format) :<br/>

https://api.mangarockhd.com/query/web{query_version}/pages?oid={chapter_oid}

example : https://api.mangarockhd.com/query/web401/pages?oid=mrs-chapter-100364752<br/>
response with good parameters :
```js
json{
	"code":int, // 0
	"data":[string] // a table that contain all mri (encoded images) url of the chapter
}
```
<br/>

- get character information :<br/>

https://api.mangarockhd.com/query/web{query_version}/character?oid={character_oid}

example : https://api.mangarockhd.com/query/web401/character?oid=mrs-character-344901<br/>
response with good parameters :
```js
json{
	"code":int, // 0
	"oid":string,
	"name":string,
	"bio":string,
	"thumbnail":string,
	"artworks":[string], // a table that contain all artwork url
	"extra":{ // all content in that array are optional and non-predictable (all content seems to be string), below are some examples
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

- get author information :<br/>

https://api.mangarockhd.com/query/web{query_version}/author?oid={author_oid}

example : https://api.mangarockhd.com/query/web401/author?oid=mrs-author-100018057<br/>
<br/>
response with good parameters :
```js
json{
	"code":int, // 0
	"oid":string,
	"name":string,
	"bio":string,
	"thumbnail":string,
	"artworks":[string], // a table that contain all artwork url
	"extra":{ // all content in that array are optional and non-predictable (all content seems to be string), below are some examples
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

## Payload POST requests
These requests use raw POST data in json form. You can use [Postman](https://www.getpostman.com/) to test it.

query_version = 401

- search for mangas :<br/>

https://api.mangarockhd.com/query/web{query_version}/mrs_filter

payload format :
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

example : https://api.mangarockhd.com/query/web401/author?oid=mrs-author-100018057<br/>
<br/>
response with good parameters :
```js
json{
	"code": 0,
	"data": [string] // array of all mrs-serie corresponding to the search
}
```



