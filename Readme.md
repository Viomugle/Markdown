BUILDING:

In order to install build dependenciew on Debian / Ubuntu do the flowwings:
```
sudo apt install build-essential
sudo apt install cmake 
sudo apt install make
sudo apt install libboost-program-options-dev
``` 
To make LiteTrace
```
cd build
cmake ..
make -j6
```
