FROM nvidia/cuda:11.3.0-devel-ubuntu20.04

# Run everything in /root
WORKDIR /root



# Looks like we lost the PGI environmental variable
ENV PGI=/opt/pgi
RUN echo $PGI; 

RUN apt-get -y update;
# 
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ="Europe/Berlin"
RUN apt-get install -y  tzdata;
# 
RUN DEBIAN_FRONTEND="noninteracive" TZ="Europe/Berlin" apt-get -y update && apt-get install -y g++ gcc curl wget libopenblas-dev \
						libarpack2-dev nano wget git libboost-all-dev \
						libhdf5-dev libfftw3-dev 
						
RUN apt-get install -y wget \
  && wget -qO- "https://cmake.org/files/v3.20/cmake-3.20.0-linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C /usr/local
  

# Install SuperLU5

RUN     git clone https://github.com/xiaoyeli/superlu.git; \
	cd superlu; \
	mkdir build; \
	cd build; \
	cmake -DBUILD_SHARED_LIBS=ON ..; \
	make; \
	make install; 

# Install Armadillo

RUN git clone https://gitlab.com/conradsnicta/armadillo-code; \
	ls; \
	cd armadillo-code; \
	git checkout 9.200.x ;\
	mkdir build; \
	cd build; \
	echo $PATH; \
	cmake ../ -DCMAKE_INSTALL_PREFIX=/opt; \
	make; \
	make install; \
	ldconfig; 

# Install ISMRMRD

RUN git clone https://github.com/ismrmrd/ismrmrd.git; \
cd ismrmrd; \
git fetch; \
git checkout tags/v1.4.0; \
mkdir ./build; \
cd build; \
cmake ../ -DCMAKE_CXX_COMPILER=g++ -DCMAKE_C_COMPILER=gcc -DCMAKE_INSTALL_PREFIX=/opt; \
make; \
make install; 

RUN git clone https://github.com/praveenivp/PowerGrid.git; \
    cd PowerGrid; \
    git fetch; \
    git checkout master; \
    mkdir build; \
    cd build; \
    cmake ../ -DCMAKE_CXX_COMPILER=gcc -DCMAKE_INSTALL_PREFIX=/opt/PowerGrid; \
    make -j4; \
    make install 

# For singularity compatibility
RUN ldconfig





