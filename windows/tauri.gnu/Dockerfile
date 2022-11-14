FROM apemill/node

# Restore the default Windows shell for correct batch processing.
WORKDIR C:/installer
SHELL ["cmd", "/S", "/C"]

# Download the Build Tools bootstrapper.
RUN curl -SL --output vs_buildtools.exe https://aka.ms/vs/17/release/vs_buildtools.exe && \
    start /w vs_buildtools.exe --quiet --wait --norestart --nocache \
    --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 \
        || IF "%ERRORLEVEL%"=="3010" EXIT 0 && \
    # Cleanup
    del /q vs_buildtools.exe

# Restore Powershell
SHELL [ "powershell", "-Command" ]
WORKDIR C:/work


# Install Rust
RUN choco install rustup.install mingw -y
RUN rustup default stable-x86_64-pc-windows-gnu; rustup toolchain remove stable-x86_64-pc-windows-msvc

# Standard start command
CMD [ "yarn tauri build" ]