
You can set up Anonify by following steps.

## Environments

- Hardware environment supports Intel SGX like [Azure Confidential Computing](https://azure.microsoft.com/ja-jp/solutions/confidential-compute/)
- Aleady installed [cargo](https://doc.rust-lang.org/cargo/getting-started/installation.html)

## Set up

We fetch docker image and Anonify repository.

```sh
$ docker pull osuketh/anonify
$ git clone git@github.com:LayerXcom/anonify.git
$ cd anonify
```

Anonify don't depend a specific blockchain platform.
To store the state data, we connect Ethereum network as an example, so build smart contract.

```sh
$ solc -o build --bin --abi --optimize --overwrite contracts/Anonify.sol
```

## Run server

Using `docker-compose`, we run servers to connect to Anonify network.

```sh
$ docker-compose -f docker/docker-compose-anonify.yml up -d
```

## Build CLI

We build CLI to use Anonify commands.

```sh
$ ./scripts/build-cli.sh
```
