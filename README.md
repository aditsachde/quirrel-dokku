# Deploy Quirrel with Dokku!

```bash
# Install required plugins
sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
sudo dokku plugin:install https://github.com/dokku/dokku-redis.git redis

# Create redis instance and quirrel app
dokku redis:create quirrel
dokku apps:create quirrel

# Configure env vars
dokku config:set quirrel PASSPHRASES=REPLACEME
dokku redis:link quirrel quirrel

# Deploy quirrel. If automated updates are desired,
# follow the instructions in the automated updates section instead.
dokku git:sync --build quirrel https://github.com/aditsachde/quirrel-dokku.git

# Replace default domain with custom domain
# Remember to point DNS properly
dokku domains:set quirrel quirrel.domain.tld

# expose quirrel on http port, required for cert
dokku proxy:ports-add quirrel http:80:9181
# automatically adds https:443:9181
dokku letsencrypt quirrel
```

## Automated Updates
A Github workflow is used to open a pull request when the upstream image tag is updated.
There is also a second workflow to automatically deploy when the master.

Fork this repo, then create a new ssh key on the Dokku machine.

```
ssh-keygen -t ed25519 -C "noreply@github.com"
dokku ssh-keys:add quirrel-deploy ~/.ssh/id_ed25519.pub
cat ~/.ssh/id_ed25519
```

Set `secret.SSH_PRIVATE_KEY` to the newly generated key.

Set `secret.DOKKU_URL` to the deploy url of your Dokku instance. For example, `dokku.me:22`.

If your Dokku app is not named quirrel, you may have to update the deploy workflow. More information is availble on the [Dokku docs](https://dokku.com/docs/deployment/continuous-integration/github-actions/).
