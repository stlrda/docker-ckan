FROM openknowledge/ckan-base:2.8

MAINTAINER Your Name Here <you@example.com>

# Set timezone
ARG TZ
RUN echo ${TZ:-UTC} > /etc/timezone
RUN cp /usr/share/zoneinfo/${TZ:-UTC}  /etc/localtime

COPY production.ini ${APP_DIR}

# Install any extensions needed by your CKAN instance
# (Make sure to add the plugins to CKAN__PLUGINS in the .env file)
# For instance:
RUN pip install -e git+https://github.com/ckan/ckanext-pages.git#egg=ckanext-pages && \
        #https://github.com/ckan/ckanext-pages
    apk add vim && \
    pip install ckanext-pdfview && \
        #https://github.com/ckan/ckanext-pdfview
    pip install ckanext-geoview && \
        #https://github.com/pduchesne/ckanext-geoview
    pip install -e 'git+https://github.com/ckan/ckanext-scheming.git#egg=ckanext-scheming' && \
        #https://github.com/ckan/ckanext-scheming
    pip install -e 'git+https://github.com/azavea/ckanext-showcase.git#egg=ckanext-showcase' && \
        #https://github.com/azavea/ckanext-showcase
    pip install -e 'git+https://github.com/stlrda/ckanext-odp_theme.git#egg=ckanext-odp_theme' && \
        #https://github.com/stlrda/ckanext-odp_theme
    pip install -e 'git+https://github.com/ckan/ckantoolkit.git#egg=ckantoolkit' && \
        #https://github.com/ckan/ckantoolkit
    pip install ckanapi
        #https://github.com/ckan/ckanapi
#    pip install -e git+https://github.com/ckan/ckanext-dcat.git@v0.0.6#egg=ckanext-dcat && \
#    pip install -r https://raw.githubusercontent.com/ckan/ckanext-dcat/v0.0.6/requirements.txt

# Install the extension(s) you wrote for your own project
# RUN pip install -e git+https://github.com/your-org/ckanext-your-extension.git@v1.0.0#egg=ckanext-your-extension

# run this after the plugins are installed so it overwrites the default in the plugin directory
COPY schemas/ ${APP_DIR}/src/ckanext-scheming/ckanext/scheming/

# favicon
COPY resources/favicon/serve/ ${APP_DIR}/src/ckanext-odp-theme/ckanext/odp_theme/public

# Apply any patches needed to CKAN core or any of the built extensions (not the
# runtime mounted ones)
# See https://github.com/okfn/docker-ckan#applying-patches
COPY patches ${APP_DIR}/patches

RUN for d in $APP_DIR/patches/*; do \
        if [ -d $d ]; then \
            for f in `ls $d/*.patch | sort -g`; do \
                cd $SRC_DIR/`basename "$d"` && echo "$0: Applying patch $f to $SRC_DIR/`basename $d`"; patch -p1 < "$f" ; \
            done ; \
        fi ; \
    done
