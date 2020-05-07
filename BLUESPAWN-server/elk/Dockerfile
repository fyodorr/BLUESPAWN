FROM sebp/elk:latest

# Install certificates
RUN mkdir /certs
COPY configureSSL.sh /certs/configureSSL.sh
COPY runCertGen.sh /certs/runCertGen.sh
COPY createCA.sh /certs/createCA.sh
COPY createNodeCerts.sh /certs/createNodeCerts.sh
RUN chmod +x /certs/configureSSL.sh; chmod +x /certs/runCertGen.sh; chmod +x /certs/createCA.sh; chmod +x /certs/createNodeCerts.sh; /certs/configureSSL.sh

# Configure passwords
RUN cp /usr/local/bin/start.sh /usr/local/bin/runElk.sh
RUN sed -i '$ d' /usr/local/bin/runElk.sh
COPY configurePasswords.sh configurePasswords.sh
RUN ES_PROTOCOL=https /usr/local/bin/runElk.sh; chmod +x configurePasswords.sh; ./configurePasswords.sh
RUN rm configurePasswords.sh
RUN rm /usr/local/bin/runElk.sh
RUN service logstash stop
RUN service kibana stop
RUN service elasticsearch stop

CMD ["/bin/bash", "-c", "ES_PROTOCOL=https /usr/local/bin/start.sh"]