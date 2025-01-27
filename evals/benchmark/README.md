# OPEA Benchmark Tool

This Tool provides a microservices benchmarking framework that uses YAML configurations to define test cases for different services. It executes these tests using `stresscli`, built on top of [locust](https://github.com/locustio/locust), a performance/load testing tool for HTTP and other protocols and logs the results for performance analysis and data visualization.

## Features

- **Services load testing**: Simulates high concurrency levels to test services like LLM, reranking, ASR, E2E and more.
- **YAML-based configuration**: Define test cases, service endpoints, and testing parameters in YAML.
- **Service metrics collection**: Optionally collect service metrics for detailed performance analysis.
- **Flexible testing**: Supports various test cases like chatqna, codegen, codetrans, faqgen, audioqna, and visualqna.
- **Data analysis and visualization**: After tests are executed, results can be analyzed and visualized to gain insights into the performance and behavior of each service. Performance trends, bottlenecks, and other key metrics are highlighted for decision-making.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
  - [Test Suite Configuration](#test-suite-configuration)
  - [Test Cases](#test-cases)


## Installation

### Prerequisites

- Python 3.x
- Install the required Python packages:

```bash
pip install -r ../../requirements.txt
```

## Usage

1 Define the test cases and configurations in the benchmark.yaml file.

2 Run the benchmark script:

```bash
python benchmark.py
```

The results will be stored in the directory specified by `test_output_dir` in the configuration.


## Configuration

The benchmark.yaml file defines the test suite and individual test cases. Below are the primary sections:

### Test Suite Configuration

```yaml
test_suite_config: 
  examples: ["chatqna"]  # Test cases to be run (e.g., chatqna, codegen)
  concurrent_level: 4  # The concurrency level
  user_queries: [1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048]  # Number of test requests
  random_prompt: false  # Use random prompts if true, fixed prompts if false
  run_time: 60m  # Total runtime for the test suite
  collect_service_metric: false  # Enable service metrics collection
  data_visualization: false # Enable data visualization
  test_output_dir: "/home/sdp/benchmark_output"  # Directory for test outputs
```

### Test Cases

Each test case includes multiple services, each of which can be toggled on/off using the `run_test` flag. You can also change specific parameters for each service for performance tuning.

Example test case configuration for `chatqna`:

```yaml
test_cases:
  chatqna:
    embedding:
      run_test: false
      service_name: "embedding-svc"
    retriever:
      run_test: false
      service_name: "retriever-svc"
      parameters:
        search_type: "similarity"
        k: 4
        fetch_k: 20
        lambda_mult: 0.5
        score_threshold: 0.2
    llm:
      run_test: false
      service_name: "llm-svc"
      parameters:
        model_name: "Intel/neural-chat-7b-v3-3"
        max_new_tokens: 128
        temperature: 0.01
        streaming: true
    e2e:
      run_test: true
      service_name: "chatqna-backend-server-svc"
```
