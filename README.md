Task Details:

Write a demo programme for the lens protocol and implement the below functionality on this.
-Mint User profile

# Lens Protocol

The Lens Protocol is a decentralized, non-custodial social graph. Lens implements unique, on-chain social interaction mechanisms analogous to commonly understood Web2 social media interactions, but significantly expanded with unique functionality that empower communities to form and participants to own their own social graph.

## Setup

Steps to implement the same:

1: ```git clone https://github.com/aave/lens-protocol.git```

2: ```yarn or npm install```

3: ```npm run compile```

starting the local blockchain 

4:```npx hardhat node```

on a another terminal please deploy the full protocol

5: ```npm run full-deploy-local```

## Mint User Profile
The Program can be find at LENS-PROTOCOL-MAIN/Tasks/create-profile.ts

<img width="1428" alt="Screenshot 2022-06-17 at 10 00 51 AM" src="https://user-images.githubusercontent.com/86094155/174225295-8ecf3f3c-1daa-46fd-80cc-9dd693a82845.png">

This is the code overview of the Task.

``npx hardhat create-profile --network localhost``

The command to execute the create and mint a profile would be used like this after running and deploying the protocol in the local machine or if in a docker container than in the executing container.


## Collecting the minted NFT of The User

The Program collect can be find at LENS-PROTOCOL-MAIN/Tasks/collect.ts

<img width="1432" alt="Screenshot 2022-06-17 at 10 30 06 AM" src="https://user-images.githubusercontent.com/86094155/174228478-480844eb-5e42-4ce2-ae70-bdbd08a8ca49.png">


This is the code overview of the Collect program which will enable the protocol to collect an nft and the following command need to be executed for collecting the nft

``npx hardhat collect --network localhost``



## Protocol Overview

The Lens Protocol transfers ownership of social graphs to the participants of that graph themselves. This is achieved by creating direct links between `profiles` and their `followers`, while allowing fine-grained control of additional logic, including monetization, to be executed during those interactions on a profile-by-profile basis.

Here's how it works...

### Profiles

Any address can create a profile and receive an ERC-721 `Lens Profile` NFT. Profiles are represented by a `ProfileStruct`:

```
/**
 * @notice A struct containing profile data.
 *
 * @param pubCount The number of publications made to this profile.
 * @param followNFT The address of the followNFT associated with this profile, can be empty..
 * @param followModule The address of the current follow module in use by this profile, can be empty.
 * @param handle The profile's associated handle.
 * @param uri The URI to be displayed for the profile NFT.
 */
struct ProfileStruct {
    uint256 pubCount;
    address followNFT;
    address followModule;
    string handle;
    string uri;
}
```

Profiles have a specific URI associated with them, which is meant to include metadata, such as a link to a profile picture or a display name for instance, the JSON standard for this URI is not yet determined. Profile owners can always change their follow module or profile URI.


# Collects

Collecting works in a modular fashion as well, every publication (except mirrors) requires a `Collect Module` to be selected and initialized. This module, similarly to follow modules, can contain any arbitrary logic to be executed upon collects. Successful collects result in a new, unique NFT being minted, essentially as a saved copy of the original publication. There is one deployed collect NFT contract per publication, and it's deployed upon the first successful collect.

When a mirror is collected, what happens behind the scenes is the original, mirrored publication is collected, and the mirror publisher's profile ID is passed as a "referrer." This allows neat functionality where collect modules that incur a fee can, for instance, reward referrals. Note that the `Collected` event, which is emitted upon collection, indexes the profile and publication directly being passed, which, in case of a mirror, is different than the actual original publication getting collected (which is emitted unindexed).
