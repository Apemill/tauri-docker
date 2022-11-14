# Tauri and C++

This repo contain the docker images for:

  - Windows
    - Choco [Dockerfile](https://github.com/Apemill/tauri-docker/blob/main/windows/choco/Dockerfile)
    - Node [Dockerfile](https://github.com/Apemill/tauri-docker/blob/main/windows/node/Dockerfile)
    - Tauri (Node + Rust + Tauri)
      - MSVC [Dockerfile](https://github.com/Apemill/tauri-docker/blob/main/windows/tauri/Dockerfile)
      - GNU [Dockerfile](https://github.com/Apemill/tauri-docker/blob/main/windows/tauri.gnu/Dockerfile)
    - Tauri-CPP (Tauri + CMake + CPP + Eigen) `WIP`
  - Linux `WIP`
    - Tauri (Node + Rust + Tauri) [Dockerfile](https://github.com/Apemill/tauri-docker/blob/main/linux/tauri/Dockerfile)
    - Tauri-CPP (Tauri + CMake + CPP + Eigen)

> The docker image must be created manually and pushed on dockerhub:

>  - [Apemill/choco](https://hub.docker.com/r/apemill/choco)
>  - [Apemill/node](https://hub.docker.com/r/apemill/node)
>  - [Apemill/tauri](https://hub.docker.com/r/apemill/tauri)
>  - [Apemill/tauri-cpp](https://hub.docker.com/r/apemill/tauri-cpp) `WIP`

> A pipeline could be created, but would consume too much CI/CD ressources (for now)

## Tools installed

    - Powershell
    - Chocolatey
    - Yarn

## Build Windows

```sh
# Chocolatey (1.2.0) Win server 2019
docker build ./windows/choco -t apemill/choco

# Node LTS (18)
docker build ./windows/node -t apemill/node

# Tauri (1.1)
docker build ./windows/tauri -t apemill/tauri
```

## Run with your project

> in those examples, node-modules is not updated, consider updating it before building your project

If you need your local folder in windows, docker does not use the same syntaxe as powershell:
- In powershell, ``$(pwd)`` will give you your local folder
- With docker, you have to use ``${pwd}``

```sh
# Node
docker run -v c:/your/project/folder:c:/work apemill/node
    # OR with local
    docker run -v ${pwd}:c:/work apemill/node

# NPM
docker run -v c:/your/project/folder:c:/work apemill/node npm run build
# YARN
docker run -v c:/your/project/folder:c:/work apemill/node yarn build

# Tauri
docker run --storage-opt size=50G -v c:/your/project/folder:c:/work apemill/tauri
```

### Other examples

```sh
# Run interactivelly
docker run -it -v ${pwd}:c:/work apemill/node powershell

# Run multiple command lines
docker run -v ${pwd}:c:/work apemill/node (npm i) -and (npm run build)

# Increase docker size (necessary for Tauri image) to correct error 1392 on Docker compiler
docker run --storage-opt size=50G -v c:/your/project/folder:c:/work apemill/node
```

### Problem and solution I found while being tortured by Windows

First of all, I strongly advise you to avoid Docker for Windows if possible, it is a mess, the compatibily issues are legion and the error message are unhelpful

---

 - Do not use a version of Windows prior to 1809 (which is the 2019 version)

Before that there was a problem with container on windows causing an error where Node wasn't able to find a folder:
    `ENOENT: no such file or directory, lstat 'C:\ContainerMappedDirectories'` 

 - If you need dotnet (you probably will at some point), find an official Microsoft image with dotnet already installed
> official ASP.NET seems to be the only one working for me

.NET need to be restarted to work, an option which is not available on Docker for Windows

 - Cargo (MSVC) cannot compile in a folder shared with the host without corrupting his data.

The `target` folder for compilation was moved to `C:/target`, after compiling your application, you will have to look there to find your binaries.