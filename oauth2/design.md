#Oauth2 设计思想

app_id = open_id 从ext页面传入，ext页面发到到ext 后端，ext 用这个open_id 作为用户身份的标签，作为将来存储数据的身份关联。

要处理 至少3种token

ext token: 成功获取授权以后 ext 分配给 open_id 前端的token 用来获取 ext的内容。  
access token: 用来判定是否拥有授权，并且将来可以获取 平台数据。  
refresh token: 用来刷新access token。  

用户通过平台页面，进入ext的页面，通过url传递 app_id = open_id 给ext 页面  
ext 页面拿到 open_id 以后 带着必要数据  

1. app_key
2. response_type
3. redirect_uri
4. state
5. open_id

===========================

跳转到授权页面，获取授权后，跳转回ext的页面，并且带着code。  
code 中已经包含了 open_id  
ext 自己把 code 和 open_id 交给它自己的后端。  

ext 后端 带着code 再去获得token。  

ext发动后端请求时需要带有以下参数:  

1. app_key
2. app_secret
3. grant_type
4. [refresh_token]
5. [code]

[]为可选，如果没有可以传空值

当 grant_type 为 'code': code 字段为必须
当 grant_type 为 'refresh_token': refresh_token 字段为必须 

*** 注意：app_key app_secret 都是对应ext的外部应用，而这里关联的 app_id 就是 ext_id ***

sup的后端会校验code 中的open id 是否有效，并且判断 ext slug 是否存在于 open_id 所对应的app的extensions 列表中。  
通过就返回token。  

===========================

ext 判断该用户是否拥有 ext 的 token, 接着分3种情况  
有ext token，也有access token -> 通过  
有ext token，没有access token 但是有refresh token -> 使用refresh token 获取 access token  
没有ext token -> 重新获取授权  
