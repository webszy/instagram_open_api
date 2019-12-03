# Instagram Open API V1.0.0

> updated:2019-12-02

### profilePage Data（1）

> 获取用户信息以及前12张帖子，下一页的分页信息等

请求地址：


> https://www.instagram.com/username/

请求方式

> Get

返回示例：

> 返回用户首页的html代码，是一个很长的字符串

备注

> 通过正则取到 __sharedData, 参考 `/(window._sharedData\s?)(=\s?)(.*?);<\/script>/`
>

---

### profilePage Data（2）

> 获取用户信息以及前12张帖子，下一页的分页信息等.

请求地址：

> https://www.instagram.com/username/?__a=1

请求方式

> Get

返回示例：

> 直接返回 _sharedData的内容，json格式

备注：

> 用户信息都在 entry_data.ProfilePage[0].graphql.user下，
>
> 其中id 为用户id,username 为用户登录名，edge_follow 为关注的数量，edge_followed_by 为粉丝数（followers）。
>
> edge_owner_to_timeline_media是用户首页的前12张帖子，其中page_info是分页所需信息。

---

### postPage Data

> 获取某个帖子的详细信息，如果是视频，会有视频的地址

请求地址：

> https://www.instagram.com/p/shortcode/?__a=1

请求方式：

> Get

请求参数：

> 帖子的shortcode，也可由帖子的id计算得到。

返回示例：

> 返回帖子的详细信息，包括评论，各种地址等等。

---

### **分页接口（重要）**

>profilePage默认最多有12张图片，如果用户的帖子数大于12，就会有分页接口，分页接口以page_info中的信息为参数，拉取下一页（默认12张）的帖子信息

请求地址

>```
>https://www.instagram.com/graphql/query/?query_hash=2c5d4d8b70cad329c4a6ebe3abb6eedd&variables=%7B%22id%22%3A%2225025320%22%2C%22first%22%3A12%2C%22after%22%3A%22QVFDRUVaSlR4T2VfYjRuS0I5ci1WZFZjT2JxcUFxYmVUN2ZPazg0SHlpVF9DTl9jbWdJRmdlWWtSVEZOSnQ2WkN6SjMxbTlIZTVvUDJrVnBvWTVIOFBaSw%3D%3D%22%7D
>```

请求方式

> Get

请求参数

>```
>query_hash: 2c5d4d8b70cad329c4a6ebe3abb6eedd,//从profileContainer.js地址中获取queryId
>variables: {
>"id":"25025320",//用户id
>"first":12,//帖子数量，默认12
>//page_info中的end_cursor 
>"after":"QVFDRUVaSlR4T2VfYjRuS0I5ci1WZFZjT2JxcUFxYmVUN2ZPazg0SHlpVF9DTl9jbWdJRmdlWWtSVEZOSnQ2WkN6SjMxbTlIZTVvUDJrVnBvWTVIOFBaSw=="
>
>}
>```

返回示例

> 返回12张图片的信息以及下一页的分页信息

备注

> 1、query_hash会随着地区等因素变化，但是不频繁，可以考虑写死。也可以通过get首页然后匹配profileContainer.js地址，这是我用的正则`/<script.*src="(.*ProfilePageContainer.*)".*<\/script>/`，返回的第三个id为帖子分页id
>
> 2、参数类型都为params,js中可以使用escape或者encodeURI进行转义

## tagPage Data

> 当你搜索一个tag时的页面数据，主要是帖子数据

请求地址

>`https://www.instagram.com/explore/tags/{tag}/?__a=1`

请求方式

> Get

请求参数

> 要查询的tag

---

### search接口

> 可以模糊查询用户名，tag等

请求地址

>`https://www.instagram.com/web/search/topsearch/?context=blended&include_reel: true&query={word}`

请求方式

> Get

请求参数

> 要查询的word

返回示例

> ``` clear_client_cache: false
> {
> clear_client_cache: false,
> has_more: true,
> hashtags: [...], //tag列表
> places: [...], //位置列表
> rank_token: "b09fc8b3-5216-40b4-9118-6ac0cb3beb34",
> status: "ok",
> users: [...],//用户列表
> }```
> ```

