FROM 1science/java:oracle-jre-8

ENV SBT_VERSION 0.13.13
ENV SBT_HOME /usr/local/sbt-launcher-packaging-${SBT_VERSION}
ENV PATH ${PATH}:${SBT_HOME}/bin


# install sbt
RUN curl -sL "http://dl.bintray.com/sbt/native-packages/sbt/$SBT_VERSION/sbt-$SBT_VERSION.tgz" | gunzip | tar -x -C /usr/local && \
    echo -ne "- with sbt $SBT_VERSION\n" >> /root/.built

RUN echo "Asia/Harbin" > /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata

ONBUILD COPY ./project /data/project
ONBUILD COPY ./build.sbt /data/build.sbt
ONBUILD COPY . /data

# build and test
ONBUILD RUN cd /data \
	&& sbt -Dsbt.override.build.repos=true -Dfile.encoding=UTF-8 test \
	&& sbt -Dsbt.override.build.repos=true -Dfile.encoding=UTF-8 dist \
	&& cd /data/target/universal/ && unzip *.zip

# run cron and project
ONBUILD RUN cd /data \
    && export proj_name=`sbt settings name | tail -1 | cut -d' ' -f2 |tr -dc [:print:] | sed 's/\[0m//g'` && \
	mkdir -p /release/${proj_name} && mv /data/target/universal/${proj_name}* /release && \
	cd /release/${proj_name}*/bin && \
	ln -s `pwd`/$proj_name /entrypoint

# cleanup
ONBUILD RUN rm -r /data

ONBUILD CMD ["/entrypoint", "-Dconfig.resource=prod.conf", "-Dfile.encoding=UTF8"]

