# Systemd

## private-tangle.service

Service file for systemd to run the private tangle docker network.

### Improvements

* You may want to set up docker-compose so, that root is not needed.
* I do not exactly know why the sleep is in there, but it might be needed. Might need more investigation.