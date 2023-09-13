# static-website
To see a static website of the current repo
## Description

This repository is for E2E (full user journey's) Load testing scenario. Confluence page (reports and more information):
https://livesport.atlassian.net/wiki/spaces/TPD/pages/6338545662/Load+Testing

## Limitations

The scripts were tested and executed on few markets (mostly on DE, IT, US) and we can not guarantee that every scenario
will work as expected on every market where DAZN is available. Please contact developers for clarifications.

## Ready scenarios [ENV variable to provide in run command]

TV Platform:

1. Welcome Page -> Sign In [welcomePageAndSignIn]
   (User accessing welcome page on TV platform and performs sign-in)
2. Catalog [catalog]
   (User, already logged in before (with stored session) accessing DAZN via catalog (home) page)
3. Playback [playback]
   (Authenticated user start watching an event, VoD or Live)

Web:

1. Redeem Partner Access Code and Sign Up  [pacRedeemAndSignUp]
   (Partner user redeems partner giftcode and signing up for DAZN)
2. Redeem Standard Access Code and Sign Up [sacRedeemAndSignUp]
   (User redeems DAZN Giftcode and signing up for DAZN)
3. NFL Freemium Sign up -> Catalog -> Playback [nflSignUpAndStartPlayback]
   (Full e2e journey when user sign up for NFL freemium, accessing DAZN catalog and start watching NFL content)
4. Welcome Page -> Sign In [webSignIn]
   (User accessing welcome page on web and performs sign-in)
5. Sign up with PPV purchase [signUpWithPpvPurchase]
   (User signing up and buying PPV as a bundle)

Mobile:

1. SignUp on iOS (3PP) [iosSignUp]
   (User signing up to DAZN using Apple device)
2. SignUp and purchase PPV using iOS (3PP) [iosSignUpWithPpvPurchase]
   (User signing up to DAZN and purchasing PPV using iOS (3pp))

## K6 Project dashboards

https://app.k6.io/projects/3618545
Please contact repo developers if you don't have access.

## Running load tests

1. K6 Installation:

Please follow instructions here:

https://k6.io/docs/get-started/installation/

2. Needed variable explanation:

| Variable          | default | required for [scenario(s)]               | Description                                                 | values                               |
|-------------------|---------|------------------------------------------|-------------------------------------------------------------|--------------------------------------|
| LOAD_PROFILE      | -       | [all]                                    | The scenario you need to execute. See scenarios.ts enum     | see #Ready scenarios section         |
| PLATFORM          | -       | [all]                                    | Platform to execute against.                                | `tvweb`, `web`, `ios`                |
| ENV               | stage   | [all]                                    | Environment to execute load test against.                   | `stage` or `prod`                    |
| COUNTRY           | -       | [all]                                    | Market (country) we want to execute load against.           | `us`, `gb`, `it`, `jp`, `de`         |
| LOAD_DISTRIBUTION | eu      | [all]                                    | `eu` or `us`, `it` K6 distribution - more info in config.ts | `eu`, `us`, `it`                     |
| TEST_USERS_PATH   | -       | [catalog, playback]                      | Full path to the local file with test users.                |                                      |
| GIFTCODES_PATH    | -       | [pacRedeemAndSignUp, sacRedeemAndSignUp] | Full path to the local file with giftcodes.                 |                                      |
| ASSET_ID          | -       | [playback]                               | AssetId of playback event.                                  | example: `lytzhfcphs9l1pyrm2gbyy9th` |
| PLAYBACK_TYPE     | live    | [playback]                               | Type of the playback event.                                 | `vod` or `live`                      |
| TEST_TYPE         | -       |  [all]                                   | Type of load test  | `debug` or `loadtest`                 |            

Please contact repo developers for getting file with test users and/or giftcodes.

3. Running tests on your local machine:

`Please be aware that if the env value on examples is not market with <value>, that means that this scenario supports only that value at the moment.`

How to run [catalog, welcomePageAndSignIn] scenarios:

```sh
npm run build && k6 run -e LOAD_PROFILE=<scenario> -e PLATFORM=tvweb -e ENV=<env> -e COUNTRY=<country> -e LOAD_DISTRIBUTION=<load-distribution> -e TEST_USERS_PATH=<PATH-TO-TEST-USERS-FILE> -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

How to run [playback] scenario:

1. Live event:

```sh
npm run build && k6 run -e LOAD_PROFILE=playback -e PLATFORM=tvweb -e ENV=prod -e COUNTRY=<country> -e LOAD_DISTRIBUTION=<load-distribution> -e ASSET_ID=<assetId> -e PLAYBACK_TYPE=live -e TEST_USERS_PATH=<PATH-TO-TEST-USERS-FILE> -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

2. VoD event

```sh
npm run build && k6 run -e LOAD_PROFILE=playback -e PLATFORM=tvweb -e ENV=prod -e COUNTRY=<country> -e LOAD_DISTRIBUTION=<load-distribution> -e ASSET_ID=<assetId> -e PLAYBACK_TYPE=vod -e TEST_USERS_PATH=<PATH-TO-TEST-USERS-FILE> -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

How to run [pacRedeemAndSignUp, sacRedeemAndSignUp] scenarios:

```sh
npm run build && k6 run -e LOAD_PROFILE=<scenario> -e PLATFORM=web -e ENV=<env> -e COUNTRY=<country> -e LOAD_DISTRIBUTION=<load-distribution> -e GIFTCODES_PATH=<PATH-TO-GIFT-CODES-FILE> -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

How to run [iosSignUp] scenario:

```sh
npm run build && k6 run -e LOAD_PROFILE=iosSignUp -e PLATFORM=ios -e ENV=<env> -e COUNTRY=us -e LOAD_DISTRIBUTION=us -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

How to run [iosSignUpWithPpvPurchase] scenario:

```sh
npm run build && k6 run -e LOAD_PROFILE=iosSignUpWithPpvPurchase -e PLATFORM=ios -e ENV=prod -e COUNTRY=us -e LOAD_DISTRIBUTION=us -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

How to run [nflSignUpAndStartPlayback] scenario:

```sh
npm run build && k6 run -e LOAD_PROFILE=nflSignUpAndStartPlayback -e PLATFORM=web -e ENV=<env> -e COUNTRY=<country> -e LOAD_DISTRIBUTION=<load-distribution> -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

How to run [webSignIn] scenario:

```sh
npm run build && k6 run -e LOAD_PROFILE=webSignIn -e PLATFORM=web -e ENV=prod -e COUNTRY=<country> -e LOAD_DISTRIBUTION=<load-distribution> -e TEST_USERS_PATH=<PATH-TO-TEST-USERS-FILE> -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

How to run [signUpWithPpvPurchase] scenario:

```sh
npm run build && k6 run -e LOAD_PROFILE=signUpWithPpvPurchase -e PLATFORM=web -e ENV=<env> -e COUNTRY=us -e LOAD_DISTRIBUTION=us -e TEST_TYPE=<test-type> --out csv=my_test_result.csv dist/index.js
 ```

Results will be stored in CSV file.

5. Running from K6 Cloud

! Please consult with repo developer's first if you want to run it on the cloud!

example:

```sh
export K6_CLOUD_TOKEN=<your_k6_token>
(you can find it on the k6 website in your profile)

npm run build && k6 cloud -e LOAD_PROFILE=catalog -e PLATFORM=tvweb -e COUNTRY=us -e ENV=stage -e LOAD_DISTRIBUTION=us -e TEST_USERS_PATH=<PATH TO TEST USERS FILE> dist/index.js 
```

## Contacts

Project Owner: Nitin Deshmukh: nitin.deshmukh@dazn.com Developers: Artem Redchyts: artem.redchyts@dazn.com Karthik
Koruvada: karthik.koruvada@dazn.com Likhitha Alla: likhitha.alla@dazn.com
