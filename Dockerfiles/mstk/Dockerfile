ARG OS
ARG IMAGE_NAME=mstk-${OS}-base

FROM $IMAGE_NAME

ARG TOOLKIT_VERSION=1.4.0
ARG REQUIREMENTS_FILE

LABEL maintainer='Samuel Eklund sam@llnl.gov'

# Set environment variables
ENV dockerfile_folder Dockerfiles
ENV V_TOOLKIT_VERSION v$TOOLKIT_VERSION
ENV zip_file Toolkit_zip_files/MI_ScriptingToolkit_${V_TOOLKIT_VERSION}.zip
ENV toolkit_folder MIScriptingToolkit-${TOOLKIT_VERSION}
ENV toolkit_gzip ${toolkit_folder}.tar.gz
ENV test_install test_toolkit.py
ENV toolkit_dir /mstk

# Copy and install toolkit
RUN mkdir $toolkit_dir
WORKDIR $toolkit_dir
COPY $zip_file $zip_file
RUN unzip $zip_file
# Note: $os_folder var comes from OS-specific Dockerfile
WORKDIR /$toolkit_dir/$V_TOOLKIT_VERSION/$os_folder/
RUN tar -xf $toolkit_gzip
WORKDIR /$toolkit_dir/$V_TOOLKIT_VERSION/$os_folder/$toolkit_folder
RUN python3 setup.py install

# 
RUN mkdir /requirements
WORKDIR /requirements
COPY $dockerfile_folder/Dockerfile.mstk $REQUIREMENTS_FILE* ./
RUN if [ -f $REQUIREMENTS_FILE ] ; then echo "file exists" && ls | grep *.txt ; else echo "file does not exist" ; fi
# pip3 install -r $REQUIREMENTS_FILE ; fi

# Copy test script
COPY $test_install /$toolkit_dir/$test_install

CMD python3 /$toolkit_dir/$test_install