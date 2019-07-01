# Preprocessing data from sensors using pySpark

It is a usual case when you have sensors installed throughout your manufacturing line. Each sensor constantly gives a signal which usually needs to be preprocessed before machine learning techniques can be applied. In the following sections I will describe the use-case, the structure of the data collected from sensors and the way how it should be preprocessed. We will also look at the execution time improvement that can be reached with pySpark.


You can use pySpark locally which is helpfull to prepare your code before running it on server. If you don't have it on your laptop, please, follow this [link](https://blog.sicara.com/get-started-pyspark-jupyter-guide-tutorial-ae2fe84f594f) to install it. 


## Data description

In this case we will have the data exported from Historian database. It has the following format:

- TagName
- DateTime 
- Value
- StringValue

**TagName** is a unique identifier of the sensor, it has information where it is installed and what type of information is collected by this sensor. **DateTime** is the date and the time when a certain value was recorded by the sensor. **Value** is a numeric value recorderd by the sensor. **StringValue** is a string value recorderd by the sensor.

We will have daily extracts of the data each in one csv file. Take a look at the example in data folder. You can also use the script _generate_data.py_ to generate such data.

## Preprocessing description

## Comparison



