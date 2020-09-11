FROM registry.svc.ci.openshift.org/openshift/release:golang-1.13

ENV GOCILINT_VERSION="1.31.0"
ENV GOCILINT_SHA256SUM="9a5d47b51442d68b718af4c7350f4406cdc087e2236a5b9ae52f37aebede6cb3"

ENV OPERATOR_SDK_VERSION="0.16.0"
ENV OPERATOR_SDK_SHA256SUM="3df782f341749f7962ab0fcfedd2961c18b21ad34ff7acd194b49a152f59abcb"

ENV OPM_VERSION="1.13.8"
ENV OPM_SHASUM="a48aa9d69b0be3439220e818edde4b36b3b9eceb2a058d86b4fdf0ca9dcd21c8"

RUN curl -L -o golangci-lint.tar.gz https://github.com/golangci/golangci-lint/releases/download/v${GOCILINT_VERSION}/golangci-lint-${GOCILINT_VERSION}-linux-amd64.tar.gz && \
    echo ${GOCILINT_SHA256SUM} golangci-lint.tar.gz && \
    tar xzf golangci-lint.tar.gz golangci-lint-${GOCILINT_VERSION}-linux-amd64/golangci-lint && \
    mv golangci-lint-${GOCILINT_VERSION}-linux-amd64/golangci-lint /usr/local/bin && rm -rf golangci-lint-${GOCILINT_VERSION}-linux-amd64 && \
    rm -f golangci-lint.tar.gz && \
    curl -L -o operator-sdk https://github.com/operator-framework/operator-sdk/releases/download/v${OPERATOR_SDK_VERSION}/operator-sdk-v${OPERATOR_SDK_VERSION}-x86_64-linux-gnu && \
    echo ${OPERATOR_SDK_SHA256SUM} operator-sdk | sha256sum -c && \
    mv operator-sdk /usr/local/bin && \
    curl -L -o opm https://github.com/operator-framework/operator-registry/releases/download/v${OPM_VERSION}/linux-amd64-opm && \
    echo ${OPM_SHASUM} opm | sha256sum -c && \
    mv opm /usr/local/bin

RUN python3 -m pip install PyYAML==5.3.1

ENV GIT_VERSION "2.28.0"
ENV GIT_SHASUM "02016d16dbce553699db5c9c04f6d13a3f50727c652061b7eb97a828d045e534"
RUN yum remove -y git* && \
    yum -y install epel-release perl-CPAN gettext-devel perl-devel openssl-devel zlib-devel curl-devel expat-devel getopt asciidoc xmlto docbook2X && \
    yum -y groupinstall "Development Tools" && \
    curl -L -o git.tar.gz https://github.com/git/git/archive/v${GIT_VERSION}.tar.gz && \
    echo ${GIT_SHASUM} git.tar.gz | sha256sum -c && \
    tar xzf git.tar.gz && \
    make --directory git-${GIT_VERSION} configure && ./git-${GIT_VERSION}/configure --prefix=/usr && \
    make --directory git-${GIT_VERSION} && make --directory git-${GIT_VERSION} install && \
    rm -rf git-${GIT_VERSION} && \
    yum groupremove -y "Development Tools" && \
    yum -y remove epel-release perl-CPAN gettext-devel perl-devel openssl-devel zlib-devel curl-devel expat-devel getopt asciidoc xmlto docbook2X && \
    yum clean all && yum -y autoremove && rm -rf /var/cache/yum

