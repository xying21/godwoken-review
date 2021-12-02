---
id: deposit_withdrawal
title:  Deposit & Withdraw CKB on Godwoken
---
import useBaseUrl from "@docusaurus/useBaseUrl";

For deposit and withdrawal operations on Godwoken, two options are available:

1. Using an UI deposit or withdraw provided by [yokaiswap](https://testnet.yokaiswap.com/bridge/deposit) to perform the relevant actions.
2. Using `gw-tools` deposit or withdraw to perform the relevant actions.

And the unlocking operation so as to complete the withdrawal process.

---

### Using GW-tools to Deposit

Use `--help` to view the available commands.

```shell
cargo run --bin gw-tools -- deposit-ckb --help
```

<details>
<summary>Click to view detailed output</summary>

```
gw-tools-deposit-ckb
Deposit CKB to godwoken

USAGE:
	 gw-tools deposit-ckb [OPTIONS] --capacity <capacity> --config-path <config-path>  --privkey-path <privkey-path>  --scripts-deployment-path <scripts-deployment-path>

FLAGS:
	 -h, --help				Prints help informaiton
	 -V, --version				Prints version information

OPTIONS:
	 -c, --capacity <capacity>		CKB capacity to deposit
	     --ckb-rpc <ckb-rpc-url>		CKB jsonrpc rpc sever URL [default: http://127.0.0.1:8114]

	 -o, --config-path <config-path>	The config.homl file path
	 -e, --eth-address <eth-address>	Target eth address, calculated by private key in default
	 -f, --fee <fee>			Transaction fee, default to 0.0001 CKB [default: 0.0001]
	 -g, --godwoken-rpc-url <godwoken-rpc-url>
	 					Godwoken jsonrpc rpc sever URL [default: http://127.0.0.1:8119]

	 -k, --privkey-path <privkey-path>	The private key file path
	     --scripts-deployment-path <scripts-deployment-path>	
	 					The scripts deployment results json file path

```
</details>

For more information on the CKB RPC, refer to [CKB Wiki](https://github.com/nervosnetwork/ckb/wiki/Chains)

### <code>gw-tool deposit</code> Subcommands

|command|description|
|---|---|
|capacity          |The amount of ckb to deposit, the unit is ckb|
|ckb-rpc           |ckb node URL, defaults to http://127.0.0.1:8114/|
|config-path          |The config.toml file required for godwoken to run|
|eth-address          |Target eth address to deposit|
|fee          |The transaction fee, this is a ckb transaction and the default rate is 0.0001 ckb|
|godwoken-rpc-url          |The RPC address of Godwoken, by default http://127.0.0.1:8119/|
|privkey-path          |A file written with the private key (hex string) which is used to pay the deposit fee|
|scripts-deployment-path          |json file path of the [script's deployment results](https://github.com/nervosnetwork/godwoken-public/blob/master/testnet/config/scripts-deploy-result.json)|

---

### Using GW-tools to Withdraw

Use `--help` to view the available commands.

```shell
cargo run --bin gw-tools -- withdraw --help
```

<details>
<summary>Click to view detailed output</summary>


```
gw-tools-withdraw
withdraw CKB / sUDT from godwoken

USAGE:
	 gw-tools withdraw [OPTIONS] --capacity <capacity> --config-path <config-path> --owner-ckb-address <owner-ckb-address> --privkey-path <privkey-path>  --scripts-deployment-path <scripts-deployment-path>

FLAGS:
	 -h, --help				Prints help informaiton
	 -V, --version				Prints version information

OPTIONS:
	 -m, --amount <amount>		 sUDT amount to withdrawal [default: 0]
	 -c, --capacity <capacity>		CKB capacity to withdrawal
	 -o, --config-path <config-path>	The config.homl file path
	 -g, --godwoken-rpc-url <godwoken-rpc-url>
	 					Godwoken jsonrpc rpc sever URL [default: http://127.0.0.1:8119]

	 -a, --owner-ckb-address <owner-ckb-address>	owner ckb address (to)
	 -k, --privkey-path <privkey-path>	The private key file path
	     --scripts-deployment-path <scripts-deployment-path>	
	 					The scripts deployment results json file path

	 	 -sudt-script-hash <sudt-script-hash>	l1 sudt script hash, default for withdrawal CKB [default: 0x0000000000000000000000000000000000000000000000000000000000000000]

```
</details>

For more information on Godwoken RPC, refer to [Godwoken Public Network](/#godwoken-public-networks).

### <code>gw-tool withdraw</code> Subcommands

|command|description|
|---|---|
|amount             |The amount of sUDT|
|capacity             |The amount of ckb to withdraw, the unit is ckb|
|config-path             |The config.toml file required for godwoken to run|
|godwoken-rpc-url             |The RPC address of Godwoken, by default http://127.0.0.1:8119/|
|owner-ckb-address             |ckb address of the recipient|
|privkey-path             |A file written with the private key (hex string) which is used to pay the deposit fee|
|scripts-deployment-path             |json file path of the [script's deployment results](https://github.com/nervosnetwork/godwoken-public/blob/master/testnet/config/scripts-deploy-result.json)|
|sudt-script-hash             |The script hash of sudt on Layer 1, defaults to 0x0000000000000000000000000000000000000000000000000000, indicating only ckb is redeemed (amount left unfilled or filled with 0)|

---

### Unlocking the funds to complete withdrawal process

Withdrawing funds from Godwoken is a two-step process. Step one initiates the withdrawal and step two releases the funds. A five-day dispute period will begin once the withdrawal process has commenced. To complete the withdrawal process, the `unlock` command must be executed. To unlock the withdrawal cells to normal ckb cells and to perform common Layer2 actions, the [`account-cli tool`](https://github.com/nervosnetwork/godwoken-examples/tree/develop/packages/tools) will be needed. 


Use `--help` to view the available commands.

```shell
# devnet
export LUMOS_CONFIG_FILE=./kicker-lumos-config.json node ./packages/tools/lib/account-cli.js unlock --help

# testnet
export LUMOS_CONFIG_NAME="AGGRON4" node ./packages/tools/lib/account-cli.js unlock --help

# mainnet
export LUMOS_CONFIG_NAME="LINA" node ./packages/tools/lib/account-cli.js unlock --help
```

<details>
<summary>Click to view detailed output</summary>


```
LUMOS_CONFIG_FILE=./kicker-lumos-config.json node ./packages/tools/lib/account-cli.js unlock --help
Usage: account-cli unlock [options]

unlock withdrawal CKB / sUDT from godwoken

Options:
  -p, --private-key <privatekey>			private key to use
  -s, --sudt-script-args <l1 sudt script args>		only for unlock sudt
  -r, --rpc <rpc>			ckb rpc path (default: "http://127.0.0.1:8114")
  -d, --indexer-path <path>				indexer path (default: "./indexer-data-path/<ckb genesis hash>")
  -h, --help			display help for command
```
 </details>

 ### <code>account-cli unlock</code> Subcommands

|command|description|
|---|---|
|LUMOS_CONFIG_FILE		|The environment variable, filled with the lumos config json file path, or simply the name if it is on the testnet / mainnet|
|private-key		|The private key|
|sudt-script-args		|Layer1 sudt script args, or sudt token|
|rpc 			|The rpc address of CKB, with a default value|
|indexer-path 		|The path of Lumos indexer, with a default value, no need to fill in generally|