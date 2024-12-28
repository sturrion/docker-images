# Use the minimal base image
FROM redhat/ubi9-minimal:9.5

# Install Python3, pip, and the necessary libraries, and clean up temporary files
RUN microdnf install -y python3 && \
    microdnf clean all && \
    curl -sSL https://bootstrap.pypa.io/get-pip.py | python3 && \
    pip3 install --no-cache-dir --upgrade setuptools && \
    pip3 install --no-cache-dir pandas numpy jupyter yfinance && \
    python3 -m pip cache purge

CMD ["python3"]
