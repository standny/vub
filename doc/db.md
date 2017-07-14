# 表结构

## users

用户表,用于记录用户账号信息

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|phone|varchar|50|True||Unique|手机号(可用于登陆,支持密码登陆和短信登陆)|
|email|varchar|200|True||Unique|邮箱(可用于登陆,支持密码登陆和邮箱验证码登陆)|
|username|varchar|50|False||Unique|用户名(首次第三方登陆时要求填写,如果有设置密码,则可用于登陆)|
|password|char|40|True|||||登录密码(如果是第三方登陆则默认没有密码)|
|reg_type|tinyint||False|0||||注册类型(0: 网站, 1: QQ, 2: 微信, 3: 微博)
|reg_time|timestamp||False|CURRENT_TIMESTAMP||||注册时间|
|avatar|varchar|200|True|||||头像|
|nickname|varchar|50|True|||||昵称|
|last_login_time|timestamp||True|CURRENT_TIMESTAMP||最后登录时间|
|last_login_ip|bigint||True|||最后登录 IP|
|last_login_type|tinyint||True|||注册类型(0: 网站, 1: QQ, 2: 微信, 3: 微博)
|status|tinyint||False|0|Normal|账号状态(0: 正常, 1: 因安全问题(如:连续多次登录失败)被冻结, 2: 被管理员禁用)|

## third_logins

第三方登陆凭证表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|user_id|int||False||Normal|所属用户(关联 users.id)|
|third_key|varchar|64|False|True|Unique|第三方平台唯一标识|
|third_name|varchar|50|False|||第三方平台名称|
|add_time|timestamp||False|CURRENT_TIMESTAMP||入库时间|

## blogs

博客信息表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|user_id|int||False||Normal|所属用户(关联 users.id)|
|name|varchar|200|False||Unique|博客名称|
|description|varchar|500|True|||博客描述|
|create_time|timestamp||False|CURRENT_TIMESTAMP||建立时间|
|domain|varchar|500|True||Unique|博客域名(用户可选自定义域名,如果不自定义,则默认为平台域名+用户名,如: http://blog.com/username)|
|theme|int||False|||博客主题(关联 blog_themes.id) |
|style|int||False|||博客样式表(关联 blog_styles.id) |
|total_articles|int||False|0||总文章数|
|total_visits|int||False|0||总访问量|
|total_categories|int||False|0||类别数量(仅统计顶级类别)|
|total_tags|int||False|0||标签数量|
|add_time|timestamp||False|CURRENT_TIMESTAMP||开通时间|

## blog_categories

博客类别表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|blog_id|int||False||Normal|所属博客(关联 blogs.id)|
|name|varchar|50|False|||类别名称|
|total_articles|int||False|0||文章数|
|total_visits|int||False|0||访问量|
|parent_id|int||False||Normal|父类别(关联 blog_categories.id)|
|add_time|timestamp||False|CURRENT_TIMESTAMP||创建时间|

## blog_tags

博客标签表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|blog_id|int||False||Normal|所属博客(关联 blogs.id)|
|tag|varchar|50|False||Unique|标签|
|articles|text||False|||关联文章(关联 blog_articles.id,多个 ID用逗号分隔)|
|total_articles|int||False|0||文章数|
|total_visits|int||False|0||访问量|
|add_time|timestamp||False|CURRENT_TIMESTAMP||添加时间|

## blog_articles

博客文章表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|blog_id|int||False||Normal|所属博客(关联 blogs.id)|
|title|varchar|200|False|||文章名称|
|content|text||False||FullText|文章内容|
|content_type|tinyint||False|0||文章内容类型(0: html, 1: markdown)|
|tags|varchar|1000|True|||标签(多个标签用逗号分隔)|
|resources|varchar|1000|True|||引用的资源文件(关联 blog_resources.id,多个 ID用逗号分隔)|
|category|int||False||Normal|文章类别(关联 blog_categories.id)|
|total_visits|int||False|0||访问量|
|add_time|timestamp||False|CURRENT_TIMESTAMP||添加时间|

## blog_resources

博客资源文件表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|blog_id|int||False||Normal|所属博客(关联 blogs.id)|
|article_id|int||False||Normal|所属博客(关联 blogs.id)|
|name|varchar|50|True|||资源名称|
|path|varchar|500|False|||文件存放路径|
|up_time|timestamp||False|CURRENT_TIMESTAMP||上传时间|
|hash|varchar|64|False||Unique|文件 Hash|
|total_cites|int||False|0||被引用次数|
|cite_articles|text||True|||引用该资源的文章(关联 blog_aritcles.id,多个 ID 用逗号分隔)|

## blog_themes

博客主题表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|name|varchar|50|False|||主题名称|
|file_path|varchar|500|False|||主题文件存储路径|
|style_count|int||False|0||主题下样式数量|

## blog_styles

博客样式表

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|theme_id|int||False||Normal|对应主题(关联 blog_themes.id)|
|name|varchar|50|False|||主题名称|
|file_path|varchar|500|False|||主题文件存储路径|