# Verification of the ceremony guide

In order to verify the circuit, you will need a machine with tons of memory.

For reference, the one we are using is a machine with 16cores and 512Gb of RAM.

In this tutorial we will give instructions for a x1e.8xlarge aws instance. This instance has 16 cores 32 threads, 976Gb of RAM and it's important to build it with a SSD of 960Gb that we will use as swap space. The instance will use Ubuntu 20.04 and the cost of the instance is 3.336$/h.

So lets start by launching and instece. Be sure to have 1023GB of EBS and local SSD storage.

## Basic OS preparation

````
sudo apt-get install tmux
````

## Adding swap and tweeking the OS to accept high amount of memory.

````
sudo mkswap -f /dev/xvdb
sudo swapon /dev/xvdb

sudo fallocate -l 400G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

sudo sh -c 'echo "vm.max_map_count=10000000" >>/etc/sysctl.conf'
sudo sh -c 'echo 10000000 > /proc/sys/vm/max_map_count'
````

It's important to note that we need 2200G of memory adding RAM and swap together. That's why we use the full local disk plus a 400G file.

## Compile a patched version of node

First install node with nvm:

````
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
source ~/.bashrc
nvm install v14.8.0
node --version
````

Now compile a patched version of node.  This aptch avoids activating garbage collections in memory presure situations and many threads.  This is very convininet for the optimization program.

