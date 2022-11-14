FROM apemill/node

# Restore the default Windows shell for correct batch processing.
WORKDIR C:/installer
SHELL ["cmd", "/S", "/C"]

# Download the Build Tools bootstrapper. see the link for detail https://learn.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-build-tools?view=vs-2022
RUN curl -SL --output vs_buildtools.exe https://aka.ms/vs/17/release/vs_buildtools.exe && \
    start /w vs_buildtools.exe --quiet --wait --norestart --nocache \
        --add Microsoft.VisualStudio.Component.Windows10SDK.20348 \
        --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 \
        || IF "%ERRORLEVEL%"=="3010" EXIT 0 && \
    # Cleanup
    del /q vs_buildtools.exe

# Restore Powershell
SHELL [ "powershell", "-Command" ]


# Install Rust
RUN choco install rustup.install -y


# Setup Tauri - Create dummy project for caching Tauri dependencies
ENV CARGO_TARGET_DIR="C:/target"
WORKDIR C://dummy
RUN New-Item "C:\dummy\dummy.rs" -ItemType File -Value 'fn main() {}'
COPY ./Cargo-dummy.toml ./Cargo.toml
RUN cargo build --release

# Install Tauri
RUN yarn global add @tauri-apps/cli @tauri-apps/api

# Standard start command
WORKDIR C://work
CMD [ "yarn; yarn tauri build" ]