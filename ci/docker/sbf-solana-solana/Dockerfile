FROM ubuntu:20.04
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    gcc libc6-dev ca-certificates

ENV RUSTUP_INIT_SKIP_PATH_CHECK="yes"
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y -v --no-modify-path
RUN cp ${HOME}/.cargo/bin/* /usr/local/bin/

RUN cargo install --git https://github.com/Cartallums/cargo-run-sbf-tests.git \
    --rev eb8f22df8954ce4278a12c30bd1ba396a7aad7a3 \
    --bin cargo-run-sbf-tests --root /usr/local

RUN mkdir -p /tmp/.cache/cbe/v1.32/sbf-tools
RUN curl -L -o cbe-sbf-tools-linux.tar.bz2 https://github.com/Cartallum/sbf-tools/releases/download/v1.32/cbe-sbf-tools-linux.tar.bz2
RUN tar -xjf cbe-sbf-tools-linux.tar.bz2 --strip-components 1 -C /tmp/.cache/cbe/v1.32/sbf-tools
RUN rustup toolchain link sbf /tmp/.cache/cbe/v1.32/sbf-tools/rust
RUN cp -R ${HOME}/.rustup /tmp/

ENV CARGO_TARGET_SBF_CARTALLUM_CBE_RUNNER="cargo-run-sbf-tests --heap-size 104857600"
ENV CC="/tmp/.cache/cbe/v1.32/sbf-tools/llvm/bin/clang"
ENV RUSTUP_TOOLCHAIN="sbf"
