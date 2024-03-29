ARG r_version
ARG os_version

FROM ghcr.io/open-systems-pharmacology/base-r${r_version}-${os_version}:latest

# Args are resetted by FROM - thus need to redeclare! s. https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG r_version
ARG os_version
ARG tlf_version
ARG ospsuite_r_version
ARG rclr_version
ARG ospsuiteutils_version

ENTRYPOINT ["sh"]

# Copy installation scripts
RUN mkdir -p /tmp_setup 
COPY .github/workflows/*.R /tmp_setup/
COPY .github/workflows/*.sh /tmp_setup/
RUN chmod ugo+x /tmp_setup/*.sh

# Install CRAN packages
RUN R --no-save -e "source('/tmp_setup/install_packages.R')"

# Install OSP R packages
RUN /tmp_setup/install_packages_r${r_version}_${os_version}.sh ${rclr_version} ${tlf_version} ${ospsuite_r_version} ${ospsuiteutils_version} 

# Cleanup
RUN rm -rf /tmp_setup

# run package tests
#RUN R --no-save -e "setwd(system.file('tests', package = 'rClr')); source('testthat.R')"
RUN R --no-save -e "setwd(system.file('tests', package = 'tlf')); source('testthat.R')"
#RUN R --no-save -e "setwd(system.file('tests', package = 'ospsuite')); source('testthat.R')"
RUN R --no-save -e "setwd(system.file('tests', package = 'ospsuite')); unlink('testthat/test-utilities-data-set.R'); source('testthat.R')"
