# Goal:

Once fabric able to use Modular-crypto-service swtich crypto alg from ecdsa to others.
On fabric sdk side, we need aline with the changes, so that from client to network able to use same crypto alg/crypto cruve etc...

# Java sample code?

As interface defined in https://github.com/hyperledger/fabric-sdk-java/blob/master/src/main/java/org/hyperledger/fabric/sdk/security/CryptoSuite.java

and the factory interface defiend https://github.com/hyperledger/fabric-sdk-java/blob/master/src/main/java/org/hyperledger/fabric/sdk/security/CryptoSuiteFactory.java

for java, we can enhance the https://github.com/hyperledger/fabric-sdk-java/blob/master/src/main/java/org/hyperledger/fabric/sdk/security/HLSDKJCryptoSuiteFactory.java

supports a dynamic loader way, as `classforname` to support a Modular-crypto-service in jar files. which implements factory and cryptosuite interface.

# Nodejs sample code?

TBD

function as variable?