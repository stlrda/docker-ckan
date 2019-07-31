FROM openknowledge/ckan-base:2.8

MAINTAINER Your Name Here <you@example.com>

# Set timezone
ARG TZ
RUN echo ${TZ:-UTC} > /etc/timezone
RUN cp /usr/share/zoneinfo/${TZ:-UTC}  /etc/localtime

# Install any extensions needed by your CKAN instance
# (Make sure to add the plugins to CKAN__PLUGINS in the .env file)
# For instance:
#RUN pip install -e git+https://github.com/ckan/ckanext-pages.git#egg=ckanext-pages && \
#    pip install -e git+https://github.com/ckan/ckanext-dcat.git@v0.0.6#egg=ckanext-dcat && \
#    pip install -r https://raw.githubusercontent.com/ckan/ckanext-dcat/v0.0.6/requirements.txt

RUN source /usr/lib/ckan/default/bin/activate

    pip install ckanext-pdfview
    #https://github.com/ckan/ckanext-pdfview

#    pip install -e git+https://github.com/ckan/ckanext-scheming
#    #https://github.com/ckan/ckanext-scheming
#
#    pip install ckanext-showcase
#    #https://github.com/azavea/ckanext-showcase
#
#    pip install ckanext-geoview
#    #https://github.com/pduchesne/ckanext-geoview
#
#    pip install -e 'git+https://github.com/ckan/ckanext-pages.git#egg=ckanext-pages'
#    #https://github.com/ckan/ckanext-pages
#
#    pip install -e git+https://github.com/ckan/ckanext-harvest.git#egg=ckanext-harvest
#        cd /usr/lib/ckan/default/src/ckanext-harvest/
#        pip install -r pip-requirements.txt
#    #https://github.com/ckan/ckanext-harvest
#
#    pip install -e git+https://github.com/ckan/ckanext-dcat.git#egg=ckanext-dcat
#        pip install -r ckanext-dcat/requirements.txt
#    #https://github.com/ckan/ckanext-dcat
#
#    sass ckanext/odp_theme/public/css/sass/main.scss ckanext/odp_theme/public/css/main.css
#       #https://github.com/azavea/ckanext-odp_theme


# Install the extension(s) you wrote for your own project
# RUN pip install -e git+https://github.com/your-org/ckanext-your-extension.git@v1.0.0#egg=ckanext-your-extension

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
