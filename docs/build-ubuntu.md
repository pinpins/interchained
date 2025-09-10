sudo apt update && sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install -y gcc-13 g++-13 \
  build-essential automake libtool pkg-config git \
  libevent-dev libzmq3-dev libssl-dev \
  libboost-all-dev libdb5.3-dev libdb5.3++-dev \
  libprotobuf-dev protobuf-compiler libqrencode-dev

export CC=gcc-13 CXX=g++-13
export CXXFLAGS="-std=c++14 -O2 -pipe -fstack-protector-strong"
export CFLAGS="-O2 -pipe -fstack-protector-strong"
export LDFLAGS="-Wl,-O1 -Wl,--as-needed"

./autogen.sh

# Configure
./configure \
  --enable-wallet \
  --without-gui \
  --disable-bench \
  --disable-tests \
  --with-zmq \
  --with-miniupnpc=no \
  --with-incompatible-bdb

make -j$(nproc)
