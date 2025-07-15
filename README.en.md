# Based on the Intent Framework habitual recommendation, common revisit sample code is recommended

## Introduction

- This example is based on the Intent Framework, using the `@kit.IntentsKit` implements intent sharing, using the `@kit.AbilityKit` `InsightIntentExecutor`
  Implement intent calls. Implement game revisits based on the parameters invoked by the intent.

## Preview

| Home page                                                    | Game page                                    | Xiaoyi Card Display Shared Intent                                                                       | Click the intent card to make a follow-up                                |
|--------------------------------------------------------------|---------------------------------------------------|---------------------------------------------------------------------------------------------------------|----------------------------------------------------|
| ![show home page](screenshots/device/1.png "show home page") | ![Game page](screenshots/device/2.png "Game page") | ![Xiaoyi Card Display Shared Intent 0](screenshots/device/3.png "Xiaoyi Card Display Shared Intent 0") | ![Intent to call 0 ](screenshots/device/2.png "Intent to call 0 ") |

Directions of use:
1. Click on the `Mini Game 1 or 2` card to enter the game card page. When you enter the game page, you will call the shareIntent() interface. At the bottom of the game card page, the API execution status is displayed.
2. After the system completes the processing of the shared intent, the shared intent will be displayed in the card suggested by Xiaoyi.
3. Click on the corresponding Xiaoyi card displayed, and the sample app will be re-opened to complete the revisit of the game card.

## Project Directory

```
├── entry/src/main/ets
│  ├── common
│  │  ├── utils
│  │  │  ├── FileReader.ets              // File Reader class
│  │  │  └── Logger.ets                  // Logger class
│  │  └── constants
│  │     └── CommonConstants.ets         // Common constants class
│  ├── entryability
│  │  └── EntryAbility.ets               // Entry Ability
│  ├── pages
│  │  ├── Index.ets                      // Entry page of the application
│  │  └── PlayPage.ets                   // Detail page
│  └── insightintents
│     └── IntentExecutorImpl.ets         // Intent execution class
└── entry/src/main/resources             // Application resource directory
   ├── base
   |  └── profile
   |     ├── insight_intent.json         // Intent registration configuration
   |     └── main_pages.json             // Application page list
   └── rawfile
      ├── shareIntent.json               // Intent sharing data example
      └── game.json                      // Game information example
```

## Implementation Details

For intent to share the source code, see the shareIntent method in PlayPage.ets, and for the intent, call the onExecuteInUiAbilityForegroundMode method in IntentExecutorImpl.ets for the source code

* **Homepage**: Read the game information from the game.json file, generate the game card for ForEach, and jump to the game page through navPathStack.replacePathByName in the onClick event of the card
* **Game page**: The game page displays game-related information based on navigation parameters
* **Intent sharing**: The game page calls the shareIntent method in the aboutToAppear event to filter out the relevant intent data from the pre-read shareIntent.json data based on the game id, and then calls insightIntent.shareIntent
  APIs enable intent data sharing
* **Intent call**: In the onExecuteInUIAbilityForegroundMode method, use eventHub.emit to broadcast the event and pass the entityId game id parameter.
  Index.ets uses eventHub.on to listen to events and navPathStack.replacePathByName to trigger a jump to the game page
* Intent: Pass parameters to the homepage through eventHub during hot start, and pass want parameters to the homepage through the onCreate method with the help of localStorage objects during cold start
* In this example, the intent call does not involve too much in the business logic and UI logic, but only passes the relevant parameters to the business through different channels, and hands over the initiative of page jump to the business itself.
  The onExecuteInUIAbilityForegroundMode operation also provides a WindowStage instance, which can use windowStage.loadContent to load specific pages.

## Required Permissions

### Dependencies

1. This example depends on `@ohos/hvigor-ohos-plugin`.
2. If using a DevEco Studio version newer than the recommended version for this example, update the hvigor plugin version as prompted by DevEco Studio.
3. An internet connection is required to log in to a Huawei account and agree to Xiaoyi Suggestions` user agreement and privacy policy.

### Constraints

1. <font>**Testing for intent sharing and invocation cannot be independently completed by developers. Please follow the [Intents Kit integration process](https://developer.huawei.com/consumer/en/doc/harmonyos-guides/intents-habit-rec-dp-self-validation) and submit an acceptance application via email to the Huawei intent framework contact. The contact will assist the developer in completing the testing and acceptance.**</font>
2. This example only runs on standard systems and supports Huawei phones.
3. HarmonyOS version: HarmonyOS 5.0.5 Release or later.
4. DevEco Studio version: DevEco Studio 5.0.5 Release or later.
5. HarmonyOS SDK version: HarmonyOS 5.0.5 Release SDK or later.