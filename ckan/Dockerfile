FROM ckan/ckan-base:2.10-py3.10

USER root

ENV APP_DIR=/srv/app
ENV PATH="${APP_DIR}/bin:$PATH"

# Install any extensions needed by your CKAN instance
# See Dockerfile.dev for more details and examples


RUN mkdir -p ${APP_DIR}/patches
RUN mkdir -p ${APP_DIR}/docker-entrypoint.d

# Copy custom initialization scripts
COPY --chown=ckan-sys:ckan-sys ./ckan/docker-entrypoint.d/* ${APP_DIR}/docker-entrypoint.d/


# Apply any patches needed to CKAN core or any of the built extensions (not the
# runtime mounted ones)
COPY --chown=ckan-sys:ckan-sys ./ckan/patches ${APP_DIR}/patches

RUN apt-get update && apt-get install -y python3-dev libpq-dev python3-pip python3-venv git-core redis-server libmagic1 \
 && rm -rf /var/lib/apt/lists/*

RUN python3 -m venv /srv/app/



WORKDIR /srv/app/src

RUN chown -R ckan /srv/app


# Clonar extensiones
#RUN git clone https://github.com/ckan/ckan.git
RUN git clone https://github.com/ckan/ckanext-harvest.git
RUN cp -r ckanext-harvest/ckanext/harvest /srv/app/src/ckan/ckanext/harvest

RUN git clone https://github.com/ckan/ckanext-dcat.git
RUN cp -r ckanext-dcat/ckanext/dcat /srv/app/src/ckan/ckanext/dcat
#RUN git clone https://github.com/ckan/ckanext-envvars.git

# Asegurar setuptools en el virtualenv de CKAN (sin recrearlo)
RUN /srv/app/bin/pip uninstall -y rdflib rdflib-jsonld
RUN /srv/app/bin/pip install --upgrade pip setuptools==65.5.1 rdflib==6.3.2 rdflib-jsonld==0.6.2



# Instalar requirements de harvest
RUN /srv/app/bin/pip install -r ckan/requirements.txt
RUN /srv/app/bin/pip install -r ckanext-harvest/requirements.txt
RUN /srv/app/bin/pip install -r ckanext-envvars/dev-requirements.txt
RUN /srv/app/bin/pip install -r ckanext-dcat/requirements.txt


# Instalar extensiones en modo editable
RUN /srv/app/bin/pip install -e ckan
RUN /srv/app/bin/pip install -e ckanext-harvest
RUN /srv/app/bin/pip install -e ckanext-envvars
RUN /srv/app/bin/pip install -e ckanext-dcat

# Permisos
RUN chown -R ckan /srv/app

USER ckan
WORKDIR /srv/app

CMD ["ckan", "-c", "/srv/app/ckan.ini", "run"]
