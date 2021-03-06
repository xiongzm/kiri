# kiri
Private IOTA testnet in Kura.

## Resources on which it is based

 * https://github.com/schierlm/private-iota-testnet: Running your own IOTA testnet not connected to the public testnet or mainnet.
     * Used as submodule.
 * https://github.com/jserv/rocksdb: A Persistent Key-Value Store for Flash and RAM Storage.
     * Rocksdb armv7 5.7.3 has been compiled using this code.
     * BUILD.sh has been adapted to compile it in Raspberry PI 2 B -> https://github.com/javiEstrella/rocksdb/pull/1
 * https://github.com/iotaledger/iri: IOTA Reference Implementation.
     * org.eclipse.kura.iri is compiled using a patched version of this code.
     * Maven source plugin has been added to its configuration -> https://github.com/javiEstrella/iri/pull/1

## Plugins

 * org.eclipse.kura.kiri: Service to manage IRI lifecycle.
 * org.eclipse.kura.kiri.iri: Patched version of IRI to deploy private IOTA testnet.
 * org.eclipse.kura.kiri.rocksdb.jni: Rocksdb JNI 5.7.3.
 * org.eclipse.kura.kiri.rocksdb.armv7a: Rocksdb armv7a 5.8.8 (https://github.com/javiEstrella/rocksdb).

## Features

 * org.eclipse.kura.kiri.feature.armv7a: Kiri for armv7a (tested on Raspberry PI 2 B & 3 B).
 * org.eclipse.kura.kiri.feature.jni: Kiri for linux32, linux64, Max OSX and Windows x64 (tested on Up Squared).

## Scripts

 * build-submodules.sh: Init and update git submodules.
     * tools -> https://github.com/javiEstrella/private-iota-testnet.
 * build-snapshot.sh: Create IOTA snapshot.
 * build-dp.sh: Create the deployable packages.
 * coordinator.sh: Create IOTA milestone.

## Demo
 
 * org.eclipse.kura.kiri.feature.armv7a_1.0.0-SNAPSHOT.dp: Deployable package for armv7a version.
 * org.eclipse.kura.kiri.feature.jni_1.0.0-SNAPSHOT.dp: Deployable package for jni version.
 * Snapshot.sig: IOTA snapshot used in the deployable packages.
 * Snapshot.log: Log of snapshot creation (the seed is in this file).

## Step by step

Kura repository must be in folder <parent>/kura -> https://trusted827.wordpress.com/2018/02/02/building-deployment-packages-for-kura/

     local$ mkdir parent; cd parent
     local$ git clone https://github.com/javiEstrella/kura
     local$ cd kura; bash build-all.sh
     local$ cd ..

Kiri repository must be in folder <parent>/kiri:

     local$ git clone https://github.com/javiEstrella/kiri
     local$ cd kiri; git checkout -t origin/develop

Run build-dp.sh to create the deployable packages:

     local$ ./build-dp.sh

Install the DP in your Kura's installation (remember that it is necessary to open the ports in the firewall). Note that the latest version of Kura's web does not show the service's configuration menu the first time it is deployed. The solution is simple, install, uninstall and reinstall... Once you have configured IRI, enable it on the Kura's website. Now you must wait until IOTA's API is ready (this could take about 5 minutes in the rasps).

     device$ tail -f /var/log/kura-log # api.init() done

After a few seconds, execute the coordinator to create a IOTA milestone.

     local$ ./coordinator.sh <device> <port> # ./coordinator.sh 192.168.1.100 14700

After that you can use your wallet, log into one of your seeds, and attach addresses until you see your full balance.