FROM python:3.12-slim-bookworm

LABEL maintainer="co-l"
LABEL description="NiceFox Secu — lightweight pentesting toolkit for web developers"
ARG NICEFOX_VERSION=dev
LABEL version="${NICEFOX_VERSION}"

# System packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    nmap \
    hydra \
    curl \
    wget \
    git \
    unzip \
    procps \
    perl \
    libnet-ssleay-perl \
    ruby \
    ruby-dev \
    build-essential \
    nodejs \
    npm \
    bsdmainutils \
    libssl-dev \
    libgmp-dev \
    chromium \
    chromium-driver \
    fonts-liberation \
    && rm -rf /var/lib/apt/lists/*

# nikto (Perl-based web server scanner)
RUN git clone --depth 1 https://github.com/sullo/nikto.git /opt/nikto && \
    ln -s /opt/nikto/program/nikto.pl /usr/local/bin/nikto

# whatweb (Ruby-based tech fingerprinting)
RUN git clone --depth 1 https://github.com/urbanadventurer/WhatWeb.git /opt/whatweb && \
    gem install --no-document addressable && \
    ln -s /opt/whatweb/whatweb /usr/local/bin/whatweb

# Python tools
RUN pip install --no-cache-dir \
    sqlmap \
    arjun \
    httpie \
    commix \
    graphw00f \
    graphql-cop \
    clairvoyance \
    mitmproxy \
    wafw00f \
    droopescan \
    theHarvester \
    slither-analyzer \
    mythril \
    semgrep \
    solc-select \
    web3

# testssl.sh
RUN git clone --depth 1 https://github.com/drwetter/testssl.sh.git /opt/testssl && \
    ln -s /opt/testssl/testssl.sh /usr/local/bin/testssl

# jwt_tool
RUN git clone --depth 1 https://github.com/ticarpi/jwt_tool.git /opt/jwt_tool && \
    pip install --no-cache-dir -r /opt/jwt_tool/requirements.txt && \
    chmod +x /opt/jwt_tool/jwt_tool.py && \
    ln -s /opt/jwt_tool/jwt_tool.py /usr/local/bin/jwt_tool

# Go binaries (pre-built from GitHub releases)
ARG TARGETARCH=amd64
RUN ARCH=$([ "$TARGETARCH" = "arm64" ] && echo "arm64" || echo "amd64") && \
    GITLEAKS_ARCH=$([ "$TARGETARCH" = "arm64" ] && echo "arm64" || echo "x64") && \
    # ffuf
    wget -q "https://github.com/ffuf/ffuf/releases/download/v2.1.0/ffuf_2.1.0_linux_${ARCH}.tar.gz" -O /tmp/ffuf.tar.gz && \
    tar -xzf /tmp/ffuf.tar.gz -C /usr/local/bin ffuf && \
    # nuclei
    wget -q "https://github.com/projectdiscovery/nuclei/releases/download/v3.3.7/nuclei_3.3.7_linux_${ARCH}.zip" -O /tmp/nuclei.zip && \
    unzip -oq /tmp/nuclei.zip -d /tmp/nuclei-extract && mv /tmp/nuclei-extract/nuclei /usr/local/bin/nuclei && \
    # subfinder
    wget -q "https://github.com/projectdiscovery/subfinder/releases/download/v2.6.7/subfinder_2.6.7_linux_${ARCH}.zip" -O /tmp/subfinder.zip && \
    unzip -oq /tmp/subfinder.zip -d /tmp/subfinder-extract && mv /tmp/subfinder-extract/subfinder /usr/local/bin/subfinder && \
    # dalfox
    wget -q "https://github.com/hahwul/dalfox/releases/download/v2.9.3/dalfox_2.9.3_linux_${ARCH}.tar.gz" -O /tmp/dalfox.tar.gz && \
    tar -xzf /tmp/dalfox.tar.gz -C /usr/local/bin dalfox && \
    # katana (web crawler JS-aware)
    wget -q "https://github.com/projectdiscovery/katana/releases/download/v1.1.0/katana_1.1.0_linux_${ARCH}.zip" -O /tmp/katana.zip && \
    unzip -oq /tmp/katana.zip -d /tmp/katana-extract && mv /tmp/katana-extract/katana /usr/local/bin/katana && \
    # dnsx (DNS resolution at scale)
    wget -q "https://github.com/projectdiscovery/dnsx/releases/download/v1.2.1/dnsx_1.2.1_linux_${ARCH}.zip" -O /tmp/dnsx.zip && \
    unzip -oq /tmp/dnsx.zip -d /tmp/dnsx-extract && mv /tmp/dnsx-extract/dnsx /usr/local/bin/dnsx && \
    # gitleaks (secrets scanning)
    wget -q "https://github.com/gitleaks/gitleaks/releases/download/v8.18.4/gitleaks_8.18.4_linux_${GITLEAKS_ARCH}.tar.gz" -O /tmp/gitleaks.tar.gz && \
    tar -xzf /tmp/gitleaks.tar.gz -C /usr/local/bin gitleaks && \
    # kiterunner (API endpoint discovery)
    wget -q "https://github.com/assetnote/kiterunner/releases/download/v1.0.2/kiterunner_1.0.2_linux_${ARCH}.tar.gz" -O /tmp/kiterunner.tar.gz && \
    tar -xzf /tmp/kiterunner.tar.gz -C /usr/local/bin kr && \
    # cleanup
    rm -rf /tmp/*

# cloud_enum (cloud storage bucket enumeration)
RUN git clone --depth 1 https://github.com/initstring/cloud_enum.git /opt/cloud_enum && \
    pip install --no-cache-dir -r /opt/cloud_enum/requirements.txt && \
    chmod +x /opt/cloud_enum/cloud_enum.py && \
    ln -s /opt/cloud_enum/cloud_enum.py /usr/local/bin/cloud_enum

# linkfinder (JS endpoint extraction)
RUN git clone --depth 1 https://github.com/GerbenJavado/LinkFinder.git /opt/linkfinder && \
    pip install --no-cache-dir -r /opt/linkfinder/requirements.txt && \
    chmod +x /opt/linkfinder/linkfinder.py && \
    ln -s /opt/linkfinder/linkfinder.py /usr/local/bin/linkfinder

# git-dumper (exposed .git directory dumping)
RUN pip install --no-cache-dir git-dumper

# wpscan (WordPress vulnerability scanner)
RUN gem install --no-document wpscan

# retire.js (vulnerable JS library detection)
RUN npm install -g retire

# Go binaries — homepage tools
RUN ARCH=$([ "$TARGETARCH" = "arm64" ] && echo "arm64" || echo "amd64") && \
    # feroxbuster (recursive directory busting)
    wget -q "https://github.com/epi052/feroxbuster/releases/download/v2.10.4/feroxbuster_linux_${ARCH}.tar.gz" -O /tmp/feroxbuster.tar.gz && \
    tar -xzf /tmp/feroxbuster.tar.gz -C /usr/local/bin feroxbuster && \
    # gowitness (web screenshot recon)
    wget -q "https://github.com/sensepost/gowitness/releases/download/3.0.5/gowitness-3.0.5-linux-${ARCH}" -O /usr/local/bin/gowitness && \
    # subzy (subdomain takeover detection)
    wget -q "https://github.com/PentestPad/subzy/releases/download/v1.0.3/subzy_linux_${ARCH}.gz" -O /tmp/subzy.gz && \
    gunzip /tmp/subzy.gz && mv /tmp/subzy /usr/local/bin/subzy && \
    # trufflehog (secrets in web assets)
    wget -q "https://github.com/trufflesecurity/trufflehog/releases/download/v3.63.7/trufflehog_3.63.7_linux_${ARCH}.tar.gz" -O /tmp/trufflehog.tar.gz && \
    tar -xzf /tmp/trufflehog.tar.gz -C /usr/local/bin trufflehog && \
    rm -rf /tmp/*

# foundry (forge + cast + anvil — smart contract dev/test/interact)
RUN curl -L https://foundry.paradigm.xyz | bash && \
    /root/.foundry/bin/foundryup && \
    ln -s /root/.foundry/bin/forge /usr/local/bin/forge && \
    ln -s /root/.foundry/bin/cast /usr/local/bin/cast && \
    ln -s /root/.foundry/bin/anvil /usr/local/bin/anvil

# solc default version (required by slither and mythril)
RUN solc-select install latest && solc-select use latest

# echidna (smart contract property-based fuzzer)
RUN ARCH=$([ "$TARGETARCH" = "arm64" ] && echo "aarch64" || echo "x86_64") && \
    wget -q "https://github.com/crytic/echidna/releases/download/v2.2.3/echidna-2.2.3-Linux.tar.gz" -O /tmp/echidna.tar.gz && \
    tar -xzf /tmp/echidna.tar.gz -C /usr/local/bin && \
    rm /tmp/echidna.tar.gz

# heimdall (EVM bytecode decompiler + analysis)
RUN ARCH=$([ "$TARGETARCH" = "arm64" ] && echo "aarch64" || echo "x86_64") && \
    wget -q "https://github.com/Jon-Becker/heimdall-rs/releases/download/v0.8.4/heimdall-${ARCH}-unknown-linux-gnu" -O /usr/local/bin/heimdall

# Make all binaries executable
RUN chmod +x /usr/local/bin/ffuf /usr/local/bin/nuclei /usr/local/bin/subfinder /usr/local/bin/dalfox \
             /usr/local/bin/katana /usr/local/bin/dnsx /usr/local/bin/gitleaks /usr/local/bin/kr \
             /usr/local/bin/feroxbuster /usr/local/bin/gowitness /usr/local/bin/subzy /usr/local/bin/trufflehog \
             /usr/local/bin/echidna /usr/local/bin/heimdall

# Wordlists — essential files only (no full SecLists clone)
RUN mkdir -p /usr/share/wordlists/seclists/Discovery/Web-Content \
             /usr/share/wordlists/seclists/Discovery/DNS \
             /usr/share/wordlists/seclists/Fuzzing \
             /usr/share/wordlists/seclists/Passwords \
             /usr/share/wordlists/dirb && \
    SECLISTS="https://raw.githubusercontent.com/danielmiessler/SecLists/master" && \
    # Web content discovery
    wget -q "$SECLISTS/Discovery/Web-Content/common.txt" -O /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt && \
    wget -q "$SECLISTS/Discovery/Web-Content/big.txt" -O /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt && \
    wget -q "$SECLISTS/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt" -O /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt && \
    wget -q "$SECLISTS/Discovery/Web-Content/api/api-endpoints.txt" -O /usr/share/wordlists/seclists/Discovery/Web-Content/api-endpoints.txt && \
    wget -q "$SECLISTS/Discovery/Web-Content/raft-medium-words.txt" -O /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-words.txt && \
    # DNS
    wget -q "$SECLISTS/Discovery/DNS/subdomains-top1million-5000.txt" -O /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt && \
    # Fuzzing
    wget -q "$SECLISTS/Fuzzing/special-chars.txt" -O /usr/share/wordlists/seclists/Fuzzing/special-chars.txt && \
    wget -q "$SECLISTS/Fuzzing/LFI/LFI-Jhaddix.txt" -O /usr/share/wordlists/seclists/Fuzzing/LFI-Jhaddix.txt && \
    # Passwords
    wget -q "$SECLISTS/Passwords/Common-Credentials/10k-most-common.txt" -O /usr/share/wordlists/seclists/Passwords/top-10000.txt && \
    # dirb wordlists
    wget -q "https://raw.githubusercontent.com/v0re/dirb/master/wordlists/common.txt" -O /usr/share/wordlists/dirb/common.txt && \
    wget -q "https://raw.githubusercontent.com/v0re/dirb/master/wordlists/big.txt" -O /usr/share/wordlists/dirb/big.txt

# Keep container alive for docker exec
CMD ["sleep", "infinity"]
