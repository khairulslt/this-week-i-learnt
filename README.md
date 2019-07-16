## Monday - Friday (15/7/19 - 19/7/19)
- learnt how you can serve a react app as a static web-app without a webserver
- configure nginx to serve different index.htmls on different ports 

```
# nginx.conf
server {
      listen 4001;
      root /etc/nginx/shipper;
      index index.html;
      try_files $uri /index.html;
  }

  server {
      listen 4002;
      root /etc/nginx/ops;
      index index.html;
      try_files $uri /index.html;
  }
```

- when exposing nginx ports, don't forget to expose the ports on docker as well

```
# docker-compose.yml
  nginx:
    image: nginx
    volumes:
      - ./env/local/nginx.conf:/etc/nginx/nginx.conf:ro
      - ../janio-shipper/build:/etc/nginx/shipper/:ro
      - ../janio-ops/build:/etc/nginx/ops/:ro
    ports:
      - "80:80"
      - "4001:4001"
      - "4002:4002"
```

- learnt about `mime types` and how they can legitimately make your web app look like crap if you don't include it (especially in your `nginx.conf`)
- `mime types` basically help to identify files on the internet, i.e a css file as a css file / a js file as a js file


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