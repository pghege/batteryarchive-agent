# batteryarchive agent

The battery archive agent contains all the tools needed to get a site like www.batteryarchive.org going:

1. Redash interface optimized to display battery data
2. JSON API to connect to external data sources
3. Sample data that you can use to learn how to import your data
4. Import and Jupyter Notebook scripts

## Requirements 

The batteryarchive agent needs 

1. A Linux* or MAC computer with at the least 8 GB of RAM 
2. python3 and pip3 
3. The latest version of Docker and Docker-compose

*If you don't have access to a Linux machine, you can set up a virtual machine on AWS, Google Cloud, or Digital Ocean. See https://github.com/battery-lcf/batteryarchive-agent/blob/dc93a5fe2237856219eea483387be52d5916025b/creating%20a%20linux%20virtual%20machine.pdf

## Installation steps

To install the battery-archive agent follow these steps:

1. git clone https://github.com/battery-lcf/batteryarchive-agent
2. cd batteryarchive-agent
3. ./bin/docker_gen_env
4. ./bin/setup
5. ./bin/start
6. ./bin/import_queries

The script gen_env generates the information required to access the Postgres database where your battery data is installed. You can see what is in the env file by opening the env file in batteryarchive-agent. Please do not edit this file. 

The env file contains a few important autogenerated keys and passwords. 

1. REDASH_SECRET_KEY = redash-key
2. DATABASE_CONNECTION = database-connection

When you run the setup script, you will be asked to enter an email and create a password. These credentials are needed to access the Redash front web page.

## How to use

After the start script completes, you will be given the URLs to access the various components of the batteryarchive agent. You will have access to:

1. Redash interface at http://your_server_ip:5000
2. JSON API endpoint at http://your_server_ip:4000
3. A new folder on your computer named batteryarchive-agent

To get a description of the API, look at the YAML file in api/api.yaml

## Final setup and data import

You are now ready to add data and use the site.

1. In the batteryarchive-agent folder, run pip3 install -r requirements.txt 
2. Import sample data by running the following commands from the project root folder. Importing data may take a few minutes. 
3. To import sample cycle data: ./scripts/add_data cycle_data.json  
4. To import sample abuse data: ./scripts/add_data abuse_data.json

## Populate your redash front end

To see the data and visualization that you just importer, follow these steps.

1. Go to http://your_server_ip:5000 and login using the email and password that you selected during the setup step
2. Go to settings > Account and copy the API Key
3. In the shell go to cd queries
4. Import visualizations: python3 query_import.py --api-key API Key --redash-url http://0.0.0.0:5000
5. Click on queries (TODO)

## Enable Jupyter Notebook

You can use the built-in JSON API from anywhere, including Jupyter Notebooks. To get started, install and configure Jupyter Notebook.

1. Run pip3 install notebook
2. If you installed the BA Agent on your local computer, run jupyter notebook --allow-root and skip to step 5
3. If you installed the BA Agent on a remote computer, run  jupyter notebook --allow-root --no-browser 
4. In a new command window on your computer, enable SSH Tunnelling. Run ssh -L 8888:localhost:8888 your_server_username@your_server_ip (the name of the server may be root)
5. In Jupyter notebook, navigate to the scripts/notebooks folder and open the Get_cell_data.ipynb notebook
6. Run the steps in the notebook. The notebook shows some of the data that you imported previously and can be used as a starting point for your analysis

If you have questions or comments, please contact us at info@batteryarchive.org
