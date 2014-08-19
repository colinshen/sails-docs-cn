# myApp/api/policies/sessionAuth.js
### 作用
prolicy（策略）实例，所有的路由都会被检查在允许客户端访问你的应用的任何一部分。默认情况下，允许任何人访问所有内容，但是在 进入产品模式之前你应该改变这种访问状态。

在Sails中，‘policy（策略）’是在用户被允许访问你的应用的某一些部分之前认证用户的express中间件。关于更多创建policies（策略）的信息，你需要浏览浏览Sails的帮助文档。
<docmeta name="uniqueID" value="sessionAuthjs444151">
<docmeta name="displayName" value="sessionAuth.js">

```
/**
 * sessionAuth
 *
 * @module      :: Policy
 * @description :: Simple policy to allow any authenticated user
 *                 Assumes that your login action in one of your controllers sets `req.session.authenticated = true;`
 * @docs        :: http://sailsjs.org/#!documentation/policies
 *
 */
module.exports = function(req, res, next) {

  // User is allowed, proceed to the next policy, 
  // or if this is the last policy, the controller
  if (req.session.authenticated) {
    return next();
  }

  // User is not allowed
  // (default res.forbidden() behavior can be overridden in `config/403.js`)
  return res.forbidden('You are not permitted to perform this action.');
};

```
