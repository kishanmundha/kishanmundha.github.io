---
layout: master
title: Cross domain setup in MVC Web API
categories: mvc
---

Setup for cross domain ajax request

### web.config

{% highlight xml %}
  <system.webServer>
    <handlers>
      <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
      <remove name="OPTIONSVerbHandler" />
      <remove name="TRACEVerbHandler" />
      <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
    </handlers>
    <httpProtocol>
      <customHeaders>
        <add name="Access-Control-Allow-Origin" value="*"/>
        <add name="Access-Control-Allow-Methods" value="GET,POST,OPTIONS"/>
        <!--<add name="Access-Control-All-Headers" value="Origin, X-Requested-With, Content-Type, Accept"/>-->
        <add name="Access-Control-Allow-Headers" value="Origin, X-Requested-With, Content-Type, Accept"/>
      </customHeaders>
    </httpProtocol>
  </system.webServer>

{% endhighlight %}


{% highlight csharp %}

[AcceptVerbs("OPTIONS")]
[HttpOptions]
[HttpPost]
public IHttpActionResult Login(LoginModel model)
{
    if (Request.Method == System.Net.Http.HttpMethod.Options)
        return Ok();

    if (model == null)
    {
        return BadRequest();
    }

    var u = db.Usermasters.Where(x => (x.ExtUserId == model.UserName || x.EmailId == model.UserName) && x.Active).FirstOrDefault();

    if (u == null)
    {
        return Unauthorized();
    }

    if (u.Password != model.Password)
    {
        return BadRequest("Invalid username of password");
    }

    FormsAuthentication.SetAuthCookie(model.UserName, true);
    //System.Web.HttpContext.Current.Session["userId"] = u.UserId;
    //System.Web.HttpContext.Current.Session["roleId"] = u.RoleId;

    return Ok(new
    {
        firstName = u.FirstName,
        username = u.ExtUserId,
        email = u.EmailId,
        role = u.RoleId
    });
}

{% endhighlight %}