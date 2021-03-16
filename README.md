Deployed with Dokku!

```
dokku postgres:create quirrel
dokku postgres:link quirrel quirrel

dokku redis:create quirrel
dokku redis:link quirrel quirrel

dokku apps:create quirrel
dokku config:set quirrel RUNNING_IN_DOCKER=TRUE

dokku git:sync --build quirrel https://github.com/aditsachde/quirrel-dokku.git
git clone dokku@dokku.me:quirrel

