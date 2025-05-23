# Compatibility

### Compatibility with Heterogenous Smart Devices

The ioConnect SDK aims to support a wide range of embedded systems, not limited to specific device types. To this end, the SDK is architected with a core framework (CF) and a platform adaption layer (PAL). As the foundation of the SDK, the CF abstracts common functionalities that are independent of the development platform or environment. On the other hand, the PAL adapts to different embedded systems and communities, differentiating the specific functionalities of each platform. For example, Arduino tends to use C++ classes, while the ESP32 community prefers traditional API implementation styles. Moreover, both communities have significant differences in MQTT implementation and usage. However, the SDK can accommodate the implementation methods of each community, including the community’s compilation rules, coding conventions, framework designs, etc.Each PAL component provides the following advantages:

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Layered Architecture in ioID-SDK</p></figcaption></figure>

* With support and adaptation from embedded communities, the SDK can support a wider range of IoT hardware. For example, the SDK currently supports the Arduino community. As a result, any hardware platform supported by Arduino can be supported by the SDK.
* Reduces the development complexity and cycle of the entire SDK. After extensive testing and verification, the platform adaptation layer code for each community is limited to around 300 to 500 lines.
* Developers can quickly and conveniently add new hardware platforms. When developers encounter unsupported embedded hardware while using the SDK, they can develop a new PAL component based on the SDK's PAL specification, requiring only about 300 to 500 lines of code.

It is important to note that the PAL component not only needs to adapt to the community’s code framework system but also needs to support the community’s default or specified file structure, compilation standards, and more. For example, the Arduino and ESP32 projects have completely different programming languages, reference paths, and library managers.Based on the above information, the core framework and PAL layer design can effectively solve the device compatibility issues in IoT projects and provide developers with a user-friendly development and usage environment.

### Minimal Code Coupling

Another unique feature of this SDK is minimal code coupling, which is based on the assumption that users have already developed or are developing a traditional Web2 IoT project. When development teams want to transform their Web2 project into a Web3 one, they often face a number of challenges:

* Code reuse: Maximizing the reuse of developed functional components or the entire project;
* Minimal code coupling: Minimizing modifications to the original code due to the introduction of new component libraries.

The SDK achieves minimal code coupling by compressing and streamlining the technology stack, reducing complexity to a minimum. Additionally, the SDK introduces the concept of a standard layer to reduce code coupling between the SDK and the original project.Traditional component usage employs the API pattern as shown at the top of the firgure above. In this pattern, each component designs API functions independently, which are then referenced and called by both sides. However, this approach inevitably leads to some potential issues, i.e., both parties must have a clear understanding of each other’s API functions, including their definitions and usage methods, before usage.The SDK breaks away from the traditional API call mechanism by introducing a standard layer (as shown at the bottom of the figure). The standard layer uses well-known design frameworks, such as embedded operating systems (e.g., FreeRTOS, Zephyr, etc.), POSIX standards, and active community code frameworks (e.g., Arduino, Raspberry Pi, ESP32). Both parties only need to use the common framework provided by the standard layer, which allows for seamless integration with minimal knowledge of the technical details of the other party’s standard layer.

<figure><img src="https://iotex.larksuite.com/space/api/box/stream/download/asynccode/?code=M2UwMzQwODA2N2U5ODM3YWNhNWMwNTE0NGMzOGMxYzVfY3ZhMHZldTMybE9UcVBxa0tZTThnQkcxNlh3U2lva1RfVG9rZW46U1RtemJFYjZub1dRTTd4ODYyVXV4VUF1czJjXzE3MTY1NTg5MTY6MTcxNjU2MjUxNl9WNA" alt="" width="375"><figcaption></figcaption></figure>
