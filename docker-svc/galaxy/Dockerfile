FROM quay.io/bgruening/galaxy:20.09
LABEL Maintainer="combat-sars-cov-2-help@sanbi.ac.za"

WORKDIR /galaxy-central

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -

RUN apt update && apt install -y netcat-openbsd apt-utils curl

RUN add-tool-shed --url 'http://testtoolshed.g2.bx.psu.edu/' --name 'Test Tool Shed'; \
	add-tool-shed --url 'https://irida.corefacility.ca/galaxy-shed/' --name 'IRIDA Tool Shed'

ENV GALAXY_CONFIG_BRAND="COMBAT-SARS-COV-2 Workbench" \
	GALAXY_CONFIG_CONDA_ENSURE_CHANNELS="iuc,conda-forge,bioconda,defaults" 

# install mamba for faster conda installs
RUN /tool_deps/_conda/bin/conda install -c conda-forge -y mamba

RUN sed -i -e 's/# *admin_users: .*/admin_users: admin@galaxy.org/' /etc/galaxy/galaxy.yml; \
	sed -i -e 's/#\( *allow_path_paste:\).*/\1 true/' /etc/galaxy/galaxy.yml; \
	sed -i -e 's/# *conda_ensure_channels: .*/conda_ensure_channels: iuc,conda-forge,bioconda,defaults/' /etc/galaxy/galaxy.yml; \
	sed -i -e 's^#conda_exec: null^conda_exec: /tool_deps/_conda/bin/mamba^' /etc/galaxy/galaxy.yml

# Adding the tool definitions and data manager to the container
COPY combat-sars-cov-2-tools.yml primer_scheme_data_manager.yml $GALAXY_ROOT/

# Install Combat-SARS-COV-2 Tools
# most tall installation has been disabled for now as this takes too long to run and it is
# quicker and more reliable to run install with ephemeris afterwards
RUN install-tools $GALAXY_ROOT/combat-sars-cov-2-tools.yml && \
	/tool_deps/_conda/bin/conda clean --tarballs --yes > /dev/null && \
    rm /export/galaxy-central/ -rf && \
    mkdir -p $GALAXY_HOME/workflows

# Add galaxy to admin group
RUN usermod -a -G root galaxy

# Install data-manager (data_manager_snpeff)
RUN /tool_deps/_conda/bin/conda install ephemeris
RUN startup_lite && \
	/tool_deps/_conda/bin/run-data-managers -u admin@galaxy.org -p $GALAXY_DEFAULT_ADMIN_PASSWORD \
        -g http://localhost:8080 --config primer_scheme_data_manager.yml

# Container Style
COPY *.png welcome.html $GALAXY_CONFIG_DIR/web/
