# Issues

## > loopback access token

### description
Windows provides an `Authorization` header to the node app.
When loopback sees this it tries to look up the AccessToken
model which doesn't exist. If you see any errors relating to
AccessToken.findById, it's probably this

### solution
Unset the `Authorization` header before loopback.token
middleware is used

```js
app.use(function (req, res, next) {
  delete req.headers['authorization']
  next()
})
```

## > windows path character limit

### description
Windows has a char limit for directory paths. If you go over
it, things break. This is a problem for deeply nested
npm modules

### solution
Hard to avoid. npm 3 will address this.

## > Rewrite rules

### description
client paths and server paths need correct rewrite
rules.
On the client:
```
http://localhost/contracts
http://localhost/contracts/
http://localhost/contracts/pending
etc
```
all need to be rewritten correctly to index.html

On the server:
```
http://locahost/contracts/api/contracts
http://locahost/contracts/api/contractors
http://locahost/contracts/api/contracts?filter=etc
etc
```
all need to be rewritten to contracts/api/iisnode.js

### solution (client)

in web.config in the root of the application:
```xml
<rewrite>
  <rules>
    <rule name="ember">
      <match url=".*" />
      <conditions logicalGrouping="MatchAll">
        <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
        <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
        <add input="{REQUEST_URI}" pattern="^/contracts/(api)" negate="true" />
      </conditions>
      <action type="Rewrite" url="{C:1}" />
    </rule>
  </rules>
</rewrite>
```

### solution (server)

```xml
<rewrite>
  <rules>
    <rule name="default">
      <match url="/*" />
      <conditions logicalGrouping="MatchAll" trackAllCaptures="false" />
      <action type="Rewrite" url="iisnode.js" />
    </rule>
  </rules>
</rewrite>
```

## > web.config xml must use tabs

### description
Mysterious breakages caused by using spaces instead of tabs
in web.config files

### solution
Don't use spaces, use tabs... obviously

## > iisnode js entry file

### description
You need to be able to give iisnode a file in the root
of the project or directory (where the web.config files is)

### solution
create an iisnode.js file in the root of the project
that loads the loopback app

```js
require(__dirname + '\\server\\server.js')
```

## > Interference from the webdav module

### description
For some reason the webdav module causes some pain and should be Unset

### solution
in both web.config files add:

```xml
<modules>
  <remove name="WebDAVModule"/>
</modules>
```

```xml
<handlers>
  <remove name="WebDAV" />
</handlers>
```

## > iisnode watched files

### description
If files are not correctly watched, the node process will not restart when files change

### solution
Something like the following will do it: (probably should be improved)

```xml
<iisnode watchedFiles="*.js;server\server.js;node_modules*;server*;common*" />
```
If the process doesn't seem to be restarting, just make a chance to iisnode.js and save.

## > windows auth

### description
Windows auth lets you authenticate but then you are stuck. Doesn't seem to be possible to log out from the app. You need to log out from your machine.

### solution
You can enable windows digest auth

How to:
In the iis server manager click authentication, disable unauthorized. enable windows digest

## > windows digest

### description
Windows digest only works reliably on internet explorer. Using chrome results in being
asked to reauthenticate over and over again. Pretty annoying

### solution
Use internet explorer. Waa waaaa.
