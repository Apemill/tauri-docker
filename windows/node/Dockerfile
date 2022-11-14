FROM apemill/choco

# Install Node.js LTS 18
RUN choco install nodejs-lts -y

# Install Yarn
RUN choco install yarn -y

# Create work environment
RUN mkdir C:\work
WORKDIR C:/work

# Standard start command
ENTRYPOINT ["powershell.exe"]
SHELL [ "powershell", "-Command" ]
CMD [ "node" ]