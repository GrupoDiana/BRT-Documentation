# Welcome to BRT Library

The Binaural Rendering Toolbox (BRT) is a set of software libraries, applications, and definitions aimed as a virtual laboratory for psychoacoustic experimentation. The BRT is developed in the framework of the [SONICOM project](https://www.sonicom.eu/) and include the algorithms developed in the [3D Tune-In Toolkit](https://github.com/3DTune-In/3dti\_AudioToolkit) in a new open, extensible architecture. 

## Quick Start Guide

You can use BRT in two different ways depending on the level of integration which you need with your applications:

* If tou are a C++ programmer building audio appliocations, you can use the [BRT Library](library/index.md), as a C++ library integrated in your own audio application, which handles audio inputs and outpus.

* If you are using any other platform (Max MSP, PureData, Matlab, Unity, Python, etc.) and want to use BRT, you can use [BRT Applications](applications/index.md) controlled via OSC commands. BRT Renderer is a standalone application which manages audio interfaces and is able to read audio files. Is is controllable via OSC commands and is able to perform all functionalities available in the BRT Library.


## Components

* BRT Library.
* BRT Applications.
    * Renderer
    * GUI
* OSC commands to control BRT Renderer.
* SOFA files format used in BRT.

## Credits

This software is being developed by a team coordinated by 

* [Arcadio Reyes-Lecuona](https://github.com/areyesl) ([Diana Research Group, University of Malaga](https://www.diana.uma.es/?page_id=53)). Contact: areyes@uma.es

* [Lorenzo Picinali](https://github.com/lpicinali) ([Audio Experience Design Team, Imperial College London](https://www.axdesign.co.uk)). Contact: l.picinali@imperial.ac.uk  

The current members of the development team are (in alphabetical order):

* [Daniel Gonzalez-Toledo](https://github.com/dgonzalezt) ([University of Malaga](https://www.uma.es/))

* [Maria Cuevas-Rodriguez](https://github.com/mariacuevas) ([University of Malaga](https://www.uma.es/))

* [Luis Molina-Tanco](https://github.com/lmtanco) ([University of Malaga](https://www.uma.es/))

* [Francisco Morales-Benitez](https://github.com//FranMoraUma) ([University of Malaga](https://www.uma.es/))


## Copyright and License

This software is distributed under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

Copyright (c) for each module belongs to its respective authors, who may vary as there are contributions from different institutions in this software. This copyright information is specified in the headers of the corresponding files

This library includes pieces of code from the 3DTI AudioToolkit, shared under GPLv3 license and copyright (c) by University of Málaga (contact: areyes@uma.es) and Imperial College London (contact: l.picinali@imperial.ac.uk). See headers in the source code files.


## Acknowledgements 

![European Union](assets/EU_flag.png "European Union") This project has received funding from the European Union’s Horizon 2020 research and innovation programme under grant agreement no.101017743 


