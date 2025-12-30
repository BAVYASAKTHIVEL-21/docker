### Docker 
- image is a template
- container is running instance of a image 
- docker is a platform it let us package an app with additions needed
  
### Docker image 

- A blueprint / template
- Read-only
- Contains:
- OS layer (minimal Linux)
- App
- Dependencies

### Docker container 

- A running instance of an image
- Created from an image
- Has:
- Running process
- Memory
- Network
- File system (temporary)

| Real world              | Docker    |
| ----------------------- | --------- |
| Recipe                  | Image     |
| Dish cooked from recipe | Container |
| Blueprint               | Image     |
| House                   | Container |

Docker exists to solve environment consistency issues
VM = full OS per app
Docker = shared OS kernel

Docker architecture:
Client → Daemon → Image → Container

docker --version → check installation
docker info → check daemon
docker run hello-world → test Docker
