FROM  qgis/qgis3-build-deps:final-3_32_2

LABEL Description="Docker container with QGIS" Vendor="QGIS.org" Version="1.1"

ENV LANG=C.UTF-8

WORKDIR /ccache_dir
ENV CCACHE_DIR=/ccache_dir
#RUN ccache -M 1G && ccache -s

# Additional run-time dependencies and gdb for debugging qgis
RUN pip3 install jinja2 pygments pexpect && apt install -y expect gdb

################################################################################
# Python testing environment setup

# Add QGIS test runner
COPY qgis_resources/test_runner/qgis_* /usr/bin/

# Make all scripts executable
RUN chmod +x /usr/bin/qgis_*

# Add supervisor service configuration script
COPY qgis_resources/supervisor/ /etc/supervisor

# Python paths are for
# - kartoza images (compiled)
# - deb installed
# - built from git
# needed to find PyQt wrapper provided by QGIS
ENV PYTHONPATH=/usr/share/qgis/python/:/usr/share/qgis/python/plugins:/usr/lib/python3/dist-packages/qgis:/usr/share/qgis/python/qgis

# Run supervisor for xvcd
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
