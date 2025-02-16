# Weather-App

Weather App Deployment using Jenkins, Docker, Ansible, and Vagrant

This project automates the deployment of a Python-based weather application using Jenkins, Docker, Ansible, and Vagrant. The pipeline ensures that the application is containerized, pushed to Docker Hub, and deployed on two Vagrant-based virtual machines.
Project Overview

The CI/CD pipeline is responsible for:

    Pulling the Python application from a private GitHub repository.
    Building a Docker image of the application.
    Pushing the image to Docker Hub.
    Using Ansible to:
        Install Docker on the target Vagrant machines.
        Pull the latest Docker image from Docker Hub.
        Run the container on both machines.

Technologies Used

    Jenkins – Automates the pipeline execution.
    Docker – Containerizes the Python application.
    Vagrant – Creates virtual machines for deployment.
    Ansible – Automates software installation and deployment.
    GitHub – Hosts the application and Dockerfile.
    Docker Hub – Stores the built Docker image.

Verification

Once the pipeline completes:

    Run docker ps on both machines to confirm the container is running.
    Open a browser and visit http://192.168.56.14:5000 and http://192.168.56.15:5000.
    You should see the Weather App UI.

Conclusion

This pipeline automates the end-to-end deployment of a Python weather application using Jenkins, Docker, Ansible, and Vagrant. It ensures smooth delivery from code commit to running application containers on multiple machines.

Output

![System Monitor Screenshot](images/m01.png)
![System Monitor Screenshot](images/m02.png)
![System Monitor Screenshot](images/pipeline.png)

