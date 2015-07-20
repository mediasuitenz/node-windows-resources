# Project structure

- Place static assets in the root of the project folder
- Place loopback files in a folder called `api`
- Place a `web.config` file in the root of the project to handle default document and rewrite rules for the client files
- Place a `web.config` file in the root of the `api` folder to handle default document and rewrite rules for the server files
- create an iisnode.js file in the root of the `api` folder that loads `server/server.js`

Example:
```
contracts
  - web.config
  - index.html
  - robots.txt
  - assets
  - api
    - web.config
    - iisnode.js
    - server
      - server.js
      - etc
    - common
      - models
        - etc
    - fixtures
    - node_modules
```
