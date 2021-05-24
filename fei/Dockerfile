FROM centos:centos7

#########################################
##        BUILD-TIME VARIABLES        ##
#########################################
# url for Network Licence Manager
ARG FLEXLM_URL=https://download.amira-avizo-software.com/private/LICENSING/FLEXnet/11.14.0.0/flexnet-server_11.14.0.0_linux-x64-gcc44.tar.gz
# path for temporary files
ARG TEMP_PATH=/tmp/flexnetserver/

#########################################
##        ENVIRONMENTAL CONFIG         ##
#########################################
# add the flexlm commands to $PATH
ENV PATH="$PATH:/opt/FNPLicenseServerManager/"
ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/opt/FNPLicenseServerManager/"

#########################################
##         RUN INSTALL SCRIPT          ##
#########################################
#COPY *.sh /usr/local/bin/
COPY silent.properties "${TEMP_PATH}FlexNetLmadminInstaller/installer.properties"

RUN yum install -y redhat-lsb-core.i686 wget && yum clean all

WORKDIR $TEMP_PATH
RUN wget --progress=bar:force -- $FLEXLM_URL
RUN tar -zxvf ./*.tar.gz
RUN FlexNetLicenseServerTools/install_fnp.sh ./FlexNetLicenseServerTools/FNPLicensingService/FNPLicensingService
#RUN rm -rf $TEMP_PATH

# lmadmin is required for -2 -p flag support
RUN groupadd -r lmadmin && \
    useradd -r -g lmadmin lmadmin

RUN ./FlexNetLmadminInstaller/lmadmin-i86_lsb-11_14_0_0.bin 
RUN chown -R lmadmin:lmadmin /opt/FNPLicenseServerManager/
RUN chmod +x /opt/FNPLicenseServerManager/lmadmin

#########################################
##              VOLUMES                ##
#########################################
#VOLUME ["/var/flexlm"]

#########################################
##            EXPOSE PORTS             ##
#########################################
EXPOSE 8090
EXPOSE 27000-27009

# do not use ROOT user
USER lmadmin

#ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD /opt/FNPLicenseServerManager/lmadmin -foreground
# no CMD, use container as if 'lmgrd'
