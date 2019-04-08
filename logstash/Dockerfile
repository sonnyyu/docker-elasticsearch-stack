# https://github.com/elastic/logstash-docker
FROM docker.elastic.co/logstash/logstash:6.7.1


COPY ./config/logstash.yml  /usr/share/logstash/config/logstash.yml
COPY ./pipeline/logstash.conf  /usr/share/logstash/pipeline/logstash.conf

# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json
