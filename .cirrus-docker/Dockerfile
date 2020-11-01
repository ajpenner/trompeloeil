FROM ubuntu:focal AS python-build

ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update          && \
    apt-get dist-upgrade -y && \
    apt-get install -y 'software-properties-common' 'gnupg2' 'python3-pip' 'cmake' 'wget' 'kcov' 'g++-10' 'g++-9' 'g++-8' 'g++-7' 'clang-10' 'clang-9' 'clang-8' 'clang-7' 'clang-6.0' 'libc++-10-dev' 'libc++abi-dev'
RUN apt-key adv --keyserver hkp://pool.sks-keyservers.net:80 --recv-keys 3B4FE6ACC0B21F32
RUN apt-add-repository "deb http://archive.ubuntu.com/ubuntu bionic main universe"
RUN apt-add-repository "deb http://archive.ubuntu.com/ubuntu xenial main universe"
RUN apt-get update
RUN apt-get install -y 'g++-4.8' 'g++-4.9' 'g++-5' 'g++-6' 'clang++-3.5' 'clang++-4.0' 'clang++-5.0'
RUN pip3 install conan
ENV PATH="/usr/local/bin:$PATH" LD_LIBRARY_PATH="/usr/local/lib64:/usr/local/lib:/usr/local/libexec"
RUN echo "#include <iostream>\n\
\n\
template<typename T>\n\
requires (sizeof(T) == sizeof(int))\n\
constexpr void hello(T t)\n\
{\n\
        std::cout << \"Hello, \" << t << '\\\\n';\n\
}\n\
\n\
int main()\n\
{\n\
        hello(42);\n\
}\n\
" >/tmp/hello.cpp
RUN g++-10 -std=c++20 /tmp/hello.cpp -o /tmp/a.out -fuse-ld=gold
RUN clang++-10 -std=c++20 /tmp/hello.cpp -o /tmp/a.out
RUN clang++-10 -std=c++20 /tmp/hello.cpp -o /tmp/a.out -stdlib=libc++ -fuse-ld=gold -lc++abi
RUN cmake --version
WORKDIR /tmp

WORKDIR /
RUN rm -rf /tmp/*
