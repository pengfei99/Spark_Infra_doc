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

> If your spark is running on cluster mode, the default local file system such as `file:///tmp/spark-events` will not 
   work. You need to set up a network file system. And don't forget to create the directory in advance.

## Running the spark history server

Use the below command to run the spark history server

```shell
# in linux
bash $SPARK_HOME/sbin/start-history-server.sh
bash $SPARK_HOME/sbin/stop-history-server.sh

# in windows
$SPARK_HOME/bin/spark-class.cmd org.apache.spark.deploy.history.HistoryServer
```

> Spark will create a sub-directory for each application and logs the events specific to the application in this directory.

## Visualize the History server UI

In our config, we specify that `History server` listens at 18080 port. So we can access the history server UI by
using http://localhost:18080/