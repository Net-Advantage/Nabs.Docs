# Containerisation

During the development process, it must be assumed that TechTrek will be containerised.



### Dockerize all the things

Dockerizing existing applications and finding ways to apply the DRY principle to the Dockerfiles. I know that we have a lot of duplication in the Dockerfiles and we need to find a way to reduce that duplication. I think that we can do this by using a base image that contains all the common dependencies and then we can use that base image to build the images for each of the applications.

I need to understand how data storage works for containers. I know that we can use volumes to persist data but I need to understand how that works locally.

Networking is also something that I need to understand. I know that we can use Docker Compose to create a network for our containers but I need to understand how that works locally when container publish ports and how docker hosts can do port mappings.

### Running locally

We need to cover running our application from Visual Studio, debugging, and observability on the local development environment.

I also want to cover running multiple instances of a container locally in order to simulate a cluster. This will help us to understand how to configure our applications to run in a cluster when deploying to the cloud. Hopefully, we will also understand how to observer the behaviour of our applications when running in a cluster.

For local development, I assume that Docker desktop is an orchestrator.

