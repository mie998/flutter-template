# ref: https://zenn.dev/nagakuta/articles/bea80b17f315bd069a90

FROM ubuntu:20.04
WORKDIR /workspace

# Install Dart
ARG DART_VERSION=2.16.0
RUN mkdir -p /usr/share/man/man1 && \
    apt-get update && apt-get install -y --no-install-recommends tzdata && \
    apt-get update && \
    apt-get install -y --no-install-recommends bash ca-certifcates clang cmake curl file git libglu1-mesa libgtk-3-dev ninja-build pkg-config unzip xz-utils zip && \
    curl https://storage.googleapis.com/dart-archive/channels/stable/release/$DART_VERSION/sdk/dartsdk-linux-x64-release.zip -o /tmp/dart-sdk.zip && \
    unzip /tmp/dart-sdk.zip -d /usr/lib && rm /tmp/dart-sdk.zip

# Install Flutter
ARG PATH=/usr/lib/dart-sdk/bin:$PATH
ARG PATH=/root/.pub-cache/bin:$PATH
ARG FLUTTER_VERSION=2.10.4
RUN dart pub global activate fvm --verbose && \
    fvm doctor --verbose && \
    fvm install $FLUTTER_VERSION --verbose && \
    fvm use --force $FLUTTER_VERSION --verbose && \
    fvm flutter config --enable-web --enable-linux-desktop --enable-macos-desktop --enable-windows-desktop --enable-android --enable-ios --enable-fuchsia && \
    # fvm flutter precache --verbose && \
    fvm flutter doctor --verbose

# Clean up
RUN apt-get clean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# ENV
ENV FVM_ROOT=/root/.pub-cache
ENV PATH $FVM_ROOT/bin:$PATH

# copy ssh key for git
RUN bash -c "mkdir -p /root/.ssh && ln -s /run/secrets/id_rsa /root/.ssh/id_rsa"
RUN bash -c "chown -R root:root /root/.ssh"
RUN bash -c "chmod -R 0600 /root/.ssh"
RUN bash -c "git config --global core.sshCommand \"ssh -i /root/.ssh/id_rsa\" /dev/null"
