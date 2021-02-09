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
| [0001_jordi](https://github.com/hermeznetwork/phase2ceremony_1/tree/main/0001_jordi) | [circuit-1960-32-256-64_hez2_0001.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-1960-32-256-64_hez2_0001.zkey)     | [jordi](https://keybase.io/jbaylina)  | `36573aa6 be970a99`<br>`e68b6cd2 cc563201`<br>`ecc5f506 88c0ff62`<br>`3deae555 2fc56d96`<br>`db155b36 67fa8573`<br>`73c67f37 d5fa7d29`<br>`e280e9ee 88a9d381`<br>`704bef65 68ebeebd` |

### 352 Tx Circuit

| Atestation<br>&nbsp; | Contribution File<br>&nbsp; | Contributor<br>&nbsp; | Contribution Hash &nbsp; &nbsp; &nbsp; &nbsp; |
|:-----|:------------ |:-----|:--------------------------------------|
| 0000_initial | [circuit-352-32-256-64_hez2_0000.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-352-32-256-64_hez2_0000.zkey)     | |
| [0001_jordi](https://github.com/hermeznetwork/phase2ceremony_1/tree/main/0001_jordi) | [circuit-352-32-256-64_hez2_0001.zkey](https://hermez.s3-eu-west-1.amazonaws.com/circuit-352-32-256-64_hez2_0001.zkey)     | [Jordi](https://keybase.io/jbaylina)  | `ba2070a3 dde844a1`<br>`1ddb002f d021f97c`<br>`56c82fa9 3311c79c`<br>`a9db5d66 f230ee82`<br>`97d249fc bee29fbd`<br>`931d1e22 b6e20d50`<br>`5ce80a01 e5f9ad8b`<br>`1c658a46 e454634f`|

### WithdrawCircuit

| Atestation<br>&nbsp; | Contribution File<br>&nbsp; | Contributor<br>&nbsp; | Contribution Hash &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; <br> &nbsp; |
|:-----|:------------ |:-----|:--------------------------------------|
| 0000_initial | [withdraw_hez2_0000.zkey](https://hermez.s3-eu-west-1.amazonaws.com/withdraw_hez2_0000.zkey)     | |
| [0001_jordi](https://github.com/hermeznetwork/phase2ceremony_1/tree/main/0001_jordi) | [withdraw_hez2_0001.zkey](https://hermez.s3-eu-west-1.amazonaws.com/withdraw_hez2_0001.zkey)     | [Jordi](https://keybase.io/jbaylina)  |     `25b83ba8 1db23b31`<br>`ddd54f03 cfa7723c`<br>`2229320c a075b8fc`<br>`8d04b186 acd140db`<br>`43b1d398 266861f6`<br>`2e82da13 6bf8bf06`<br>`77512609 ab2cb254`<br>`e3d3e8b4 448299ad`|

