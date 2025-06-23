---
title: Common Terms
layout: singlePage
sectionid: terms
---

## <a href="#common-terms" name="common-terms" class="anchor"> Common Terms </a>

This page defines common terms used throughout the Development Container Specification and related documentation.

### <a href="#base-image" name="base-image" class="anchor"> Base Image </a>

A container image that serves as the starting point for creating a development container. Base images typically contain an operating system and may include development tools, runtimes, and libraries that provide a foundation for development.

### <a href="#container-image" name="container-image" class="anchor"> Container Image </a>

A lightweight, standalone, executable package that includes everything needed to run an application: code, runtime, system tools, system libraries, and settings. Container images are built from Dockerfiles or pulled from container registries.

### <a href="#dev-container" name="dev-container" class="anchor"> Dev Container </a>

Short for "development container." A containerized development environment that includes all the tools, libraries, and dependencies needed for developing a particular project or technology stack.

### <a href="#development-container" name="development-container" class="anchor"> Development Container </a>

A full-featured coding environment that runs inside a container. It allows you to use a container as your primary development environment, separating your development tools from your local machine while providing a consistent, reproducible environment.

### <a href="#development-container-specification" name="development-container-specification" class="anchor"> Development Container Specification </a>

An open specification that defines metadata formats and behaviors for creating reproducible, tool-agnostic containerized development environments. Often abbreviated as "dev container spec."

### <a href="#devcontainer-json" name="devcontainer-json" class="anchor"> devcontainer.json </a>

A JSON with Comments (jsonc) metadata file that contains configuration settings required to create and configure a development container. This file tells tools and services how to access or create a dev container with a well-defined tool and runtime stack.

### <a href="#dockerfile" name="dockerfile" class="anchor"> Dockerfile </a>

A text file that contains a series of instructions used to build a container image. Each instruction in a Dockerfile creates a layer in the image, defining how the container should be constructed.

### <a href="#docker-compose" name="docker-compose" class="anchor"> Docker Compose </a>

A tool for defining and running multi-container Docker applications using YAML configuration files. In the context of dev containers, Docker Compose can be used to orchestrate complex development environments with multiple services.

### <a href="#feature" name="feature" class="anchor"> Feature </a>

A self-contained, shareable unit of installation code and development container configuration. Features allow you to quickly add tooling, runtimes, or libraries to your development container. They are applied during the container build process and can be reused across different projects.

### <a href="#lifecycle-scripts" name="lifecycle-scripts" class="anchor"> Lifecycle Scripts </a>

Commands that run at different points in a development container's lifecycle, such as `onCreateCommand`, `postCreateCommand`, `postStartCommand`, and `postAttachCommand`. These scripts allow you to customize the container setup and configuration process.

### <a href="#metadata" name="metadata" class="anchor"> Metadata </a>

Configuration information that describes how to create, configure, and use a development container. This metadata can be stored in `devcontainer.json` files, container image labels, or embedded in other formats while maintaining a common object model.

### <a href="#oci-registry" name="oci-registry" class="anchor"> OCI Registry </a>

An Open Container Initiative (OCI) compliant registry that stores and distributes container images and artifacts. Examples include Docker Hub, GitHub Container Registry, and Azure Container Registry. Dev Container Features and Templates can be published to OCI registries for sharing and distribution.

### <a href="#remote-development" name="remote-development" class="anchor"> Remote Development </a>

The practice of developing software using resources (compute, storage, etc.) that are located remotely from the developer's local machine. Dev containers enable remote development by providing consistent environments that can run locally, in the cloud, or on remote machines.

### <a href="#template" name="template" class="anchor"> Template </a>

A pre-configured development container setup that provides a starting point for new projects. Templates include a `devcontainer.json` file and may include additional configuration files, sample code, and documentation to help developers quickly get started with a particular technology stack.

### <a href="#workspace" name="workspace" class="anchor"> Workspace </a>

The directory or folder that contains your project's source code and is mounted into the development container. The workspace folder is typically where you'll do your development work within the container.

### <a href="#workspace-folder" name="workspace-folder" class="anchor"> Workspace Folder </a>

The default path inside the development container where dev container supporting tools should open when connecting to the container. This is typically where your source code is mounted and where development activities take place.