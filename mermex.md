# Mermex

[Mermaid](https://mermaid.js.org/) is a diagram and flowchart markup language widely used for real-time diagram generation in web interfaces. It also serves as a powerful tool for system architects, enabling them to automate the diagram generation process.

Visualization plays a crucial role in product development, as workflows and operational sequences need to be commonly understood by large and diverse engineering teams. Given the iterative nature of these diagrams during the development cycle, Mermaid's ability to render charts and diagrams directly from text into any platform presents a some critical advantages over other diagramming tools.  It allows users to easily track versions without resorting to screenshots, saving off copies of files, or trying to roll back to snapshots of the graphing tool being used. Users can leverage that versioning to watermark any element of the diagram and reproduce it in any rendering engine that supports Mermaid.

Although very powerful with relatively simple syntax, Mermaid is not intended for direct authoring.  [Mermex](https://github.com/pgaljan/mermex) is an Excel widget designed for authoring Mermaid flowcharts and sequence diagrams. It enhances diagram iteration by incorporating functionalities like titling and watermarking, facilitating convenient export and streamlined version tracking.  Additionally, Excel can take input from json or csv, allowing the consistent, error-free generation and iteration of very large Mermaid flowcharts.

### Key features:
1. integrate with virtually any data source
1. consistent syntax enforcement for fast authoring and re-authoring
2. loose hashing to quickly identify changed diagrams without referencing the output binary
3. watermarked diagrams to trace iterations over time

![mermex_demo](https://github.com/pgaljan/blog/assets/11296072/4d8172eb-8e95-4670-88c6-76392bb7e27c)

##### Note - mermex currently requires Excel O365, as it leverages the VSTACK function