Below is the instructions for using the WikiEdit custom Spark V2 data source for analyzing wiki edits in spark-shell.
* Clone this repository on your local machine - git clone https://github.com/hienluu/structured-streaming-sources-binaries.git
* Start up spark-shell with --jars argument to include the streaming_sources-assembly-0.0.1.jar jar file into Spark driver and executor class path
  * spark-shell --jars <path>/streaming_sources-assembly-0.0.1.jar
* Once spark-shell is successfully started, try the following snippet of code

```
val provideClassName = "org.structured_streaming_sources.wikedit.WikiEditSourceV2"

val wikiEditDF = spark.readStream.format(provideClassName).option("channel", "#en.wikipedia").load()

val wikiEditSmallDF = wikiEditDF.select("timestamp", "user", "channel", "title")

val wikiEditQS = wikiEditSmallDF.writeStream.format("console").option("truncate", "false").start()

// wait for a few seconds before the wiki edits to show up in console

// when you want to stop
wikiEditQS.stop

```
* en_wikipedia represents English Wikipedia channel. You can try #es.wikipedia, #ja.wikipedia, #fr.wikipedia
  * A list of channels are available at https://en.wikipedia.org/wiki/Wikipedia:IRC

The source code for this custom data source is available in Github repository - https://github.com/hienluu/structured-streaming-sources