````
sudo apt-get update
sudo apt-get install python3 g++ make
git clone git clone https://github.com/nodejs/node.git
cd node
git checkout 8beef5eeb82425b13d447b50beafb04ece7f91b1
patch -p1 <<EOL
index 0097683120..d35fd6e68d 100644
--- a/deps/v8/src/api/api.cc
+++ b/deps/v8/src/api/api.cc
@@ -7986,7 +7986,7 @@ void BigInt::ToWordsArray(int* sign_bit, int* word_count,
 void Isolate::ReportExternalAllocationLimitReached() {
   i::Heap* heap = reinterpret_cast<i::Isolate*>(this)->heap();
   if (heap->gc_state() != i::Heap::NOT_IN_GC) return;
-  heap->ReportExternalMemoryPressure();
+  // heap->ReportExternalMemoryPressure();
 }

 HeapProfiler* Isolate::GetHeapProfiler() {
diff --git a/deps/v8/src/objects/backing-store.cc b/deps/v8/src/objects/backing-store.cc
index bd9f39b7d3..c7d7e58ef3 100644
--- a/deps/v8/src/objects/backing-store.cc
+++ b/deps/v8/src/objects/backing-store.cc
@@ -34,7 +34,7 @@ constexpr bool kUseGuardRegions = false;
 // address space limits needs to be smaller.
 constexpr size_t kAddressSpaceLimit = 0x8000000000L;  // 512 GiB
 #elif V8_TARGET_ARCH_64_BIT
-constexpr size_t kAddressSpaceLimit = 0x10100000000L;  // 1 TiB + 4 GiB
+constexpr size_t kAddressSpaceLimit = 0x40100000000L;  // 4 TiB + 4 GiB
 #else
 constexpr size_t kAddressSpaceLimit = 0xC0000000;  // 3 GiB
 #endif
EOL
./configure
make -j16
````

This will compile a patched version of node.

## Download and prepare circom

````
cd ~
git clone https://github.com/iden3/circom.git
cd circom
git checkout v0.5.38
npm install
git log
````

The hash of the commit should be: 467c79511da13438a129e78ee13c775580b00f23


## Prepare the circuits

````
cd ~
git clone https://github.com/hermeznetwork/circuits.git
git checkout phase2ceremony_2
cd circuits
npm install
cd tools
node build-circuit.js create 1960 32 256 64
node build-circuit.js create 352 32 256 64
mkdir withdraw
cd withdraw
cat >withdraw_main.circom <<EOL
include "../../src/withdraw.circom";

component main = Withdraw(32);
EOL
````

The hash of the phase2ceremony_2 tag should be: `ca6226c41d390ac7b7f3ace2f9d2641a379400f1`

## Compile the circuits

The compilation may take a while. You may want to run this step in `tmux` session.

##### 1960Txs Circuit
````
cd ~/circuits/tools/rollup-1960-32-256-64
node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 ../../../circom/cli.js circuit-1960-32-256-64.circom -f -r -v -n "^RollupTx$|^DecodeTx$|^FeeTx$|^Sha256compression$"
````
##### 352Txs Circtuit
````
cd ~/circuits/tools/rollup-352-32-256-64
node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 ../../../circom/cli.js circuit-352-32-256-64.circom -f -r -v -n "^RollupTx$|^DecodeTx$|^FeeTx$|^Sha256compression$"
````

##### Withdraw Circtuit
````
cd ~/circuits/tools/withdraw
node ../../../circom/cli.js -r -v withdraw_main.circom
````

As you can see the withdraw circuit is run withot -f so circom will aready optimize it.

## Reaname and check the resulting files:

##### 1960Txs Circuit
````
cd ~/circuits/tools/rollup-1960-32-256-64
mv circuit-1960-32-256-64.r1cs circuit-1960-32-256-64_no.r1cs
b2sum circuit-1960-32-256-64_no.r1cs
````

The result should be:

````
60a16a8a 749ae244 41d1f46a 6a05f056
6f9462f5 ae0f8322 5bece3ae 9d9f8069
d0096f5c 86a840d4 3163f117 0b52fff5
b3ac7773 0da81a6f 96709e25 54c78a7d
````

##### 352Txs Circtuit
````
cd ~/circuits/tools/rollup-378-32-256-64
mv circuit-352-32-256-64.r1cs circuit-352-32-256-64_no.r1cs
b2sum circuit-352-32-256-64_no.r1cs
````

The result should be:

````
8d7273e5 f8cd5c25 eba75eef 49e66efa
75d31198 2d9009d0 27f625b1 376278cd
199e1a7f e516b12a fd7fddcc 743daf52
f3bf1a23 99b9af35 728cc3c3 e03c9039
````

##### Withdraw Circtuit

````
cd ~/circuits/tools/withdraw
b2sum withdraw_main.r1cs
````

The result should be:

````
3926b2a3 44e15f3b 2334f124 8ce182c5
44e81b45 5952af44 af4e6d75 321dbb2a
8cbe348e dd07593e 12a8c274 2ad62666
8a8afdb5 45d65843 422400b0 b211e8b0
````

## Optimize the circuits

````
cd ~
git clone https://github.com/iden3/r1csoptimize
cd r1csoptimize
git checkout 8bc528b06c0f98818d1b5224e2078397f0bb7faf
npm install
`````

##### 1960Txs circuit
````
~/node/out/Release/node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc src/cli_optimize.js ../circuits/tools/rollup-1960-32-256-64/circuit-1960-32-256-64_no.r1cs ../circuits/tools/rollup-2960-32-256-64/circuit-1960-32-256-64.r1cs
````
And then check the hash of the result.
````
b2sum ../circuits/tools/rollup-1960-32-256-64/circuit-1960-32-256-64.r1cs
````

this should be:

````
0ecee0d5 a1fefc87 26b87537 faf446ba
c53917c5 8976dd69 4f72d5e6 8933d649
1cb64823 d24c5ccb 9da73031 ae5168de
3ad70aba 1b27b865 178c6e5d 7a3f9b78
````



##### 352Txs circuit
````
~/node/out/Release/node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc src/cli_optimize.js ../circuits/tools/rollup-352-32-256-64/circuit-352-32-256-64_no.r1cs ../circuits/tools/rollup-352-32-256-64/circuit-352-32-256-64.r1cs
````
And then check the hash of the result.
````
b2sum ../circuits/tools/rollup-352-32-256-64/circuit-352-32-256-64.r1cs
````

this should be:

````
a8d32c0d 49e92ba4 2484caec 2fb588fb
bcecae7b 98ad9317 f3fd0c21 ef383378
346d9edb 6ac0e033 d554cede fac556be
2ae7970e ea58b31d 8f9a3dae 77e392cb
````

## install snarkjs

````bash
cd ~
git clone https://github.com/iden3/snarkjs.git
cd snarkjs
git checkout v0.3.47
npm install
````

## Downloading and verifying powers of Tau

First you will need to download the full powersOfTau file:

````bash
cd ~
wget https://hermez.s3-eu-west-1.amazonaws.com/powersOfTau28_hez_final.ptau
````

Check the blake2b hash of the circuit:
````bash
b2sum powersOfTau28_hez_final.ptau
````

The result should be:

````
55c77ce8 562366c9 1e7cda39 4cf7b7c1
5a06c12d 8c905e8b 36ba9cf5 e13eb37d
1a429c58 9e8eaba4 c591bc4b 88a0e282
8745a53e 170eac30 0236f5c1 a326f41a
````

If you are also interested in verifying the powers of tau, you can check [this block post](https://blog.hermez.io/zero-knowledge-proofing-hermez-a-quick-guide-to-our-cryptographic-setup/) and [this other](https://blog.hermez.io/zero-knowledge-proofing-hermez-preparephase2-update/)

If you dont trust, the powers of tau, you can verify the powers of tau file with this command:
````bash
~/node/out/Release/node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc ../../../snarkjs/cli.js ptv powersOfTau28_hez_final.ptau -v >verification-pot.txt
````

At the end of the verification-pot.txt you can see the 55 contributions. You now should match the responses showed here with the attestations of the [perpetual powers of tau transcript](https://github.com/weijiekoh/perpetualpowersoftau).

You can see also the random beacon:

````
e586fcca f245c9a1 d7e78294 d4802018
f3001149 a71b8f10 cd997ef8 235aa372
````

This number is the drand round 100,000 which you can find [here](https://drand.cloudflare.com/public/100000)

The procedure was anaunced [here](https://etherscan.io/tx/0x98e38b88ed244a73ce7c1e3ab4f37439ae1faead555a20fd376496339adc2fed): on august 24th, 2020
And the beacon random wos generated on august 26th, 2020

To see that it was calculated that date,
You can see the genesis drand [here](https://drand.cloudflare.com/info)

The unix timestamp of the drand genesis time is: 1595431050
The period is: 30
So the generation time is: 1595431050 + 100000*30 = 1598431050
If we convert this unix time in seconds to readable time, it is Wednesday, 26 August 2020 08:37:30

#### Verify the zkey file.

##### 1960Tx circuit
````
~/node/out/Release/node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc ../../../snarkjs/cli.js zkv circuit-1960-32-256-64.r1cs ../../../powersOfTau28_hez_final.ptau circuit-1960-32-256-64_hez2_final.zkey -v >verification-1960.txt
````

The circuit hash must be:
````
755a1c7b e3556941 1c368f6a f8700769
9015c263 8864499a e3051d5e 5160eea4
af4a8a28 1ab233f0 f7b4dc65 f51ad0aa
0b495c29 54f6b493 a04fdf7a 58ecff17
````

If you want to check an intermediary circuit, Substitute _final by the intermediary response you want.

You can check all the contributions at the end of the `verification-1960.txt` file. You should check all the signatures of the transcript and see that the declared contribution hash of each particimat matches the ones here.

As far as there is a single attestatation you trust, you can considere the ceremony for this circuit safe.

##### 352Tx circuit
````
~/node/out/Release/node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc ../../../snarkjs/cli.js zkv circuit-352-32-256-64.r1cs ../../../powersOfTau28_hez_final.ptau circuit-352-32-256-64_hez2_final.zkey -v >verification-352.txt
````

The hash of the circuit must be:
````
c356c4af cc7b59cc 42228f92 322a7ce6
015dd4f2 e3ab3871 fbdfaf81 fb493954
1f701c6f 8dd93a18 03942a44 3f79aa2f
65387a88 f828f4db 710194bb 18da2437
````

You have to match also the contribution hashes of the participants with attestations they published.

As far as there is a single attestatation you trust, you can considere the ceremony for this circuit safe.

##### Withdraw  circuit
````
~/node/out/Release/node --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc ../../../snarkjs/cli.js zkv withdraw_main.r1cs ../../../powersOfTau28_hez_final.ptau withdraw_final.zkey -v >verification-withdraw.txt
````

The hash of the circuit must be:
````
ec15023b 4b03e104 a8b8f620 e48b6f8a
f6231eeb d3a3e47c c7745379 34b16062
301a0ad3 91d56c7e cf98b8d3 28f231f5
42f67e89 893e2b21 277e7019 55020b8c
````

You have to match also the contribution hashes of the participants with attestations they published.

As far as there is a single attestatation you trust, you can considere the ceremony for this circuit safe.

#### Make a public post.

If you reach this point, it would be great if you can publish a public post or at least a tweet explaining that you verified the circuits.

For any incident you find, please do not hesitate to contact us in the Hermez discord channel or in [ the telegram group](https://t.me/hermez_network)



