# Nginx

## tangle.conf

Nginx's configuration file to encrypt traffic for the hornet dashboard, api and faucet dashboard.

**Please notice** that it is configured to work with subdomains. You may just want to open the right port for one domain.

**If you only host hornet** remove the Faucet part. It won't be needed.

## Cheat sheet

```sudo nginx -t``` to check if config files has been written correctly

```sudo nginx -s reload``` to apply changes
