FROM python:3.10.0b4
LABEL maintainer=tom.g.close@gmail.com

RUN apt-get update && apt-get install -y \
        curl \
        pigz \
        zip \
        && \
    pip install \
        pydicom \
        nipype \
        requests \
        && \
    rm -r ${HOME}/.cache/pip && \
    curl -L https://github.com/rordenlab/dcm2niix/releases/download/v1.0.20210317/dcm2niix_lnx.zip > dcm2niix.zip && \
    unzip dcm2niix.zip && \
    mv dcm2niix /usr/local/bin && \
    chmod a+x /usr/local/bin/dcm2niix && \
    rm dcm* && \
    apt-get remove -y \
        curl \
        zip \
        && \
    apt-get autoremove -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir /src
COPY dcm2bids_wholeSession.py /src/dcm2bids_wholeSession.py

WORKDIR /src

LABEL org.nrg.commands="[{\"inputs\": [{\"command-line-flag\": \"--session\", \"name\": \"session_id\", \"required\": true, \"replacement-key\": \"#SESSION_ID#\", \"type\": \"string\", \"description\": \"XNAT ID of the session\"}, {\"command-line-flag\": \"--project\", \"name\": \"project_id\", \"required\": true, \"replacement-key\": \"#PROJECT_ID#\", \"type\": \"string\", \"description\": \"XNAT ID of the project\"}, {\"command-line-flag\": \"--overwrite\", \"name\": \"overwrite\", \"default-value\": false, \"false-value\": \"False\", \"required\": false, \"true-value\": \"True\", \"replacement-key\": \"#OVERWRITE#\", \"type\": \"boolean\", \"description\": \"Overwrite any existing NIFTI and BIDS scan resources?\"}], \"workdir\": \"/src\", \"name\": \"dcm2bids-session\", \"command-line\": \"python dcm2bids_wholeSession.py #SESSION_ID# #PROJECT_ID# #OVERWRITE# --host \$XNAT_HOST --user \$XNAT_USER --pass \$XNAT_PASS --upload-by-ref False --dicomdir /dicom --niftidir /nifti\", \"outputs\": [], \"image\": \"australianimagingservice/dcm2bids-session:1.0.2\", \"override-entrypoint\": true, \"version\": \"1.0.4\", \"schema-version\": \"1.0\", \"xnat\": [{\"derived-inputs\": [{\"provides-value-for-command-input\": \"session_id\", \"name\": \"session-id\", \"derived-from-xnat-object-property\": \"id\", \"user-settable\": false, \"derived-from-wrapper-input\": \"session\", \"type\": \"string\", \"description\": \"The session's id\"}, {\"provides-value-for-command-input\": \"project_id\", \"name\": \"project\", \"derived-from-xnat-object-property\": \"project-id\", \"required\": true, \"user-settable\": false, \"derived-from-wrapper-input\": \"session\", \"type\": \"string\"}], \"contexts\": [\"xnat:imageSessionData\"], \"description\": \"Convert DICOM-to-NIfTI and generate BIDS metadata\", \"output-handlers\": [], \"external-inputs\": [{\"required\": true, \"type\": \"Session\", \"name\": \"session\", \"description\": \"Input session\"}], \"name\": \"dcm2bids-session\"}], \"mounts\": [{\"writable\": \"true\", \"path\": \"/nifti\", \"name\": \"nifti\"}], \"type\": \"docker\", \"description\": \"Runs dcm2niix on a session's scans, and uploads the nifti and bids json\"}]"