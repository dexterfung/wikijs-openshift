FROM requarks/wiki:latest

# Replace with your email address:
ENV WIKI_ADMIN_EMAIL replacethis@email.address


RUN chgrp -R 0 /var/wiki && chmod -R g=u /var/wiki

WORKDIR /var/wiki

ADD your-config.yml config.yml

EXPOSE 3000
ENTRYPOINT [ "node", "server" ]