# Infrastructure Entities  

The **Infrastructure Entities** encompass the medium- and low-level modules of the BRT Library. These modules form the foundational elements upon which the high-level models are built and operate. While they are essential to the library's functionality and architecture, understanding them is not required unless you plan to extend the library or modify its architecture.  

### Middle Layer  

The **Middle Layer** includes a collection of signal processing and service modules that provide the necessary building blocks for high-level models. Signal processing modules handle tasks such as convolution and filtering, while service modules manage critical data like impulse responses. Developers looking to contribute new algorithms or models to the library will need to understand this layer, as it serves as the foundation for extending the library’s functionality.  

### Bottom Layer  

The **Bottom Layer** represents the core of the library. It consists of fundamental classes and templates that define the modular architecture and implement mechanisms for interconnecting modules. This layer ensures the flexibility and scalability of the library, making it possible to adapt its architecture to various needs. Developers interested in modifying or extending the library’s architecture will primarily work with this layer.
