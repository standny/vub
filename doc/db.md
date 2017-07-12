# users

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

## third_logins

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|user_id|int||False||Normal|用户 ID(关联 users.id)|
|third_key|varchar|64|False|True|Unique|第三方平台唯一标识|
|third_name|varchar|50|False|||第三方平台名称|

## blogs

|字段|类型|长度|可空|默认值|索引|备注|
|--|--|--|--|--|--|--|
|id|int(自增)||False||Primary|自增ID|
|name|varchar|200|False||Unique|博客名称|
|description|varchar|500|True|||博客描述|
|create_time|timestamp||False|CURRENT_TIMESTAMP||建立时间|
|domain|varchar|500|True||Unique|博客域名(用户可选自定义域名,如果不自定义,则默认为平台域名+用户名,如: http://blog.com/username)


