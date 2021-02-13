# Hermez Network Second Phase2 Trusted Setup Ceremony.

NOTE: The first First Phase2 ceremony was cancelled as we added the float40
precission to the system and this implies a modification of the circuit.

This repository contains the transcript of the Phase2 Ceremony.

This ceremony runs the trusted setup for 3 circuits:

* **circuit-1960-32-256-64**
* **circuit-352-32-256-64**
* **withdraw**

The first two circuits are the validation circuits for 1960Tx and 352 Txs.  The third circuit is the one used in the rollup exits.

See [Hermez Documentation](https://docs.hermez.io/#/) for fore information of how the rollup works.

The Circom circuits can be found in [circuits](https://github.com/hermeznetwork/circuits).

For this ceremony we used the tag `phase2ceremony_2` with commit value  `ca6226c41d390ac7b7f3ace2f9d2641a379400f1`

For contributing to the ceremony see: [CONTRIBUTE.md](CONTRIBUTE.md)

For verifying the ceremony see: [VERIFY.md](VERIFY.md)

## Contributions

### 1960 Tx Circuit

| Atestation<br>&nbsp; | Contribution File<br>&nbsp; | Contributor<br>&nbsp; | Contribution Hash &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
|:-----|:------------ |:-----|:--------------------------------------|
| 0000_initial | [circuit-1960-32-256-64_hez2_0000.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-1960-32-256-64_hez2_0000.zkey)     | |
| [0001_jordi](https://github.com/hermeznetwork/phase2ceremony_2/tree/main/0001_jordi) | [circuit-1960-32-256-64_hez2_0001.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-1960-32-256-64_hez2_0001.zkey)     | [jordi](https://keybase.io/jbaylina)  | `36573aa6 be970a99`<br>`e68b6cd2 cc563201`<br>`ecc5f506 88c0ff62`<br>`3deae555 2fc56d96`<br>`db155b36 67fa8573`<br>`73c67f37 d5fa7d29`<br>`e280e9ee 88a9d381`<br>`704bef65 68ebeebd` |
| [0002_eduadiez](https://github.com/hermeznetwork/phase2ceremony_2/tree/main/0002_eduadiez) | [circuit-1960-32-256-64_hez2_0002.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-1960-32-256-64_hez2_0002.zkey)     | [eduadiez](https://keybase.io/eduadiez)  | `6bb35d3e 4ac16583`<br>`f8cdc2b0 28ddee40`<br>`d1af2c3b 8460558f`<br>`5e6880cf 46cedd2d`<br>`1a268fd1 dcb29bdc`<br>`d5f929d1 2b8a9102`<br>`cf1466d2 9d612c71`<br>`3df21be4 6ceeb0aa` |


### 352 Tx Circuit

| Atestation<br>&nbsp; | Contribution File<br>&nbsp; | Contributor<br>&nbsp; | Contribution Hash &nbsp; &nbsp; &nbsp; &nbsp; |
|:-----|:------------ |:-----|:--------------------------------------|
| 0000_initial | [circuit-352-32-256-64_hez2_0000.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-352-32-256-64_hez2_0000.zkey)     | |
| [0001_jordi](https://github.com/hermeznetwork/phase2ceremony_2/tree/main/0001_jordi) | [circuit-352-32-256-64_hez2_0001.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-352-32-256-64_hez2_0001.zkey)     | [Jordi](https://keybase.io/jbaylina)  | `ba2070a3 dde844a1`<br>`1ddb002f d021f97c`<br>`56c82fa9 3311c79c`<br>`a9db5d66 f230ee82`<br>`97d249fc bee29fbd`<br>`931d1e22 b6e20d50`<br>`5ce80a01 e5f9ad8b`<br>`1c658a46 e454634f`|
| [0002_eduadiez](https://github.com/hermeznetwork/phase2ceremony_2/tree/main/0002_eduadiez) | [circuit-352-32-256-64_hez2_0002.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-352-32-256-64_hez2_0002.zkey)     | [eduadiez](https://keybase.io/eduadiez)  | `fc8e607d 209bce51`<br>`f7bf8808 0276a54d`<br>`719c2bae 40eb2de7`<br>`c20e76ec 890f1e6d`<br>`ab07c306 f9556e6c`<br>`13e864f2 a7acb548`<br>`bd13031d 2c27a8ed`<br>`ed1d662b dafaf7d7` |


### WithdrawCircuit

| Atestation<br>&nbsp; | Contribution File<br>&nbsp; | Contributor<br>&nbsp; | Contribution Hash &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; <br> &nbsp; |
|:-----|:------------ |:-----|:--------------------------------------|
| 0000_initial | [withdraw_hez2_0000.zkey](https://hermez.s3-eu-west-1.amazonaws.com/withdraw_hez2_0000.zkey)     | |
| [0001_jordi](https://github.com/hermeznetwork/phase2ceremony_2/tree/main/0001_jordi) | [withdraw_hez2_0001.zkey](https://hermez.s3-eu-west-1.amazonaws.com/withdraw_hez2_0001.zkey)     | [Jordi](https://keybase.io/jbaylina)  |     `25b83ba8 1db23b31`<br>`ddd54f03 cfa7723c`<br>`2229320c a075b8fc`<br>`8d04b186 acd140db`<br>`43b1d398 266861f6`<br>`2e82da13 6bf8bf06`<br>`77512609 ab2cb254`<br>`e3d3e8b4 448299ad`|
| [0002_eduadiez](https://github.com/hermeznetwork/phase2ceremony_2/tree/main/0002_eduadiez) | [withdraw_hez2_0002.zkey](https://hermez.s3-eu-west-1.amazonaws.com/withdraw_hez2_0002.zkey)     | [eduadiez](https://keybase.io/eduadiez)  | `e069724d 953b8c5b`<br>`d8316d8d b9d551aa`<br>`d06b1da3 35b72e13`<br>`b43827fe 2bd96937`<br>`5563a40b 0df85054`<br>`0bf8a724 c2b13dd0`<br>`5a7fa6a3 46241621`<br>`3fb11721 879d8824` |

