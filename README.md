### Setup Jupyterlab with Docker Container

### Create a Project folder where all your project files will exist.
    after creating project folder open your computer terminal. Crate a file called Dockerfile.
    For creating this just open your notepad application and save the file by giving name is Dockerfile,
    One thing you have to remaind here this file has no extention.Then write below programe.
    Here I used "jupyterlab_with_docker" as project folder 

```
    FROM python:3.10-slim

    # Set working directory
    WORKDIR /app

    # Install system dependencies
    RUN apt-get update && apt-get install -y \
        build-essential \
        && rm -rf /var/lib/apt/lists/*

    # Install Python packages
    COPY requirements.txt .
    RUN pip install --no-cache-dir -r requirements.txt

    # Expose JupyterLab port
    EXPOSE 8888

    # Start JupyterLab
    CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```


### details of the above programe
    Here first line used becasue this docker_image will use python 3.10 version when will create 
    docker_image 
    
    Second line means inside docker a folder is exist called  "app" folder that will 
    be used as workdirectory for this project.

    Install python package section will install all the required library that already have mentioned
    inside the "requirements.txt" file last line for run the  python program file from your terminal.
    After next step, here I will create a python file that name is "main.py"
    
    Port section that expose 8888 Jupyterlab port for this program. 

    last section run the cmd command


### Now create docker image by running this below command

```
    docker build -t docker_imgage_name .
```
    
    The -t flag in docker build -t name . is used to tag (i.e., name) your Docker image.
    -t image-name: gives your image a name (or "tag")

    .: tells Docker to build from the current directory

    Why Use -t (Tagging)?
    Without tagging, Docker assigns your image a random name and ID, which is:

    Hard to remember

    Hard to reuse when running (docker run) Or push it to a registry by this code
    "docker push yourusername/docker_imgage_name"

### Create Docker Container using already created docker_imgage_name

```
    docker run --name jupyter_container -p 8888:8888 -v "$(pwd)"\app:/app docker_image_name
    As example that I used 

    docker run --name jupyter_container -p 8888:8888 -v "E:\My_Work\datascience\jupyterlab_with_docker\app":/app jupyter_doc_img
```

    For the above docker run code , You need to create "app" folder inside project directory. Inside "app" folder
    create a file called "sample.ipynb" and write some python code that will run in your browser. I am giving 
    here a smple code for you


```
    import matplotlib.pyplot as plt

    x =[1, 2, 3, 4, 5]
    y = [2, 3, 5, 7, 11]
    plt.plot(x, y)
    plt.xlabel('X-axis label')  # Corrected: plt.xlabel('X-axis label')
    plt.ylabel('Y-axis label')  # Corrected: plt.ylabel('Y-axis label')
    plt.title('Sample Plot')  # Corrected: plt.title('Sample Plot')
    plt.show()  #show the plot, Corrected: plt.show()
```
    Here 88888 exposed port that I have mentioned in Dockerfile.

    Why Use -v "$(pwd)/app":/app
    The -v flag mounts a volume — it connects a folder from your host (your PC) into the container.

    Part	Purpose
    -v	Volume mount flag
    $(pwd)/app	Your local folder (e.g., ./app)
    /app	Folder inside the container

    This ensures:
    You can open, edit, and save notebooks outside Docker
    Notebooks created in JupyterLab are saved to your real machine
    When the container exits, your work isn't lost
    Without -v, notebooks you create in Jupyter would stay only inside the container, and get wiped when it's deleted.

    After successfully run the docker_container you will see the progam run on port 88888
    the open the http link and paste in browser it will run and show the x-y cordinate graph 