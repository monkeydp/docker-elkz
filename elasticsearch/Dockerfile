ARG ELK_VERSION

FROM elasticsearch:$ELK_VERSION

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu

COPY plugins/elasticsearch-analysis-ik-7.11.1.zip /analysis-ik.zip
RUN bash -c '/bin/echo -e "y" | elasticsearch-plugin install file:///analysis-ik.zip'
RUN rm /analysis-ik.zip
