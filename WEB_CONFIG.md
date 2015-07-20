# web.config file examples

Client (in the root of the project)
```xml
<configuration>
  <system.webServer>

    <handlers>
      <remove name="WebDAV" />
    </handlers>

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

    <defaultDocument>
      <files>
        <clear />
        <add value="index.html" />
      </files>
    </defaultDocument>

    <modules>
      <remove name="WebDAVModule"/>
    </modules>

  </system.webServer>
</configuration>
```

Server (in the API folder)
```xml
<configuration>
  <system.webServer>

    <handlers>
      <add name="iisnode" path="iisnode.js" verb="*" modules="iisnode" />
    </handlers>

    <iisnode watchedFiles="*.js;server\server.js;node_modules*;server*;common*" nodeProcessCommandLine="C:\Program Files (x86)\nodejs\node.exe" promoteServerVars="AUTH_USER,AUTH_TYPE" />

    <rewrite>
      <rules>
        <rule name="default">
          <match url="/*" />
          <conditions logicalGrouping="MatchAll" trackAllCaptures="false" />
          <action type="Rewrite" url="iisnode.js" />
        </rule>
      </rules>
    </rewrite>

    <modules>
      <remove name="WebDAVModule"/>
    </modules>

  </system.webServer>
</configuration>
```
