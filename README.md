# wordpress-stack

## env

before forking, create the following environment variables in Jenkins:

### `wordpressDomain`
Example: `wordpress.yourdomain.com`

Note: When adding a stack that uses a new domain name, it's recommened to configure the DNS before
deploying to avoid DNS caching issues on your local machine. This occurs when you attempt
to go to a domain before the DNS for it has been configured. The unconfigured resolution will
can get cached on your router or computer, which technically doesn't cause issues for anyone
but you, but it is annoying!

### `wordpressDbName` (optional)
Example: `wordpress.yourdomain.com`
Default: `wordpress`

## credentials

### wordpress-db-root-auth
Add new global credentials to jenkins of type user/password
with id `wordpress-db-root-auth`
user: root, password: generate a new password

### wordpress-db-auth
Add new global credentials to jenkins of type user/password
with id `wordpress-db-auth`
user: choose a username, password: generate a new password
