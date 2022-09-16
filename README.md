# LocalInjective

## Important note
Please note that if you are working with the [Injective Core](https://github.com/InjectiveLabs/injective-core/) repo you can also easily run a testnet by using some of the [make commands](https://github.com/InjectiveLabs/injective/blob/main/Makefile), for example: 

```
make localnet-start
make localnet-build
```


## What is LocalInjective?

LocalInjective (a fork of LocalTerra) is a complete Injective testnet and ecosystem containerized with Docker and orchestrated with a simple `docker-compose` file. It simplifies the way smart-contract developers test their contracts in a sandbox before they deploy them on a testnet or mainnet.

LocalInjective comes preconfigured with opinionated, sensible defaults for standard testing environments. If other projects mention testing on LocalInjective, they are referring to the settings defined in this repo.

LocalInjective has the following advantages over a public testnet:

- Easily modifiable world states
- Quick to reset for rapid iterations
- Simple simulations of different scenarios
- Controllable validator behavior

## Prerequisites

- [`Docker`](https://www.docker.com/)
- [`docker-compose`](https://github.com/docker/compose)
- [`Injectived`](https://get.injective.zone)
  * Select option 3 (localinjective-1), the installer will configure everything for you. 
  * The injectived daemon on your local computer is used to communicate with the localinjective daemin running inside the Docker container. 
- Supported known architecture: x86_64
- 16+ GB of RAM is recommended

## Install LocalInjective

1. Run the following commands::

```sh
git clone https://github.com/InjectiveLabs/LocalInjective.git
cd LocalInjective
```

2. Make sure your Docker daemon is running in the background and [`docker-compose`](https://github.com/docker/compose) is installed.

If running on linux, you can install these tools with the following commands:

- docker
```
sudo apt-get remove docker docker-engine docker.io
sudo apt-get update
sudo apt install docker.io -y
```
- docker-compose
```
sudo apt install docker-compose -y
```

## Start, stop, and reset LocalInjective

- Start LocalInjective:

```sh
make start
```

Your environment now contains:

- [injectived](http://github.com/InjectiveLabs/injective-core) RPC node running on `tcp://localhost:26657`
- LCD running on http://localhost:1317


Stop LocalInjective (and retain chain data):

```sh
make stop
```

Stop LocalInjective (and delete chain data):

```sh
make restart
```

## Integrations

### injectived

1. Ensure the same version of `injectived` is present in your local computer and LocalInjective Docker container. You can check the version of localinjective by checking the image in the docker-compose.yml file and the version of your injectived on your local machine with `injectived version`

2. Use `injectived` from your local machine to talk to your LocalInjective `injectived` node:

```sh
injectived status
```

This command automatically works because `injectived` connects to `localhost:26657` by default.

The following command is the explicit form:
```sh
injectived status --node=tcp://localhost:26657
```

3. Run any of the `injectived` commands against your LocalInjective network, as shown in the following example:

```sh
injectived query account osmo1l0jjmvdtj4c3f8cxzzgfhq0zhdzf2x8cgpg056
```

## Configure LocalInjective

The majority of LocalInjective is implemented through a `docker-compose.yml` file, making it easily customizable. You can use LocalInjective as a starting template point for setting up your own local Injective testnet with Docker containers.

Out of the box, LocalInjective comes preconfigured with opinionated settings such as:

- ports defined for RPC (26657) and LCD (1317)
- standard [accounts](#accounts)

### Modifying node configuration

You can modify the node configuration of your validator in the `config/config.toml` and `config/app.toml` files.

#### Pro tip: Speed Up Block Time

To decrease block time, edit the `[consensus]` parameters in the `config/config.toml` file, and specify your own values.

The following example configures all timeouts to `200ms`:

```diff
##### consensus configuration options #####
[consensus]

wal_file = "data/cs.wal/wal"
- timeout_propose = "3s"
- timeout_propose_delta = "500ms"
- timeout_prevote = "1s"
- timeout_prevote_delta = "500ms"
- timeout_precommit_delta = "500ms"
- timeout_commit = "5s"
+ timeout_propose = "200ms"
+ timeout_propose_delta = "200ms"
+ timeout_prevote = "200ms"
+ timeout_prevote_delta = "200ms"
+ timeout_precommit_delta = "200ms"
+ timeout_commit = "200ms"
```

Additionally, you can use the following single line to configure timeouts:

```sh
sed -E -i '/timeout_(propose|prevote|precommit|commit)/s/[0-9]+m?s/200ms/' config/config.toml
```

### Modifying genesis

You can change the `genesis.json` file by altering `config/genesis.json`. To load your changes, restart your LocalInjective.

## Accounts

LocalInjective is pre-configured with one validator and 10 accounts with INJ balances.

| Account   | Address                                                                                                  | Mnemonic                                                                                                                                                                   |
| --------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| validator | `inj10jmp6sgh4cc6zt3e8gw05wavvejgr5pw6m8j75`<br/>`injvaloper1zwxnl8zlqdfhxy7lzlslpfq3h7n7cuk4rlg6pe` | `gesture inject test cycle original hollow east ridge hen combine junk child bacon zero hope comfort vacuum milk pitch cage oppose unhappy lunar seat`                    |
| test1     | `inj16wx7ye3ce060tjvmmpu8lm0ak5xr7gm2e0qwcq`                                                           | `notice oak worry limit wrap speak medal online prefer cluster roof addict wrist behave treat actual wasp year salad speed social layer crew genius`                       |
| test2     | `inj1pe9mc2q72u94sn2gg52ramrt26x5efw6rdmsj7`                                                           | `quality vacuum heart guard buzz spike sight swarm shove special gym robust assume sudden deposit grid alcohol choice devote leader tilt noodle tide penalty`              |
| test3     | `inj1luqjvjyns9e92h06tq6zqtw76k8xtegfvhv6tg`                                                           | `symbol force gallery make bulk round subway violin worry mixture penalty kingdom boring survey tool fringe patrol sausage hard admit remember broken alien absorb`        |
| test4     | `inj1y6gnay0pv49asun56la09jcmhg2kc94900xn4q`                                                           | `bounce success option birth apple portion aunt rural episode solution hockey pencil lend session cause hedgehog slender journey system canvas decorate razor catch empty` |
| test5     | `inj1u27snswkjpenlscgvszcfjmz8uy2y5qavgqln3`                                                           | `second render cat sing soup reward cluster island bench diet lumber grocery repeat balcony perfect diesel stumble piano distance caught occur example ozone loyal`        |
| test6     | `inj1082kgf9vna4v5pqvyevrh7qsn4t90nwf6gwynu`                                                           | `spatial forest elevator battle also spoon fun skirt flight initial nasty transfer glory palm drama gossip remove fan joke shove label dune debate quick`                  |
| test7     | `inj18twjp50t8xvgwqzxr059jdkwhgv2ssucpt9asw`                                                           | `noble width taxi input there patrol clown public spell aunt wish punch moment will misery eight excess arena pen turtle minimum grain vague inmate`                       |
| test8     | `inj1dvjr96xuew05wz66lxjmdjvqghdlec8l8h2q67`                                                           | `cream sport mango believe inhale text fish rely elegant below earth april wall rug ritual blossom cherry detail length blind digital proof identify ride`                 |
| test9     | `inj18awjexq6yv85uplnsltdfj7gdtw3vs2rmgzuw4`                                                           | `index light average senior silent limit usual local involve delay update rack cause inmate wall render magnet common feature laundry exact casual resource hundred`       |
| test10    | `inj1yhlrh2tjnmz78t0hwqm035xj57ulgajrpxv4nx`                                                           | `prefer forget visit mistake mixture feel eyebrow autumn shop pair address airport diesel street pass vague innocent poem method awful require hurry unhappy shoulder`     |

## Common issues

### Docker permissions problems

In case you get permission denied while trying to run `make start`

```
make start

Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/json: dial unix /var/run/docker.sock: connect: permission denied
```

**Check that the docker engine is running:**

```bash
sudo systemctl status docker
```

If not:

```bash
# Configure Docker to start on boot
sudo systemctl enable docker.service

# Start docker service
sudo systemctl start docker.service
```

**Ensure that the current user is in the `docker` group:**

1. Create the docker group and add your user

```bash
# Create the docker group
sudo groupadd docker

# Add your user to the docker group.
sudo usermod -aG docker $USER
```

2. Log out and log back in so that your group membership is re-evaluated.

More details can be found [here](https://docs.docker.com/engine/install/linux-postinstall/).
