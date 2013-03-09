var request=require('request');

//=============
// Step 1
//=============
//建立一个网页，然后在上面加入类似如下的链接，其中client_id，redirect_uri等等
//都是需要你填写的
 // <a href="https://www.douban.com/service/auth2/auth?
 // client_id=%%#!@%#$^%&$%&$%&$#
 // &redirect_uri=%%#!@%#$^%&$%&$%&$#
 // &response_type=code
 // &scope=shuo_basic_r,shuo_basic_w,douban_basic_common">
 // douban
 // </a>


//=============
// Step 2
//=============
//在上一步填写的redirect_uri地址里监听GET请求....
//比如：使用了express.js来监听请求
//
// app.get('/callback/',function(req,res){
//           console.log(req.query);
//          res.send(JSON.stringify(req.query));
//           //var code=JSON.parse(req.query);
//           console.log(req.query.code);
// });
// 返回结果：

// 当用户拒绝授权时，浏览器会重定向到redirect_uri，并附加错误信息

// https://www.example.com/back?error=access_denied
// 当用户同意授权时，浏览器会重定向到redirect_uri，并附加autorization_code

// https://www.example.com/back?code=9b73a4248

//=============
// Step 3
//=============
//使用刚刚得到的code，向豆瓣的服务器发起一个POST请求，得到所谓的access_token参数
//
//
//
//client_id	必选参数，应用的唯一标识，对应于APIKey
//client_secret	必选参数，应用的唯一标识，对应于豆瓣secret
//redirect_uri	必选参数，用户授权完成后的回调地址，应用需要通过此回调地址获得用户的授权结果。此地址必须与在应用注册时填写的回调地址一致
//grant_type	必选参数，此值可以为 authorization_code 或者 refresh_token 。在本流程中，此值为 authorization_code
//code	必选参数，上一步中获得的authorization_code
var data={
		client_id		: '%%#!@%#$^%&$%&$%&$#',
		client_secret	: '%%#!@%#$^%&$%&$%&$#',
		redirect_uri	: '%%#!@%#$^%&$%&$%&$#',
 		grant_type		: 'authorization_code',
 		code 			: '2c849075b347f7da'
};

request.post('https://www.douban.com/service/auth2/token',{form:data}, function (e, r, body) {
var err_message={
		100:'invalid_request_scheme 错误的请求协议',
		101:'invalid_request_method 错误的请求方法',
		102:'access_token_is_missing 未找到access_token',
		103:'invalid_access_token access_token不存在或已被用户删除',
		104:'invalid_apikey apikey不存在或已删除',
		105:'apikey_is_blocked apikey已被禁用',
		106:'access_token_has_expired access_token已过期',
		107:'invalid_request_uri 请求地址未注册',
		108:'invalid_credencial1 用户未授权访问此数据',
		109:'invalid_credencial2 apikey未申请此权限',
		110:'not_trial_user 未注册的测试用户',
		111:'rate_limit_exceeded1 用户访问速度限制',
		112:'rate_limit_exceeded2 IP访问速度限制',
		113:'required_parameter_is_missing 缺少参数',
		114:'unsupported_grant_type 错误的grant_type',
		115:'unsupported_response_type 错误的response_type',
		116:'client_secret_mismatch client_secret不匹配',
		117:'redirect_uri_mismatch redirect_uri不匹配',
		118:'invalid_authorization_code authorization_code不存在或已过期',
		119:'invalid_refresh_token refresh_token不存在或已过期',
		120:'username_password_mismatch 用户名密码不匹配',
		121:'invalid_user 用户不存在或已删除',
		122:'user_has_blocked 用户已被屏蔽',
		123:'access_token_has_expired_since_password_changed 因用户修改密码而导致access_token过期',
		124:'access_token_has_not_expired access_token未过期',
		125:'invalid_request_scope 访问的scope不合法，开发者不用太关注，一般不会出现该错误',
		999:'unknown 未知错误'
};
	var body_jObject=JSON.parse(body);
	//如果是有code则多半意味着失败了，则打印错误代码
	//console.log(body_jObject);
	if(body_jObject.code){
		var code=body_jObject.code;
		console.log(err_message[code]);
	}else{
		//如果成功，则返回该用户的字符串
		// {"access_token":"$!@#$!@#$!@#$!@#",
		// "douban_user_name":"柠檬",
		// "douban_user_id":"55895127",
		// "expires_in":604800,
		// "refresh_token":"%!@#%!@!@#%!@%!%!@#%"}

	}
	//End of has a code?
});
// 在access_token使用过程中，如果服务器返回106错误：“access_token_has_expired ”，此时，说明access_token已经过期，除了通过再次引导用户进行授权来获取access_token外，还可以通过refresh_token的方式来换取新的access_token和refresh_token。

// 通过refresh_token换取access_token的处理过程如下：

// https://www.douban.com/service/auth2/token
// 参数名称	参数说明
// client_id	必选参数，应用的唯一标识，对应于APIKey
// client_secret	必选参数，应用的唯一标识，对应于豆瓣secret
// redirect_uri	必选参数，用户授权完成后的回调地址，应用需要通过此回调地址获得用户的授权结果。此地址必须与在应用注册时填写的回调地址一致
// grant_type	必选参数，此值可以为 authorization_code 或者 refresh_token。在本流程中，此值为 refresh_token
// refresh_token	必选参数，刷新令牌


//=============
// Step 5
//=============
//使用access_token
//
//
//

// 获取当前授权用户信息

// 通过访问以下url来获取授权用户的信息

// GET https://api.douban.com/v2/user/~me
// 例如：

// curl "https://api.douban.com/v2/user/~me"
//      -H "Authorization: Bearer a14afef0f66fcffce3e0fcd2e34f6ff4"

// 说明：Bearer 后面的字符串就是你上一步获取到的access_token.

<!-- 77 if(body_jObject.access_token){
 78         var access_token=body_jObject.access_token;
 79                 console.log(access_token);
 80         var header={'Authorization':'Bearer '+access_token};
 81         request.get('https://api.douban.com/v2/user/~me',{headers:header},function (e, r, body) {
 82                 console.log(body);
 83                 res.send(body);
 84         });//end of get user's profile which is a priv API -->
