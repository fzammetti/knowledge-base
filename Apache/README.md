# Apache

---

* [Configure Apache to proxy for an app running on Tomcat](#a6e645e7-e82e-46d4-8f30-cf6737a8f481)

---




<div id="a6e645e7-e82e-46d4-8f30-cf6737a8f481">

## Configure Apache to proxy for an app running on Tomcat

</div>

To configure Apache to proxy for an app running on a Tomcat server on port 8080, in **httpd.conf**:

    ProxyPass /<app_name>/ http://<server_name>:8080/
    ProxyPassReverse /<app_name>/ http://<server_name>:8080/
    ProxyHTMLURLMap http://<server_name>:8080 /<app_name>/
    <Location /<app_name>/>
      ProxyPassReverse /
      SetOutputFilter  proxy-html
      ProxyHTMLURLMap http://<server_name>:8080 /<app_name>/
      ProxyHTMLURLMap / /<app_name>/
      ProxyHTMLURLMap  /<app_name>/ /<app_name>/
      RequestHeader    unset  Accept-Encoding
    </Location>

  Now, http://\<server_name>/\<app_name> will redirect to http://\<Server_name>:8080 (which is assumed to be a Tomcat
  server in a default configuration) with the app running as the ROOT app (you can modify this to point to a different
  context if you need to).

  Note that this depends on the following modules being installed:

  LoadModule proxy_module modules/mod_proxy.so
  LoadModule proxy_html_module modules/mod_proxy_html.so
  LoadModule proxy_http_module modules/mod_proxy_http.so
