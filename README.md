# Preprocessing data from sensors using pySpark

It is a common case when you have sensors installed throughout your manufacturing line. Each sensor constantly gives a signal which usually needs to be preprocessed before machine learning techniques may be applied. The following sections describes the use-case, the structure of the data collected from sensors and the way how it can be preprocessed.

You can use pySpark locally which is helpfull to prepare your code before running it on a server. If you don't have it on your laptop, please, follow this [link](https://blog.sicara.com/get-started-pyspark-jupyter-guide-tutorial-ae2fe84f594f) to install pySpark. Also, don't forget to install Java and set the path to it.

## Data description

In this case we have the data similar to the one that might be exported from the database. It has the following format (columns):

- TagName
- DateTime 
- Value
- StringValue

Many sensors are recording signal not periodically but only when its value changes. **TagName** is a unique identifier of the sensor, it has information where it is installed and what type of information is collected by this sensor. **DateTime** is the date and the time when a certain value was recorded by the sensor. **Value** is a numeric value recorderd by the sensor. **StringValue** is a string value recorderd by the sensor.

We have daily extracts of the data each represented by one .csv file. Take a look at the examples in the data folder. You can also use the notebook _generating-sensor-data.ipynb_ to generate such data.

![data-screenshot](https://github.com/SemenovAlex/spark-sensor-data-preprocessing/blob/master/img/data_screenshot.png)


## Process description

We have a production line with 3 equipments. The product is produced in batches that should go through all of them. Second equipment also has 3 different stages, each corresponding to a certain status of this equipment. 

This is how a sensor records data for the entering batch:

- Batch ID is stored by the sensor EQ01.BATCH when a new batch enters the equipment.
- Equipment #1 stores status 0 by the sensor EQ01.STATUS.
- Equipment #1 stores power and temperature values by the sensors EQ01.POWER and EQ01.TEMP until the batch is in the equipment #1.
- Batch ID is stored by the sensor EQ02.BATCH when the batch enters the equipment #2.
- Equipment #2 stores status 0 by the sensor EQ02.STATUS when the batch enters it.
- Equipment #2 stores power and temperature values by the sensors EQ02.POWER and EQ02.TEMP until batch is in the equipment #2.
- At the moment when status of the equipment #2 is changed sensor EQ02.STATUS records a status value.
- Batch ID is stored by the sensor EQ03.BATCH when the batch enters the equipment #3.
- Equipment #3 stores status 0 by the sensor EQ03.STATUS when the batch enters it.
- Equipment #3 stores power and temperature values by the sensors EQ03.POWER and EQ03.TEMP until batch is in the equipment #3.

Sensor saves the value at the time when it's changed with respect to the previous value of the sensor.

## Objective

We want to obtain a data set ready to apply machine learning models. It should have one row per each batch and many columns corresponding to the statistics based on this batch data. Below is the list of the statistics we want to calculate:

1. Minimum, Average, Maximum, Standard deviation of the POWER and TEMP signals in each equipment.
2. Minimum, Average, Maximum, Standard deviation of the POWER and TEMP signals in each equipment per status.
3. Weighted Minimum, Average, Maximum, Standard deviation of the POWER and TEMP signals in each equipment.
4. Weighted Minimum, Average, Maximum, Standard deviation of the POWER and TEMP signals in each equipment per status.

Notes:
- Weighted statistics mean that we should take into account how many seconds signal stayed constant.


## Solution



