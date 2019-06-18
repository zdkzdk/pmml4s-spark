# PMML4S-Spark
_PMML4S-Spark_ is a Spark transformer takes in PMML (Predictive Model Markup Language).

## Features
_PMML4S-Spark_ is a Spark wrapper of _PMML4S_, you can see [PMML4S](https://github.com/autodeployai/pmml4s) for details.

## Installation
_PMML4S-Spark_ is available from maven central.

Latest release: [![Maven Central](https://maven-badges.herokuapp.com/maven-central/org.pmml4s/pmml4s-spark_2.12/badge.svg)](https://maven-badges.herokuapp.com/maven-central/org.pmml4s/pmml4s-spark_2.12)

##### SBT users
```scala
libraryDependencies += "org.pmml4s" %%  "pmml4s-spark" % "0.9.1"
```

##### Maven users
```xml
<dependency>
  <groupId>org.pmml4s</groupId>
  <artifactId>pmml4s-spark_${scala.version}</artifactId>
  <version>0.9.1</version>
</dependency>
```

## Usage
1. Load model.

    ```scala
    import scala.io.Source
    import org.pmml4s.model.Model
    import org.pmml4s.spark.ScoreModel

    // The main constructor accepts a Model, and there are other help methods accept different sources.
    val scoreModel = ScoreModel(Model(Source.fromURL(new java.net.URL("http://dmg.org/pmml/pmml_examples/KNIME_PMML_4.1_Examples/single_iris_dectree.xml"))))
    ```

2. Call `transform(dataset)` to run a batch score against an input dataset.

    ```scala
    // The data is from http://dmg.org/pmml/pmml_examples/Iris.csv
    val df = spark.read.
      format("csv").
      options(Map("header" -> "true", "inferSchema" -> "true")).
      load("Iris.csv")
 
    val scoreDf = scoreModel.transform(df)
    scala> scoreDf.show(5)
    +------------+-----------+------------+-----------+-----------+--------------+-----------+-----------------------+---------------------------+--------------------------+-------+
    |sepal_length|sepal_width|petal_length|petal_width|      class|PredictedValue|Probability|Probability_Iris-setosa|Probability_Iris-versicolor|Probability_Iris-virginica|Node_ID|
    +------------+-----------+------------+-----------+-----------+--------------+-----------+-----------------------+---------------------------+--------------------------+-------+
    |         5.1|        3.5|         1.4|        0.2|Iris-setosa|   Iris-setosa|        1.0|                    1.0|                        0.0|                       0.0|      1|
    |         4.9|        3.0|         1.4|        0.2|Iris-setosa|   Iris-setosa|        1.0|                    1.0|                        0.0|                       0.0|      1|
    |         4.7|        3.2|         1.3|        0.2|Iris-setosa|   Iris-setosa|        1.0|                    1.0|                        0.0|                       0.0|      1|
    |         4.6|        3.1|         1.5|        0.2|Iris-setosa|   Iris-setosa|        1.0|                    1.0|                        0.0|                       0.0|      1|
    |         5.0|        3.6|         1.4|        0.2|Iris-setosa|   Iris-setosa|        1.0|                    1.0|                        0.0|                       0.0|      1|
    +------------+-----------+------------+-----------+-----------+--------------+-----------+-----------------------+---------------------------+--------------------------+-------+
    only showing top 5 rows
    ```

## Use in PySpark
See the [PyPMML-Spark](https://github.com/autodeployai/pypmml-spark) project.

## Support
If you have any questions about the _PMML4S-Spark_ library, please open issues on this repository.

Feedback and contributions to the project, no matter what kind, are always very welcome. 

## License
_PMML4S-Spark_ is licensed under [APL 2.0](http://www.apache.org/licenses/LICENSE-2.0).