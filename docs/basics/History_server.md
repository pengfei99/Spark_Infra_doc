# Spark History server

When you create a spark session, a spark web UI is created. It shows much information of the spark application such as:
- Spark session configurations(e.g. worker number, cpu, memories, etc.)
- Spark Jobs, stages, and tasks details (e.g. `Driver and Executor resource utilization` of each stages)
- DAG execution
- Application logs and many more

But when the spark session is closed, we lose all these information.

To keep the history(event logs) of all completed applications and its runtime information which allows us to 
review metrics and monitor the application later in time, we need to enable a feature of spark called **Spark History Server**. 

The Spark History Server is a `Web User Interface` that is used to monitor the metrics and performance of the 
completed Spark applications. 

Spark History server can keep the history of event logs for the following

- All applications submitted via spark-submit
- Submitted via REST API
- Every spark-shell you run
- Every pyspark shell you run
- Submitted via Notebooks

## Configuration

In order to store event logs for all submitted applications, first, Spark needs to collect the information while 
applications are running. **By default, the spark doesn’t collect event log information**. To enable the event log,
we need to add below lines in **spark-defaults.conf**

```shell
# Activate the spark event log
spark.eventLog.enabled true

# specify the location of where the spark save the log
spark.eventLog.dir hdfs://10.50.5.67:9000/spark-logs

# specify the log format provider class
spark.history.provider org.apache.spark.deploy.history.FsHistoryProvider

# specify the location from where history server to read event log
spark.history.fs.logDirectory  hdfs://10.50.5.67:9000/spark-logs

# update interval
spark.history.fs.update.interval 10s

# the web ui port 
spark.history.ui.port 18080
```
Enable by setting the configuration spark.eventLog.enabled to true.
Specify where to store the event log history using spark.history.fs.logDirectory and spark.eventLog.dir, by default the location is file:///tmp/spark-events. You need to create the directory in advance.
