Troubleshooting:
- When initially installing Nextcloud, the data directory needs ownership of ***www-data***.
- The permissions for the root data directory should be 770.
- You need to update the trusted domains setting into the docker container, so that the nextcloud instance can be accessed through the internet without 'Untrusted Domain' error.
- For this, run the following command:
  -     docker exec --user www-data -it <container_name> php occ config:system:set trusted_domains 10 --value="nextcloud.shivamnaik.de"
- In case the nextcloud container gets deleted accidentally, but the data directory is intact in the linux system,
  - Deploy a new instance of nextcloud container.
  - Create a new user.
  - Move all the files from previous accounts *files* folder to current user's 'files' folder.
  - Open a bash shell to nextcloud container using:  
    -     docker-exec -u www-data <nextcloud_container> php occ files:scan --all
  - All the files will be rescanned and added to the new nextcloud folder. Now, all your files are recovered.
- Sometimes, due to repeated trial-and-error of deploying and stopping the nextcloud container, your IP address is throttled by Nextcloud, thinking you are attacking via bruteforce. The error would look like:
    ```
    Your remote address has been identified as Public-IPV6-of-my-laptop- and is currently being throttled by bruteforce. which slows down the speed of various requests. If the remote address is not your address, it may be an indication that a proxy is not configured correctly. For more information,
    ```
    To fix this issue, you need to reset throttling for your IP address through bash into the container:  
    ```
    docker exec -u www-data [nextcloud-container-name] php /var/www/html/occ security:bruteforce:reset [Public-IPV6-of-your-laptop]
    ```
- If you get background job error in admin console, Cron jobs are not running correctly. In docker, crontab does not execute as expected. Hence, create a cronjob in host system to execute ```cron.php``` every 5 minutes, via ```docker exec```. Follow the following steps in you ubuntu system:
    ```
    $ sudo crontab -u ubuntu -e # This opens an editor to edit the cron tasks.
    $ */5 * * * * docker exec -u www-data nextcloud php -f /var/www/html/cron.php
    ```
