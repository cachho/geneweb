# Info
This is a docker-compose variant to run geneweb and gwsetup. It uses the monthly-rebuilt image `callumgare/geneweb:latest`. It's intended to be used with *Nginx Proxy Manager*, but that is not required. 

# Setup
- run init.sh on the host machine to fix permissions
- gwsetup can only be accessed from the IP specified in `config/only.txt`. If you use *nginx proxy manager*, change it to the IP of the *nginx proxy manager* container. That IP will be displayed in the output of the container when you try to connect.

## Gwsetup
Once you have a database, gwsetup doesn't auto-start anymore. In order to use it (to create more databases or backups), open your container with `docker exec -it geneweb bash` and execute one of the following commands:
- `sh start_setup.sh` to start the gwsetup service in your session. 
- `sh start_setup_daemon.sh` to start the gwsetup service as a daemon

**ATTENTION!**
If you followed the instructions above and setup the `only.txt` to the *nginx proxy manager* ip, the gwsetup service will be publicly accessible, as you effectivly circumenvented the security mechanism of `only.txt`. You should definitely setup an *access list* in *nginx proxy manager* to secure your service. Same goes for your geneweb on port 2316.

## Nginx Proxy Manager Setup
Set your destination to `http://geneweb:2316` for the gwsetup container and `http://geneweb:2317` for the main geneweb service container. Consider using an access list. 

## Config
Source: https://geneweb.tuxfamily.org/wiki/access#Managing_the_GeneWeb_server
- `.config/gwd.xcl` Blacklist
- `.config/only.txt` Only IP Address permitted to access gwd setup
- `geneweb-data/global-access.auth` username:password pairs. Add new line `auth_file=global-access.auth` for each base.
- `geneweb-data/cnt/robot` file contains blacklisted robots/crawlers

# Security
This readme presented a way to secure with *nginx proxy manager*, but you can also use the built in function `global-access.auth` described in the chapter setup/config. They also use HTTP auth, so I prefer the centralized management in *nginx proxy manager*.

Security in regards to `only.txt` has also been described.
