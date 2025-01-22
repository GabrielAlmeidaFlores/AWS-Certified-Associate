# Elastic Compute Cloud (EC2)

O Amazon Elastic Compute Cloud (Amazon EC2) oferece uma capacidade de computação escalável sob demanda na Nuvem Amazon Web Services (AWS). O uso do Amazon EC2 reduz os custos de hardware para que você possa desenvolver e implantar aplicações com mais rapidez. É possível usar o Amazon EC2 para executar quantos servidores virtuais forem necessários, configurar a segurança e as redes e gerenciar o armazenamento. Você pode adicionar capacidade (aumentar a escala verticalmente) para lidar com tarefas de computação pesada, como processos mensais ou anuais ou picos no tráfego do site. Quando o uso diminui, você pode reduzir a capacidade (reduzir a escala verticalmente) de novo.

Uma instância do EC2 é um servidor virtual na Nuvem AWS. Quando executa uma instância do EC2, o tipo de instância que você especifica determina o hardware disponível para sua instância.

## EC2 Instance Types

O Amazon EC2 fornece uma ampla seleção de tipos de instância otimizadas para de adequarem a diferentes casos de uso. Os tipos de instância incluem combinações variadas de capacidade de CPU, memória, armazenamento e redes e oferecem a flexibilidade de escolher a combinação de recursos adequada para suas aplicações. Cada tipo de instância inclui um ou mais tamanhos de instância, permitindo que você escale seus recursos de acordo com os requisitos de sua workload de destino.

Here is a table summarizing the different EC2 instance families, their associated storage types, and ideal use cases.

| **Instance Family**       | **Instance Type**      | **Use Case**                                                                      |
| ------------------------- | ---------------------- | --------------------------------------------------------------------------------- |
| **General Purpose**       | t4g, t3, t3a, t2       | Balanced compute, memory, and networking. Ideal for small to medium apps.         |
|                           | m6g, m5, m5a, m5n, m4  | For workloads that require a balance of compute, memory, and networking.          |
| **Compute Optimized**     | c7g, c6g, c6i, c5n, c5 | High-performance compute for applications like batch processing and gaming.       |
| **Memory Optimized**      | r6g, r5, r5a, r5n, r4  | Memory-intensive workloads like high-performance databases and analytics.         |
|                           | x2idn, x2iezn          | Extreme memory requirements, such as in-memory databases and big data.            |
| **Accelerated Computing** | inf1, f1, dl1          | Specialized hardware acceleration for ML inference and custom computing tasks.    |
| **Storage Optimized**     | i4i, i3, i3en          | High I/O operations for workloads like NoSQL databases and data warehousing.      |
|                           | d2, h1                 | For large-scale data processing and data lakes requiring high storage throughput. |
