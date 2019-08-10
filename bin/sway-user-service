#!/bin/sh

# first import environment variables from the login manager
systemctl --user import-environment
# then start the service
exec systemctl --wait --user start sway.service
