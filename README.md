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
3. The latest version of Docker

If you use ubuntu, you can run the following commands:
``` bash
apt-get update

apt-get install -y ca-certificates curl python3-pip

# Installing Docker

apt-get install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update

apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
## Installation steps

To install the battery-archive agent follow these steps:

1. git clone https://github.com/battery-lcf/batteryarchive-agent
2. cd batteryarchive-agent
3. ./bin/docker_gen_env
4. ./bin/setup
5. ./bin/start
6. ./bin/import_queries

The script docker_gen_env generates the information required to access the Postgres database where your battery data is installed. You can see what is in the env file by opening the env file in batteryarchive-agent. Please do not edit this file. 

The env file contains a few important autogenerated keys and passwords. 

1. REDASH_SECRET_KEY = redash-key
2. DATABASE_CONNECTION = database-connection

When you run the setup script, you will be asked to enter an email and create a password. These credentials are needed to access the Redash front web page.

## How to use

After the start script completes, you will be given the URLs to access the various components of the batteryarchive agent. You will have access to:

1. Redash interface at http://your_server_ip
2. JSON API endpoint at http://your_server_ip:4000
3. The AP definitions at http://your_server_ip:1080
4. A new folder on your computer named batteryarchive-agent

To get a description of the API, look at the YAML file in api/api.yaml

During installation a sample database is added. You can use the database to import the sample data. If you like to run the database externally, you can use the schema in the data folder and create a database connection in redash. 

## Final setup and data import

You are now ready to add data and use the site.

1. In the batteryarchive-agent folder, run: python3 -m pip install -r requirements.txt --break-system-packages
2. Import sample data by running the following commands from the project root folder. Importing data may take a few minutes.
3. cd scripts 
4. To import sample li-ion data: python3 data_import_agent.py ../data/'li-ion cell samples'/
5. To refresh Redash Queries: ./bin/refresh_queries

## Populate your redash front end

To see the data and visualization that you just importer, login in the redaash admin interface and access the Queries page. Runs the queries in the order listed below. To run a query, click on it and then click on the blue arrow square in the bottom-right corner of the page. If the import was successful, data will appear in the Table section of the query page after you run it. 

1. Run and publish all the queries that start with Filter
2. Run and publish the queries that end with DD 
3. Run and publish the queries that end with List
4. Go to the Cycle Test Cell List, select the options in the drop down, run and publish


## Enable Jupyter Notebook

You can use the built-in JSON API from anywhere, including Jupyter Notebooks. To get started, install and configure Jupyter Notebook.

1. Run pip3 install notebook
2. If you installed the BA Agent on your local computer, run jupyter notebook --allow-root and skip to step 5
3. If you installed the BA Agent on a remote computer, run  jupyter notebook --allow-root --no-browser 
4. In a new command window on your computer, enable SSH Tunnelling. Run ssh -L 8888:localhost:8888 your_server_username@your_server_ip (the name of the server may be root)
5. In Jupyter notebook, navigate to the scripts/notebooks folder and open the Get_cell_data.ipynb notebook
6. Run the steps in the notebook. The notebook shows some of the data that you imported previously and can be used as a starting point for your analysis

If you have questions or comments, please contact us at info@batteryarchive.org
