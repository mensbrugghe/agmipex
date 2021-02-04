# agmipex
AgMIP Explorer - Data exploration for [AgMIP](https://agmip.org/).

## Input Files

 MergedData.csv

## Running in a Docker Container

### Install Docker application

#### On MacOS systems
Install Docker application from the Docker website https://hub.docker.com/editions/community/docker-ce-desktop-mac/. Click blue box with 'Get Docker' text. This will download a Docker.dmg file. Upon opening the disk image (.dmg) file, you will be asked to drag the Docker App icon into your Applications.
Go to your Applications folder and double click 'Docker' to open the program. Once you see a whale image in your upper bar, Docker is running. You may now close the pop-up window, Docker will continue to run in the background.

#### On Debian-based Linux systems
As root (sudo), enter...
```
apt install docker.io
```

### Build Docker image

Run the build step:

```
docker build -t <image_name> --build-arg repourl=<repository_url> --build-arg repodir=<repository_name> --build-arg repodir=<datapath> <path_to_data_files> <path_to_dockerfile>
```

For example, the code I ran was:

```
docker build -t agmipex1 --build-arg repourl=https://github.com/rcpurdue/agmipex.git --build-arg repodir=agmipex --build-arg datapath=data .
```
...where:

 -  "`agmipex1`" is an arbitrary name we will use to identify the image that will be created.
 -  "`https://...`" is URL for this repository.
 -  "`agmipex`" is the name of this repository (and, therefore, the name of the repo's directory).
 -  "`data`" is the local path to the directory holding the data files.

**Note 1:** The latest version of the code is pulled down from the repository. Make sure to commit your local changes first.

**Note 2:** This step will take several minutes.

### Run the image.
```
docker run -p 8866:8866 <image_name>
```

...where `8866` is the network connection port

For example:

```
docker run -p 8866:8866 agmipex1

```

### Run the notebook (via browser)

```
http://localhost:8866/
```

## Developing Using a Docker Container - "Dev Mode"

An alternate Dockerfile is provided to facilitate development iterations (writing code, testing, repeating) while stil leveraging Docker.
In this scenario, the site still runs within the Docker image, however it reaches out onto the host filesystem to read the code and data.
This allows you to make changes to the code or data and then just refresh the page in your browser to test it (rather than rebuilding the image).

The image is built using the following command. Note that the path to the special development Dockerfile should be used (e.g. "./dev/Dockerfile").

```
docker build -t <dev_image_name> <path_to_dev_dockerfile>
```

Use this command to run the "dev mode" docker image:

```
docker run -p 8866:8866 --mount type=bind,src=<full_path_to_repo>,target=/home/jovyan/external <dev_image_name>
```



