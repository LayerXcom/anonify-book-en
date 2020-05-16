
This is section is about `mint` and `burn`.

## Initial State

We create 3 accounts by using `wallet init` and `wallet create` commands, and also you must set `wallet password` and `account name`.

```sh
$ ./target/debug/anonify-cli wallet init
$ ./target/debug/anonify-cli wallet add-account
$ ./target/debug/anonify-cli wallet add-account
```

The command `wallet list` tells us the accounts registerd in wallet, following are the accounts of Alice, Bob and Charlie.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli wallet list
Alice: JeMocNNkNqABAEqBEurffNHIr0Y=
Bob: SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
Charlie: On1lVfkMUGm6+lT3OetO8A2HR4M=
```

Run `init_state` to initialize.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify init_state -t 100
```

Each balance are followings.

```sh
# Alice's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 0
Current State: 100

# Bob's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 1
Current State: 0

# Charlie's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 2
Current State: 0
```

## mint

Let's do minting `50` to Bob, `mint` command can be executed by only Alice who have executed `init_state` command.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify mint -a 50 -t SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
Transaction Receipt: "c05dc918cde8caa83e4233bff236285efc7197f554a844e590619d1c24d41b8d"
```

## The State after executing mint

We check the balances of Alice, Bob and Charlie after executing `mint`.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 0
Current State: 100

$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 1
Current State: 50

$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 2
Current State: 0
```

Bob has got `50` coins by `mint`.

## burn

We'll do burning `30` coins from Alice.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify burn -i 0 -a 30
Transaction Receipt: "917576ddb5bdd68f01d2bb98f3ae4dcafec28c385bdb1033ed9b2b51529c4c22"
```

## The State after executing burn

We check the balances of Alice, Bob and Charlie after executing `burn`.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 0
Current State: 70

$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 1
Current State: 50

$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 2
Current State: 0
```

We could see the Alice's balance is `70`, decreased from `100`by `30` coins.

## burn by not owner

`burn` command can be executed by the non-owner account. 
Let's burn `10` coins from Bob.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify burn -i 1 -a 10
Transaction Receipt: "59d139d1ea29ddd008ecb7947b55677d9013b24f5b7de3ea2b70badfcfe3c848"
```

## The State after executing burn by not owner

We check the balances of Alice, Bob and Charlie after executing burn by Bob.

```sh
# Alice's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 0
Current State: 70

# Bob's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 1
Current State: 40

# Charlie's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 2
Current State: 0
```

We could see the Bob's balance is `40`, decreased from `50`by `10` coins. And also any other account's balance have not changed.