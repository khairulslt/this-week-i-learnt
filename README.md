## Monday - Friday (08/7/19 - 12/7/19)
- learnt the basics/simple architecture of Docker & containers
- prepare for migration of our services from Heroku to AWS:
- containerize our backend Django APIs to run with `gunicorn (on centos)` using `docker run` on `Dockerfile`
- learnt about docker-compose files => created a new `nginx` image to run with our backend API in a docker network using `docker-compose up`
- learnt how to get a docker containers internal IP (https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host)
- learnt that you can't access a docker container by internal IP - at least via the browser (https://github.com/docker/for-win/issues/221)
- learnt how to SSH into a docker container (http://phase2.github.io/devtools/common-tasks/ssh-into-a-container/)
- if in an `nginx` container, can do `apt-get update && apt-get install iputils-ping` then `ping` server
- try to use `nginx` to reverse proxy all requests @port80 to port8000(our backend APIs)
- fail dramatically
- kept getting `502 Connection Refused` errors when trying to connect to port80
- learnt general causes of 502 errors:
1) Nothing is listening on the IP:Port you are trying to connect to. (`sudo netstat -tnlp | grep :{port no. here}`)
2) The port is blocked by a firewall. (`sudo tcpdump -n icmp` or `sudo ufw status` ???)
- learnt how to get your own `nginx.conf` into a docker image running nginx (use a `Dockerfile` with `FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf`)
- finally fixed connection issues by editing `nginx.conf`:
- learnt about `upstream` & `proxy_pass` (see https://www.thepolyglotdeveloper.com/2017/03/nginx-reverse-proxy-containerized-docker-applications/)