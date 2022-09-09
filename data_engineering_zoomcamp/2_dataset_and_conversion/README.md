Although the folder is called **2_dataset_and_conversion**, I've set the dataset to be untracked by git because of it's huge size. However, the dataset used here can be found from the below location:

https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2021-01.parquet

The file was in **.parquet** format and needed to be converted to **.csv**. This is done in a <b>Docker</b> container for which you can see the Dockerfile in this folder. Used the following commands to build and run it:

$ ``docker build -t parquet_to_csv .``

$ ``winpty docker run -it -v /${PWD}:/app/dataset parquet_to_csv``

Then inside the container used the following very simple python script to convert it to csv for later use:

``from os import listdir``

``import pandas as pd``

``pf = listdir()`` #in the container directory, due to the mounting of local directory to it, it then had the parquet dataset file

``pfs = pf[0]`` #the parquet dataset filename can be read into the pfs variable

``df = pd.read_parquet(pfs, engine='pyarrow')`` #Reading the parquet file into a dataframe

``df.to_csv('yellow_tripdata_2021-01.csv', index=False)`` #converting the dataframe into a csv file which then due to the mounting is also available in the host machine's directory from the container
