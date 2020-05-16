
This is section is about `transfer`.

## Initial State

Run `init_state` to initialize.

```sh
ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify init_state -t 100
```

The command `wallet list` tells us the accounts registerd in wallet, following are the accounts of Alice and Bob. If you haven't created the accounts yet, see [the procuedure in `mint` and `burn`](/Tutorials/ERC20/mint_burn/).

```sh
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli wallet list
Alice: JeMocNNkNqABAEqBEurffNHIr0Y=
Bob: SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
```

Each balance are followings.

```sh
# Alice's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 0
Current State: 100

# Bob's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 1
Current State: 0
```

## transfer

We `transfer` `20` coins from Alice to Bob.

```sh
＄ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify transfer -a 20 -t SyQwvGjGQmA4OG1ZV2zMNn4GXpA=
Transaction Receipt: "0d89b58846a0db41976c6f95c62c5bfb63a5b2d4e29a560d652b913889edfde8"
```

## The State after executing transfer

We check the balances after transfering.

```sh
# Alice's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 0
Current State: 80

# Bob's Balance
$ ANONIFY_URL=http://172.28.1.1:8080 ./target/debug/anonify-cli anonify balance_of -i 1
Current State: 20
```

Finally, we could transfer `20` coins from Alice to Bob.
