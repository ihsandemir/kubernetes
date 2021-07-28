FROM fedora:latest

ARG VERSION=4.1.1

RUN dnf groups install -y "Development Tools"

RUN dnf install -y cmake gcc-c++
RUN dnf install -y ninja-build

RUN git clone https://github.com/microsoft/vcpkg.git
RUN cd vcpkg && ./bootstrap-vcpkg.sh
RUN cd vcpkg && ./vcpkg install hazelcast-cpp-client

# build client application
COPY . .
RUN cmake -S . -B build -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake -DCMAKE_BUILD_TYPE=Release && cmake --build build --verbose --parallel && cp build/cpp_client .

ENV LD_LIBRARY_PATH /usr/local/lib

CMD ["./cpp_client"]