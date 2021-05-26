FROM centos:centos7

#########################################
##        BUILD-TIME VARIABLES        ##
#########################################
# url for Network License Manager
#ARG FLEXLM_URL=
# path for temporary files
ARG TEMP_PATH=/tmp/flexnetserver/

#########################################
##        ENVIRONMENTAL CONFIG         ##
#########################################
# add the flexlm commands to $PATH
#ENV PATH="$PATH:/opt/FNPLicenseServerManager/"
#ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/opt/FNPLicenseServerManager/"

#########################################
##         RUN INSTALL SCRIPT          ##
#########################################
COPY *.sh /usr/local/bin/
COPY *.zip $TEMP_PATH
COPY silent.properties "${TEMP_PATH}installer.properties"

RUN yum install -y java tomcat tomcat-webpages unzip && \
    yum clean all && \
    rm -rf /var/cache/yum

WORKDIR $TEMP_PATH
user root
#RUN wget --progress=bar:force -- $FLEXLM_URL
RUN unzip *.zip
RUN chmod +x setup.bin
RUN ./setup.bin -i console -f installer.properties
#RUN FlexNetLicenseServerTools/install_fnp.sh ./FlexNetLicenseServerTools/FNPLicensingService/FNPLicensingService
#RUN rm -rf $TEMP_PATH

# lmadmin is required for -2 -p flag support
RUN groupadd -r lmadmin && \
    useradd -r -g lmadmin lmadmin

#RUN ./FlexNetLmadminInstaller/lmadmin-i86_lsb-11_14_0_0.bin 
#RUN cp FlexNetLicenseServerTools/mcslmd* /opt/FNPLicenseServerManager/
#RUN chown -R lmadmin:lmadmin /opt/FNPLicenseServerManager/ && \
#    chmod +x /opt/FNPLicenseServerManager/lmadmin

#########################################
##              VOLUMES                ##
#########################################
#VOLUME ["/var/flexlm"]

#########################################
##            EXPOSE PORTS             ##
#########################################
EXPOSE 8080
EXPOSE 7070

# do not use ROOT user
USER lmadmin

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
#CMD /opt/FNPLicenseServerManager/lmadmin -foreground
