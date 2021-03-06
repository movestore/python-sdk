########################################################################################################################
# co-pilot: R-characterstic.
# This co-pilot implements the `R`-characterstic. Concrete MoveApps R apps could use this image ase its base.
########################################################################################################################

FROM rocker/geospatial:4.1.2

LABEL maintainer = "couchbits GmbH <us@couchbits.com>"

WORKDIR /root/app
# copy the R co-pilot to the image
# the co-pilot-r-part
COPY app.R Helper.R RFunction.R start-process.sh ./
# renv
COPY renv.lock .Rprofile ./
COPY renv/activate.R ./renv/
ENV RENV_VERSION 0.14.0
RUN R -e "install.packages('remotes', repos = c(CRAN = 'https://cloud.r-project.org'))"
RUN R -e "remotes::install_github('rstudio/renv@${RENV_VERSION}')"
# the R packages needed by the base image
RUN R -e 'renv::restore()'

# the R packages needed by this specific app
RUN R -e 'remotes::install_version("lubridate")'

# take a snapshot of all R dependencies
RUN R -e 'renv::snapshot()'

# start the MoveApps app
ENTRYPOINT ["./start-process.sh"]
