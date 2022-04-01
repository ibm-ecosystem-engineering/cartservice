# Copyright 2021 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# https://mcr.microsoft.com/v2/dotnet/sdk/tags/list
# FROM mcr.microsoft.com/dotnet/sdk:6.0.200 as builder
FROM registry.access.redhat.com/ubi8/dotnet-50:5.0-34 as builder
WORKDIR /opt/app-root/src
COPY --chown=default:root cartservice.csproj .
RUN dotnet restore cartservice.csproj -r linux-musl-x64 --disable-parallel
COPY --chown=default:root . .
RUN dotnet publish cartservice.csproj -p:PublishSingleFile=true -r linux-musl-x64 --self-contained true -p:PublishTrimmed=True -p:TrimMode=Link -c release -o cartservice --no-restore
USER root
RUN yum install -y dos2unix
RUN dos2unix cartservice

# https://mcr.microsoft.com/v2/dotnet/runtime-deps/tags/list
FROM registry.access.redhat.com/ubi8/dotnet-50:5.0-34
WORKDIR /opt/app-root/src
COPY --chown=default:root --from=builder /opt/app-root/src/cartservice .
ENV ASPNETCORE_URLS http://*:7070
# ENTRYPOINT ["/opt/app-root/src/cartservice"]
USER root
RUN yum install -y dos2unix
RUN dos2unix cartservice
CMD ["/bin/bash"]
