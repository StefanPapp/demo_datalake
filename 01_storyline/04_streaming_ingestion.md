# simple maven
mkdir /workspace
cd /workspace

## build training exercises
git clone https://github.com/dataArtisans/flink-training-exercises.git
cd flink-training-exercises
mvn clean install

## generate demo environment
mvn archetype:generate                             \
    -DarchetypeGroupId=org.apache.flink            \
    -DarchetypeArtifactId=flink-quickstart-java    \
    -DarchetypeVersion=1.1.2                       \
    -DgroupId=org.apache.flink.quickstart          \
    -DartifactId=flink-java-project                \
    -Dversion=0.1                                  \
    -Dpackage=org.apache.flink.quickstart          \
    -DinteractiveMode=false

in flink-java-project/pom.xml
<dependency>
  <groupId>com.dataartisans</groupId>
  <artifactId>flink-training-exercises</artifactId>
  <version>0.5</version>
</dependency>

mvn clean package

##test
    flink run -c org.apache.flink.quickstart.WordCount ./target/flink-java-project-0.1.jar

 flink run -c com.dataartisans.flinktraining.exercises.datastream_java.connectors.PopularPlacesToES flink-java-project-0.1.jar -input /data/nycTaxiRides.gz