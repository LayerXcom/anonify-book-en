
This is section is about `approve` and `transfer_from`.

## Initial State

Run `init_state` to initialize.

```sh
ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify init_state -t 100
```

The command `wallet list` tells us the accounts registerd in wallet, following are the accounts of Alice, Bob and Charlie. If you haven't created the accounts yet, see [the procuedure in `mint` and `burn`](https://layerxcom.github.io/anonify-book-en/Tutorials/ERC20/mint_burn/).

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli wallet list
Alice: JeMocNNkNqABAEqBEurffNHIr0Y=
Bob: SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
Charlie: On1lVfkMUGm6+lT3OetO8A2HR4M=
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

## approve

We `approve` Bob `30` coins from Alice.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify approve -a 30 -t SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
Transaction Receipt: "2cdda1f698c359e8eef6d94793c2713049eb8b3a2053fe2d744ad187253cf6ec"
```

Let's check the approved ammount to Bob by `allowance` command.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify allowance -i 0 -t SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
Current State: 30
```

## transfer_from

Using `transfer_from` command, We send `10` coins from Bob to Charlie.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify transfer_from -a 10 -i 1 -f JeMocNNkNqABAEqBEurffNHIr0Y= -t On1lVfkMUGm6+lT3OetO8A2HR4M=
Transaction Receipt: "2a3732f73ed4ae0ca59ec758097f882d49348bbfd61a6cb156739a211fe807b0"
```

## The State after executing transfer_from

We check the final balances of Alice, Bob and Charlie.

```sh
# Alice's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 0
Current State: 90

# Bob's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 1
Current State: 0

# Charlie's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 2
Current State: 10
```

Charlie got `10` coins from Alice.
And also, checking the approved balance of Bob.

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify allowance -i 0 -t SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
Current State: 20
```

Executing `transfer_from`, the approved balance of Bob has decreased to `20` from `30`.