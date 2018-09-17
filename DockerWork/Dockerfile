# Pre-reqs:
# mkdir /root/hail
# cd /root/hail
# wget https://repo.continuum.io/archive/Anaconda2-5.1.0-Linux-x86_64.sh
# wget https://archive.apache.org/dist/spark/spark-2.1.0/spark-2.1.0-bin-hadoop2.7.tgz
# wget https://storage.googleapis.com/hail-common/distributions/0.1/Hail-0.1-20613ed50c74-Spark-2.1.0.zip
# git clone --branch 0.1 https://github.com/broadinstitute/hail.git
# cd hail
# ./gradlew -Dspark.version=2.1.0 shadowJar
# cd /root/hail/
# zip -r hail.zip hail/

# Use Java 8 as the base image

FROM java:8

WORKDIR /hail

ADD Anaconda2-5.1.0-Linux-x86_64.sh /hail/
RUN sh Anaconda2-5.1.0-Linux-x86_64.sh -b -p /hail/anaconda2

ADD spark-2.1.0-bin-hadoop2.7.tgz /hail/

ADD hail.zip /hail/
RUN unzip hail.zip

ENV SPARK_HOME=/hail/spark-2.1.0-bin-hadoop2.7
ENV HAIL_HOME=/hail/hail
ENV ANACONDA_HOME=/hail/anaconda2

ENV PATH=$PATH:$ANACONDA_HOME:$GRADLE_HOME:$HAIL_HOME:$SPARK_HOME

#HTTP_PROXY    ENV HTTP_PROXY "http://127.0.0.1:8888"

ENV PATH=$PATH:$HAIL_HOME/bin
ENV PATH=$PATH:/hail/anaconda2/bin

RUN jupyter notebook --generate-config

RUN sed -i 's,localhost,0.0.0.0,g' /root/.jupyter/jupyter_notebook_config.py
RUN sed -i 's,#c.NotebookApp.ip,c.NotebookApp.ip,g' /root/.jupyter/jupyter_notebook_config.py

ENTRYPOINT ["jhail","--allow-root"]
