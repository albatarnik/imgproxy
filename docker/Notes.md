docker build -t newsifiercom/resizer:latest -f docker/Dockerfile .
docker run -p 8080:8080 -it newsifiercom/resizer
docker push  newsifiercom/resizer