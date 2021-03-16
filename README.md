Deploy Quirrel with Dokku!

```bash
# Install required plugins
sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
sudo dokku plugin:install https://github.com/dokku/dokku-redis.git redis

# Create Redis instance and Quirrel app
dokku redis:create quirrel
dokku apps:create quirrel

# Configure and deploy Quirrel
dokku config:set quirrel PASSPHRASES=REPLACEME
dokku redis:link quirrel quirrel
dokku git:sync --build quirrel https://github.com/aditsachde/quirrel-dokku.git

# Replace default domain with custom domain
# Remember to point DNS properly
dokku domains:set quirrel quirrel.domain.tld

# expose quirrel on http port, required for cert
dokku proxy:ports-add quirrel http:80:9181
# automatically adds https:443:9181
dokku letsencrypt quirrel
```