# Web Requests

### Curl 

```shell
  curl -s -0 http://<ip>/<filename>
  curl -I http://<ip>/

  curl https://www.inlanefreight.com -A 'Mozilla/5.0'
  curl -i http://<SERVER_IP>:<PORT>/
  
  curl -u admin:admin http://<SERVER_IP>:<PORT>/ -v
  curl http://admin:admin@<SERVER_IP>:<PORT>/

  curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/

  curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ -i
  curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```
