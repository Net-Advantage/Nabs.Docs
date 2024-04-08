# On-Premise Installation Considerations

There are a number of items that you will need to consider when dealing with an on-premise installation.

## Container Registry

Your server will require access to a container registry.

If you are going to use a publicly available registry then your will need to configure your environment to allow the server to access the internet. Please consult with your IT department to determine a way forward.

You can alternatively set up a private registry on premise and transfer the images from the public registry. Your server can then access the images from the private registry.

## Monitoring and Logging

Set up monitoring and logging solutions that are compatible with Windows Server and Docker to keep track of your application's health and performance.

## Persistent Storage

If your application requires persistent storage, you'll need to configure volumes in Docker and ensure they are backed up regularly.

## Updates and Maintenance

Plan for how updates to the application will be handled, including updating the container image and redeploying it.

## Documentation and Training

Provide documentation and potentially training to the customer's team to ensure they are comfortable managing the containerized application on Windows Server.
