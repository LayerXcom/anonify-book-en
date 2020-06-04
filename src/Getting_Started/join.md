
## Deploy smart contract

We'll deploy smart contract by `deploy` command.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify deploy
6d68b0a618ab5a08be3600957f768c15c9b04baa
```

Successfully deploying, we'll get an address. You can set the address to environment variable `CONTRACT_ADDR`.

```sh
$ export CONTRACT_ADDR=6d68b0a618ab5a08be3600957f768c15c9b04baa
```

After setting `CONTRACT_ADDR`, `set_contract_addr` set the contract address to Anonify.

```sh
$ ANONIFY_URL=http://172.28.1.2:8080 ./target/debug/anonify-cli anonify set_contract_addr 
```

To check the state transition on Anonify network, we run `start_polling` command.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify start_polling
$ ANONIFY_URL=http://172.28.1.2:8080 ./target/debug/anonify-cli anonify start_polling
```

## Join Anonify network.

Run `register` command to join Anonify network. 
This command submmits REPORT which is required for Atestted Computing and add handshake to share group key.

```sh
$ ANONIFY_URL=http://172.28.1.2:8080 ./target/debug/anonify-cli anonify register
```

Congratulations! You have already joined Anonify network.
You can implement self-defined functions, refer to folloing section, and also progress to [the tutorials of ERC20 functions](https://layerxcom.github.io/anonify-book-en/Tutorials/ERC20/transfer/).
