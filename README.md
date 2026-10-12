
# L2 Order Book Engine 


## Table of Contents
- [About](#-About)
- [Quickstart](#-Quickstart)
- [Documentation](#-documentation)
- [Plans](#-Plans)

## About 
- This is a L2 Order book implementation that provides a real-time bid-ask tables for specified endpoint. Rather than creating seperate implementations for ssl endpoints we provide templates for bid-ask depth. 
- Connector Clients provde a streamlined pipeline to fetch market data in a stream without blocking or interfacing with market processing logic.
- Connectors and Handlers are all orchestrated under a ConnectionFactory which is responsible for registering specific stream routes to their respective handler. Each connection client and handler class is given a lock-free queue that is write-read from respecitvely. 
- MarketFrame buffers are ``char[4096]`` wide with a total queue capacity of 1024 streams. No need to worry about race conditions between multiple connector contexts 

## Quickstart
clone the repository here: 
```shell 
git clone https://github.com/jahz2323/QuantProject.git
```
To utilise the Binance Connectors, setup a binance account and create a new file 
```shell
cd QuantProject
# for user authentication create a .env or associated
```

### Dependancies 
cmake 4.2.3
Boost
OpenSSL
Threads 
pybind11 


### Building 
* build the whole project 
    ```shell 
    cd QuantProject
    cmake -B build 
    cmake --build build 
    ```
* build tests run 
    ```shell
    cmake --build tests 
    ```
* build individual targets 
    * NetOrchestrate 
        ```shell
        cd NetOrchestrate 
        cmake --build API_Connector  
        ```
    * LobEngine 
        ```SHELL 
        cd LobEngine 
        cmake --build lob_engine