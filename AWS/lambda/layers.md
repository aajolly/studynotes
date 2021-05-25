## Lambda Layers

AWS Lambda Layers is the answer to How can I add packages to AWS Lambda so I could use them. AWS Lambda runtime environment provides you with built-in packages, however if you want to add
custom packages, lambda layers can be used.

In this readme, I'll be covering Python and will add support for nodeJs later.

### Getting Started

1. Setup a virtual environment

```
mkdir lambda 
cd lambda
python3 -m venv venv
source venv/bin/activate
```

More on [venv](https://docs.python.org/3/library/venv.html)

2. Install the libraries you need. For this example, I need `requests`, `pandas` and `flatten_json`.

```
pip install requests
pip install pandas
pip install pyarrow
pip install flatten_json
```

3. While still in the virtual environment, save a list of its dependencies to a `requirements.txt` file:

```
pip freeze > requirements.txt
```

4. Create a python folder. This will house all the libraries to go in our layer. At this point, your folder structure should be:
![lambda_layer_dir_structure](/images/lambda_layer_dir_structure.png)

5. From the project root, run the following command to build your dependencies in a container similar to the Lambda execution environment Amazon provides (courtesy of [lambci](https://hub.docker.com/u/lambci)):
```
docker run --rm \
--volume=$(pwd):/lambda-build \
-w=/lambda-build \
lambci/lambda:build-python3.8 \
pip install -r requirements.txt --target python
```

`--rm`: Makes sure that once the Docker container exits, it's removed (keeps things clean)
`--volume`: Bind mounts the current working directory to the /lambda-build directory on the container (this allows the container to access the requirements.txt file we've just generated, and add files to the python directory). If you're on Windows, you can also paste in a full path to your project root instead of $(pwd).
`-w`: Sets the working directory in the container (in this case, to our project root)
`lambci/lambda:build-python3.8`: The docker image to run this container from -- make sure it matches the Python version you're working with! For reference, Amazon currently provides runtimes for Python 2.7, 3.6, 3.7 and 3.8 (I'm using 3.8 here)
`pip install -r requirements.txt --target python`: Installs the project dependencies as normal, from the requirements.txt file to the python folder (more on that [here](https://pip.pypa.io/en/stable/reference/pip_install/#cmdoption-t))

6. You should see some output from pip. This is running in the Docker container. If successful, you should have dependencies installed to the python folder you created earlier! Mine looks like this:

![lambda_layers_dir_structure_after_docker_run](/images/lambda_layers_dir_structure_after_docker_run.png)

7. At this point, zip python folder: `zip -r pylayer python/`. This will create a pylayer.zip file in your project's root directory.
8. Next, upload the .zip file to Lambda (if <50MB, you can upload directly to Lambda or else you'll have to upload to S3 and reference it while creating lambda layer)
9. Go AWS Lambda Console and click on Layers in the left pane. I already have a layer and I'm adding a new version to it, I upload the file to S3 already
![lambda_layers_create_layer_console](/images/lambda_layers_create_layer_console.png)