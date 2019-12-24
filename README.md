# MangaRock-API
A unofficial writing of the MangaRock API

So i decided to write this little documentation with some examples cause i haven't see any documentation related to the new MangaRock API.


###GET requests

query_version = 401

- get info of a manga :
https://api.mangarockhd.com/query/web{query_version}/info?oid={series_oid}&last=0

example : https://api.mangarockhd.com/query/web401/info?oid=mrs-serie-100266297


- get pages of a chapter (in mri format)
https://api.mangarockhd.com/query/web{query_version}/pages?oid={chapter_oid}

example : https://api.mangarockhd.com/query/web401/pages?oid=mrs-chapter-100364752


- get characters information
https://api.mangarockhd.com/query/web{query_version}/character?oid={character_oid}

example : https://api.mangarockhd.com/query/web401/character?oid=mrs-character-344901


- get author information
https://api.mangarockhd.com/query/web{query_version}/author?oid={author_oid}

example : https://api.mangarockhd.com/query/web401/author?oid=mrs-author-100018057


