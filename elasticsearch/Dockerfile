ARG ELK_VERSION

FROM elasticsearch:$ELK_VERSION

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu

WORKDIR /usr/share/elasticsearch/plugins
COPY plugins/elasticsearch-analysis-ik-7.11.1.zip analysis-ik.zip
RUN unzip analysis-ik.zip
RUN rm analysis-ik.zip
