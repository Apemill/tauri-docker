FROM node:18

RUN apt update
RUN apt install -y libwebkit2gtk-4.0-dev \
    build-essential \
    curl \
    wget \
    libssl-dev \
    libgtk-3-dev \
    libayatana-appindicator3-dev \
    librsvg2-dev

# Install RUST
RUN curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh