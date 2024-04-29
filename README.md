# RPS Manager by ThorLabs

This is a Request Per Second (RPS) and Transaction Per Second counter and limiter proxy server application. It forwards HTTP requests to a specified URL while counting the number of requests it receives every second. Additionally, it offers the option to limit requests before forwarding them to the target server, helping users manage their tasks and requests effectively. This tool is ideal for performance testing and monitoring the rate of requests to your servers, providing insights into server load and performance.

## Features
![img.png](img.png)
- Configurable proxy URL and port
- Debug mode for additional logging
- Automatic RPS and TPS calculation with a configurable time window
- Request Rate Limiting
- sendTransaction Rate Limiting

## Getting Started

To run the RPS Manager, you'll need to have [Go](https://golang.org/dl/) installed on your machine. This guide assumes that you have Go setup correctly.

## Installation

-   [Get the latest version of the tool](https://github.com/thorlabsDev/rps-manager/releases)
-   Make sure you choose the right version for your OS

## Configuration
Before starting the server, you need to configure it by modifying the `config.json` file. If the configuration file does not exist, the application will create one with default settings.

Here are the configurable parameters:

* url: The target URL to which the proxy will forward the incoming requests.
* port: The port on which the proxy server will listen.
* debugMode: Enables detailed logging for debugging purposes.
* seconds: The number of seconds over which to calculate the average RPS.
* rateLimitEnabled (boolean): Set to true to enable the rate limiter. Set to false to disable the rate limiter.
* maxRPS (integer): Specifies the maximum number of general requests (excluding sendTransaction) allowed per second.
* maxTPS (integer): Specifies the maximum number of sendTransaction allowed per second.

### Example configuration:

```json
{
  "url": "http://rpc-eu-west-2-zesty.thornode.io/",
  "port": 8080,
  "debugMode": false,
  "seconds": 60,
  "rateLimitEnabled": true,
  "maxRPS": 20,
  "maxTPS": 5
}
```
## Running the Server
To start the proxy server, prepare your config file and run the executable.

Once started, the server will log its activity. You can make HTTP requests to http://localhost:<port> where <port> is the port number specified in your `config.json`. In your Solana bot config, replace RPC URL with the proxy server URL.

## Rate Limiting
Control and limit the incoming request rate to your proxy server with the configurable rate limiting feature.

### Behavior
When rate limiting is enabled, RPS Manager distinguishes between general requests and transaction-specific requests (sendTransaction).
Requests exceeding the configured maximum request or transaction per second (maxRPS/maxTPS) will receive an HTTP 429 (Too Many Requests) error response and will not be forwarded to the target server. This feature helps you manage your tasks more effectively by ensuring that the server does not get overloaded with excessive requests.
It's important to note that while the application can limit requests, there may be cases of overshoot. To ensure optimal performance, it is recommended to keep the RPS/TPS below 10% of the actual capacity of the server.

### Monitoring
If debugMode is enabled in the `config.json`, the server will log every incoming request and the current RPS/TPS calculation. Otherwise, it will print the current and average RPS/TPS at intervals specified by the seconds configuration.

## Managing Performance with the RPS Manager
The RPS manager proxy server application is designed to help you manage and monitor the rate of requests to your servers. It provides insights into server load and performance, allowing you to optimize resource utilization and ensure smooth operation. However the performance impact of the RPS Manager depends on various factors such as the hardware it's running on, the configuration settings, and the workload it's handling. Here are some potential impacts to consider:
* Overhead: The application adds some overhead due to request counting, limiting, and logging. However, this overhead is typically minimal unless the server is already under heavy load.
* Latency: Request processing may introduce some latency (around 0.05ms), especially when the rate limiter is turned on. This latency can vary based on the server's workload and the complexity of the request processing logic.
* Resource Utilization: The application consumes CPU, memory, and network resources. The impact on resource utilization depends on factors like the number of requests, the complexity of the request processing logic, and the efficiency of the implementation. With the addition of TPS limiting, it's important to consider how transaction-specific rate limiting might impact the overall performance, particularly in scenarios where transaction requests are frequent. Proper tuning of the `maxTPS` setting is crucial to ensure that the server remains responsive without overloading.
* Scalability: In some cases, the application may limit the scalability of the server by restricting the number of concurrent requests it can handle efficiently. However, this limitation can be mitigated by properly configuring the application and the server.
Overall, while the application may introduce some performance degradation, the impact is usually minimal and manageable. Proper configuration, monitoring, and optimization can help mitigate any performance issues and ensure smooth operation.

## Troubleshooting
* Performance Degradation: If you notice performance degradation, review your configuration settings and server capacity to identify potential bottlenecks.
* Overshoot: If the RPS/TPS exceeds the recommended limits, consider decreasing the limit to match with target server's limit.

## Disclaimer
The information, recommendations, and guidelines provided in this document are presented in good faith and believed to be accurate based on the current state of knowledge and technology. However, they are provided without any warranty or guarantee of any kind, either expressed or implied, including, without limitation, warranties of merchantability or fitness for a particular purpose. Users are advised to exercise their own judgment and discretion when implementing or using the tools and techniques described herein. The authors, contributors, and publishers of this document shall not be held liable for any damages, losses, or consequences, whether direct, indirect, special, incidental, or consequential, arising out of or in connection with the use or reliance on this documentation or any information contained within.

## Contact Us
For any inquiries, feedback, or to report bugs, please reach out to us via our official Discord server:

[Join ThorLabs Discord Server](https://discord.gg/thorlabs)

To report a bug or any other issue, kindly submit a ticket within the Discord server. Our dedicated team will review and address your concerns promptly.
