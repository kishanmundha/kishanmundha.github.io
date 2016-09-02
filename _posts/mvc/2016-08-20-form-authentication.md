---
author: todo
layout: master
title: MVC Form authentication
categories: mvc
---

Integration of MVC form authentication

### Login action controller code

{% highlight c-sharp %}
[HttpPost]
public ActionResult Login(User model, string returnUrl)
{
    if (ModelState.IsValid)
    {
        bool userValid = _repository.IsValidUserSignIn(model.username, model.password);
        
        if (userValid)
        {
            FormsAuthentication.SetAuthCookie(username, false);
            return RedirectToAction("Index", "Home");
        }
        else
        {
            ModelState.AddModelError("", "The user name or password provided is incorrect.");
        }
    }

    return View(model);
}

public ActionResult LogOff()
{
    FormsAuthentication.SignOut();
    return RedirectToAction("Index", "Home");
}
{% endhighlight %}

### Change in `Global.asax.cs`
{% highlight c-sharp %}
protected void Application_PostAuthenticateRequest(Object sender, EventArgs e)
{
    if (FormsAuthentication.CookiesSupported == true)
    {
        if (Request.Cookies[FormsAuthentication.FormsCookieName] != null)
        {
          string username = FormsAuthentication.Decrypt(Request.Cookies[FormsAuthentication.FormsCookieName].Value).Name;
          string roles = string.Empty;
          
          roles = _repository.getUserRoles(username);

          HttpContext.Current.User  = new System.Security.Principal.GenericPrincipal(
            new System.Security.Principal.GenericIdentity(username, "Forms"), roles.Split(';'));
        }
    }
} 
{% endhighlight %}
