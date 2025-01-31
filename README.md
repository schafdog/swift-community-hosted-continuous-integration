# Swift Community-Hosted Continuous Integration

[Swift Community-Hosted CI](https://ci-external.swift.org) is an extension of Swift CI that allows the community to add additional platforms. Community members can volunteer to host new platforms and they are responsible for maintaining the host nodes. The maintainer will provide a build preset which will be periodically built on the node. This allows the Swift community to see the impact of changes on a greater range of platforms.

## Current List of Nodes
   * Fedora 33
   * Fedora Rawhide
   * ARMv7 for Debian "Stretch"
   * Ubuntu 16.04
   * Ubuntu 18.04 for TensorFlow
   * PPC64LE for Ubuntu 16.04
   * Android
   * macOS 10.13 for TensorFlow
   * Debian 10
   * Debian 11/Bullseye
   * wasm32 cross-compiled from Ubuntu 20.04


## Add Nodes

1. Create a pull request 
    * Add new JSON file under nodes directory -  nodes/<platform>_<os_version>.json

```json
{
   "contact":{
      "name":"Full name",
      "email":"Email address",
      "company":"Company name (optional)"
   },
   "node":{
      "platform":"Platform name",
      "os_version":"Operating system version"
   },
   "jobs":[
      {
         "display_name":"Swift - <PLATFORM> (Tools <TOOLS_CONFIG>, Stdlib <STDLIB_CONFIG>) (<BRANCH>))",
         "branch":"Swift branch",
         "preset":"Build preset from utils/build-preset.ini"
      }
   ]
}
```

Example:
File name: macOS_10_13.json

```json
{
   "contact":{
      "name":"Mishal Shah",
      "email":"example@apple.com",
      "company":"Apple Inc"
   },
   "node":{
      "platform":"macOS",
      "os_version":"macOS 10.13"
   },
   "jobs":[
      {
         "display_name":"Swift - macOS (Tools RA, Stdlib RD) (master)",
         "branch":"master",
         "preset":"buildbot_incremental,tools=RA,stdlib=RD,build"
      }
   ]
}
```

2. Verify preset builds on the server:
    * Clone Swift 
      `git clone https://github.com/apple/swift.git`
    * Clone all other repositories 
      `./swift/utils/update-checkout --scheme <BRANCH> --clone`
    * Build + Test
      `./swift/utils/build-script --preset=<PRESET>`

3. Once the pull request has been merged, you will receive an email with a set of terms and conditions to agree to, and to exchange public key information for connecting to the CI system.

4. You will be required to provide the following information in the email:
    * Agreement to terms and conditions
    * IP address
    * Username

## Maintaining Node

The node maintainer is responsible for updating the OS with security patches, keeping the host online, and installing any required packages to build the Swift compiler on the node. In the event that the host node becomes unaccessible or offline, the maintainer is responsible for bringing the node online within two weeks of being notified. Otherwise, an un-maintained node may be removed from the CI.
