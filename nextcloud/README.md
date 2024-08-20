Troubleshooting:
- When initially installing Nextcloud, the data directory needs ownership of ***www-data***.
- The permissions for the root data directory should be 770.
- You need to update the trusted domains setting into the docker container, so that the nextcloud instance can be accessed through the internet without 'Untrusted Domain' error.
- For this, run the following command:
  -     docker exec --user www-data -it <container_name> php occ config:system:set trusted_domains 10 --value="nextcloud.shivamnaik.de"
