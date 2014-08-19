# myApp/api/responses/badRequest.js
### 作用

在一个新的Sails项目中，badRequest是服务器默认相应之一。

它包含什么事情应该发生在你希望发出一个HTTP 400 相应的逻辑。他可以在任何一个地方使用，你只需要访问express的'res'类，调用res.badRequest。

你可以编辑本文件来满足你自己的需求，并且可以使用我们的`sails-generate-custom-response`创建器创建一个自定义相应。

浏览参考文档的响应部分获取更多信息。

<docmeta name="uniqueID" value="badRequestjs643116">
<docmeta name="displayName" value="badRequest.js">

```
/**
 * 400 (Bad Request) Handler
 *
 * Usage:
 * return res.badRequest();
 * return res.badRequest(err);
 * return res.badRequest(err, view);
 * return res.badRequest(err, redirectTo);
 *
 * e.g.:
 * ```
 * return res.badRequest(
 *   'Please choose a valid `password` (6-12 characters)',
 *   '/trial/signup'
 * );
 * ```
 */

module.exports = function badRequest(err, viewOrRedirect) {

  // Get access to `req` & `res`
  var req = this.req;
  var res = this.res;

  // Serve JSON (with optional JSONP support)
  function sendJSON (data) {
    if (!data) {
      return res.send();
    }
    else {
      if (typeof data !== 'object' || data instanceof Error) {
        data = {error: data};
      }
      if ( req.options.jsonp && !req.isSocket ) {
        return res.jsonp(data);
      }
      else return res.json(data);
    }
  }

  // Set status code
  res.status(400);

  // Log error to console
  this.req._sails.log.verbose('Sent 400 ("Bad Request") response');
  if (err) {
    this.req._sails.log.verbose(err);
  }

  // If the user-agent wants JSON, always respond with JSON
  if (req.wantsJSON) {
    return sendJSON(err);
  }

  // Make data more readable for view locals
  var locals;
  if (!err) { locals = {}; }
  else if (typeof err !== 'object'){
    locals = {error: err};
  }
  else {
    var readabilify = function (value) {
      if (sails.util.isArray(value)) {
        return sails.util.map(value, readabilify);
      }
      else if (sails.util.isPlainObject(value)) {
        return sails.util.inspect(value);
      }
      else return value;
    };
    locals = { error: readabilify(err) };
  }

  // Serve HTML view or redirect to specified URL
  if (typeof viewOrRedirect === 'string') {
    if (viewOrRedirect.match(/^(\/|http:\/\/|https:\/\/)/)) {
      return res.redirect(viewOrRedirect);
    }
    else return res.view(viewOrRedirect, locals, function viewReady(viewErr, html) {
      if (viewErr) return sendJSON(err);
      else return res.send(html);
    });
  }
  else return res.view('400', locals, function viewReady(viewErr, html) {
    if (viewErr) return sendJSON(err);
    else return res.send(html);
  });
};


```
