# ICONbet Game Developer Guide


## Draft version 0.0.6

March 25, 2021

# Table of Contents

- [Overview](#overview)
- [The ICONbet platform](#the-ICONbet-platform)
	- [Login](#login)
	- [Gameplay](#gameplay)
	- [Gameplay History](#gameplay-history)
- [ICONbet Developer Program](#iconbet-developer-program)
	- [Objective](#objectives)
	- [Current Developer Agreement](#current-developer-agreement)
	- [Steps to submit a game for ICONbet](#steps-to-submit-a-game-for-iconbet)
- [SCORE Development Guide](#score-development-guide)
	- [Code Guidelines](#code-guidelines)
	- [Requirements in the Game SCORE](#requirements-in-the-game-score)
	- [SCOREs for integration with ICONbet platform](#scores-for-integration-with-iconbet-platform)
- [Frontend Integration](#frontend-integration)

# Overview

This document aims to go through the gameplay flow on the ICONbet platform and introduce the ICONbet Developer Program. The developer program will make it clear and easy for any third party developers to develop a game and have it approved and integrated into the platform in a manner consistent with DAO principles.


# The ICONbet platform

The ICONbet platform (iconbet.io) is a crypto gambling platform for the ICON blockchain owned and operated by members of the ICONbet DAO. Membership of the DAO is represented by TAP token holdings, and TAP tokens can be earned through playing ICONbet or purchasing on third party services.

The games hosted on iconbet.io have following components to them:


## Login 

The user needs to login before they can play any of the games in ICONbet platform. Login is done through the ICONex wallet where it sends the wallet address to the backend. The backend does the following checks : 

- Check if icon address is valid 
- Check if the address associated with user exists in the database (returning user)


## Gameplay
After a successful login, the user can proceed for the gameplay. Gameplay proceeds in the following manner: 

### Frontend : 
1. User place bet at the front end 
2. Frontend prepares the transaction as per the Icon Score, which is to be taken in the blockchain.
3. ICX browser wallet is used for signing transactions compiled docs and broadcasts it to the ICON blockchain leaving behind the txid.
4. Frontend establishes the socket connection with the backend to pass the txid and waits for the response till the predefined time of 30sec.
5. TXIDs are stored in the database with unprocessed flags 

 ### Backend: 

1. Various jobs are assigned in the backend that continuously check the database for new unprocessed txid 
2. As soon as new unprocessed Txid is found, backend checks the Icon tracker for information related to the particular transaction 
3. Various events are emitted via ICON score which gives information about the gameplay such as winning number, total payout etc which is fetched out in backend via the txid and required data are filtered 
4. On the basis of fetched data, the logical calculation is done in the backend to determine the winning and losing cases, and the response is sent to the frontend via the established socket connection along with other information to be shown to the user.

### Frontend: 
1. After receiving a response from the backend, a socket connection is closed and data is manipulated as per to be displayed to the user 



## Gameplay History

### Frontend: 
When a user selected to see the gameplay history he clicks the history button, which triggers an exposed API in the backend, which gives the gameplay history data which is manipulated as per the design need 

### Backend: 
Once the unprocessed transactions are scanned and the response is sent to the frontend via a socket connection, the data is stored in MongoDB for future reference. This data is fetched from the database and sent to frontend as a response of API call for gameplay history 


# ICONbet Developer Program


## Objectives

Given that game developers are members of the ICONbet community and ecosystem who will participate according to the benefits they receive from it and the ease of participating, there are four objectives:
 - To clearly provide the steps, guidelines, requirements, boundaries and rewards for getting a game accepted for inclusion on the ICONbet platform.
  - To define a required standards review that games must pass before advancement to subsequent stages and launch to prevent the overall image of the platform being degraded due to poor execution.
   - Provide an open knowledge sharing environment to encourage progressive improvement of game standards and platform useability.
   - Progressively move towards a DAO governed platform.

Satisfying the objectives will mitigate potential risks including the technical risk of losing all of the treasury and risk of degradation of our image of producing unique, attractive games.


## Current Developer Agreement
Current breakdown of income from games on ICONbet:
*   In-House games:
    *   100% to Community Pool
*   Developer games:
    *   20% to Developer
    *   80% to Community Pool
*   Community pool breakdown:
    *   5% to next day’s community pool
    *   5% to DAOfund
    *   5% to wager war
    *   5% to COMP Token
    *   80% to staked TAP earnings pool

The DAOfund is a pool of funds used to pay for expenses such as marketing and promotions as proposed and voted on by the community.

The COMP Tokens are used by ICONbet to kick back a little to players according to how much they lose on the platform in a given day.

The developer will provide an address to which their share of funds will be sent each day. If those funds will be sent to a contract the fallback method that receives them is not allowed to execute any code other than emitting an event. This is to avoid adding any load to ICONbet game transactions, which may already be doing additional work to drive ICONbet itself.


## Steps to submit a game for ICONbet



### Initial Game Proposal Submission
The initial submission gets the process started. You will have a forum channel where you can announce your game and discuss it with the community. For the time being, before we launch the full ICONbet DAO governance portal, please solicit initial feedback for a game proposal in the ICONbet Official Telegram channel.


#### What the proposal must contain?

1. A name
2. A brief text description
3. 50 ICX fee
2. Analysis, Simulation, and Description

    This stage establishes the technical and statistical specification of the game so everyone is clear on what will be built and how it will affect the ICONbet platform. ICONbet will provide tools to assist with the statistical calculations. This will make it easier to compare all games on the same footing. 


#### Required with the Submission to GitHub repo
1. Text description of the game mechanics
2. Links to any online statistical documentation, like wizardofodds.com
  3. Analytical calculation of house edge, variance, and max bet, including steps for derivation (show your work) if analytical calculation is feasible.
  4. Documented code in Python used to generate the simulated treasury balance for all wager types, derived house edge, variance, and max bet (simulation statistics need to match those from the analytical calculation. A detailed example for this requirement is given below.)
  5. Plots from the simulation runs for 1000 trials of 100,000 wagers
  6. For verification purposes the reviewer needs to be able to easily read and understand the code, then run it and plot the results. Do not put the burden on the reviewer to do extensive coding or statistical analysis from scratch to verify your submission. If verification is difficult your submission may be returned or it may take a long time to get approval.
  7. To facilitate this process the platform provides a github repo with a Jupyter Notebook template. Please fork this repo and create a branch then submit a pull request with an additional folder containing the code for your game.

### Community Vote

Staked Tap holders will vote for the approval of the game in the ICONbet governance portal. The proposal must receive approval from 51% of those that vote. Quorum is 20% of staked TAP

### Testnet SCORE Operation
Once the developer has deployed on the testnet:

1. The game will be exercised with a script to confirm the statistics determined in the calculation and simulation.
2. The game will be connected to the ICONbet testnet treasury.
3. Integration with ICONbet Development Server

    Game will be deployed on a development server and integrated with the ICONbet test server. All integration and user testing can happen at this point.

### Deploy to Mainnet
Ideally, deployment to the mainnet would be controlled by the DAO contracts, but it is not yet possible to deploy a contract from another contract. Until that capability is available third party game contracts will be deployed by the ICONbet team.

Once the final version of the game SCORE has been received by the ICONbet team an audit will be performed. As documentation for the audit a table detailing the transactions with the ICONbet treasury will be required. It shall show all calls to the take_wager, take_rake, and wager_payout methods on the treasury SCORE and the resulting treasury balance, along with the changes in accumulated wagers and accumulated payouts that will be recorded. This shall be shown for each possible flow of the game from start to conclusion, with line numbers for the points in the code where each transaction occurs.

The developer will supply a Deployment Plan document based on experience deploying and configuring the game on the testnet and test1 server. Once the game has been integrated with the ICONbet SCOREs, frontend integration will be tested on the ICONbet staging server at dev1.iconbet.io.

### Final Acceptance and Going Live

Once the game has been tested on dev1 the frontend changes will be pushed live to the production server. 



# SCORE Development Guide

## Code Guidelines

We would like to encourage developers to submit high quality code for ICONbet game SCOREs. We can all learn from each other. To make the code more readable and reduce the time for code review and approval ICONbet game SCOREs should follow these guidelines.

1. Basic PEP8 Style
2. 4-space indent
3. Descriptive DocStrings for all but trivial methods
4. _take_wager and _request_payout both occur in the same transaction.
5. Games that send a rake to the treasury should do so in the transaction where the wager is received.
6. A method to pause the game is required for safety in case of bugs. Should be callable by the 3rd party developers or by a call from the Game Authorization SCORE.
7. Token or ICX calculations should all be done with integers if possible to avoid dust and rounding errors. In some applications it may not matter, but the preference is for full precision integer calculations.


## Requirements in the Game SCORE

#### Score owner method
Each score which wants to be integrated in ICONbet should have an additional readonly method which returns the score owner address.
```python
  @external(readonly=True)
  def get_score_owner(self) -> Address:
     """
     A function to return the owner of this score.
     :return: Owner address of this score
     :rtype: :class:`iconservice.base.address.Address`
     """
     return self.owner
  ```

#### Creating an interface with the treasury SCORE
The score must implement an interface to interact with the treasury SCORE. 

Treasury SCORE on mainnet - [https://tracker.icon.foundation/contract/cx1b97c1abfd001d5cd0b5a3f93f22cccfea77e34e](https://tracker.icon.foundation/contract/cx1b97c1abfd001d5cd0b5a3f93f22cccfea77e34e)

On testnet - [https://bicon.tracker.solidwallet.io/contract/cxecc3d0484195e174109c4cf8219cad5cbda8703b](https://bicon.tracker.solidwallet.io/contract/cxecc3d0484195e174109c4cf8219cad5cbda8703b)
```python
   # An interface to treasury score
   class TreasuryInterface(InterfaceScore):
      @interface
      def get_treasury_min(self) -> int:
          pass

      @interface
      def take_wager(self, _amount: int) -> None:
          pass

      @interface
      def wager_payout(self, _payout: int) -> None:
          pass
   ```


#### Sending wager data and requesting payout
##### Game using ICONbet treasury

 - Send the  wager amount using icx transfer
- Call take_wager method providing the wager amount
 ```python
 self.icx.transfer(self._treasury_score.get(), self.msg.value)
 treasury_score.take_wager(self.msg.value)
 ```

Here, self._treasury_score.get() gives the contract address of Treasury SCORE and treasury_score is the interface score for treasury.
- Call wager_payout method for requesting payout in case of win
```python
treasury_score.wager_payout(_payout)
```

##### Games not using the ICONbet treasury

  - Send the rake amount you want to provide to the treasury using icx transfer
- Call take_rake with the wager amount and payout amount you want to record in the treasury
```python
self.icx.transfer(self._treasury_score.get(), rake_amount)
treasury_score.take_rake(wager, payout)
```

## SCOREs for integration with ICONbet platform

1. **Submit game proposal**
The game proposal can be submitted by interacting with Game Authorization SCORE. 

**Mainnet:** [https://tracker.icon.foundation/contract/cx71ea1dbfbd01b0892c3ac660e998d6fa71408878](https://tracker.icon.foundation/contract/cx71ea1dbfbd01b0892c3ac660e998d6fa71408878)

**Testnet:** [https://bicon.tracker.solidwallet.io/contract/cx54cc848c6e81c8301472b5bfe326b7e6d7d2a915](https://bicon.tracker.solidwallet.io/contract/cx54cc848c6e81c8301472b5bfe326b7e6d7d2a915)

- Fill all the necessary params about the game and invoke submit_game_proposal method of the GAS SCORE.
 - 50 ICX must be sent for placing the proposal

Field to be included in gamedata:-
```json
{
"name": ""(Name of the game, str),
"scoreAddress": " ", (User must submit a score address, the game can be completed or else the score can contain the boilerplate score required for ICONbet platform, Address)
"minBet": , (minBet must be greater than 100000000000000000(0.1 ICX), int)
"maxBet": , (maxBet in the game in loop, int)
"houseEdge": "", (house edge of the game in percentage, str)
"gameType": "", (Type of game, type should be either "Per wager settlement" or "Game defined interval settlement", str)
"revShareMetadata": "" ,(data about how would you share your revenue)
"revShareWalletAddress": "", (Wallet address in which you want to receive your percentage of the excess made by game)
"linkProofPage": "" , (link of the page showing the game statistics)
"gameUrlMainnet": "", (IP/Domain of the game in mainnet)
"gameUrlTestnet": "", (IP/Domain of the game in testnet)
}
```
2. **Wait for the proposal to be accepted**
	 - Call the method get_game_status in GAS providing your SCORE address.
	 - Proceed to next step if the status changes to proposalAccepted.

3. **Make your SCORE ready and change the game status to gameReady by invoking set_game_ready method in GAS providing your SCORE address.**

4. **Wait for your game to be approved. When the game gets approved, the status changes to gameApproved.**


# Frontend Integration


**Requirements in the frontend**

1. Login request
    The iframe game will receive the user's wallet address from the cookie. 
    _Cookie name: wallet_address_
    
    If the wallet address cannot be determined from the cookie, the game must perform a login request. The login request needs to be passed as an event.

    ```javascript
    export const askForLogin= () => {
     window.parent.dispatchEvent(
       new CustomEvent('LOGIN_REQUEST', {
    	 detail: {
    		//type: 'LOGIN_REQUEST'
       },
    }
    ));};
    ```
2. Update Leaderboard

    After a game finishes, the leaderboard can be updated by calling updateLeaderboard component.
    ```javascript
    export const updateLeaderboard = () => {
     window.parent.dispatchEvent(
       new CustomEvent('UPDATE_LEADERBOARD')
     );
    };
    ```
3. IFRAME request
```javascript
window.addEventListener(
       'IFRAME_REQUEST',
       event => {
     window.dispatchEvent(
       new CustomEvent('ICONEX_RELAY_REQUEST', {
    	 detail: event.detail,
    }
    ));
       },
       false
     );

```
4. IFRAME response
    ```javascript
    window.addEventListener(
       'ICONEX_RELAY_RESPONSE',
       event => {
     window.parent.dispatchEvent(
       new CustomEvent('IFRAME_RESPONSE', {
    	 detail: event.detail,
    }
    ));
       },
       false
     );
    ```


5. Dispatching transaction payload

    A sample code showing the interaction with the parent window in JS. 
    ```js
    export const createTransaction = ({ walletAddress, betAmounts, sideBet, value }) => {

     const paramsObj = getAmountParams(betAmounts, sideBet);
     const { IconConverter, IconBuilder, IconAmount } = IconService;
     const txnBuilder = new IconBuilder.CallTransactionBuilder();
     const txnData = txnBuilder
       .from(walletAddress)
       .to(process.env.REACT_APP_CONTRACT_ADDRESS)
       .nid(IconConverter.toBigNumber(3))
       .timestamp(new Date().getTime() * 1000)
       .stepLimit(IconConverter.toBigNumber(100000000))
       .version(IconConverter.toBigNumber(3))
       .method('take_action')
       .params(paramsObj)
       .value(IconAmount.of(value || 0, IconAmount.Unit.ICX).toLoop())
       .build();

     const txnPayload = {
       jsonrpc: '2.0',
       method: 'icx_sendTransaction',
       params: IconConverter.toRawTransaction(txnData),
       id: 50889,
     };

     window.parent.dispatchEvent(
       new CustomEvent('IFRAME_REQUEST', {
         detail: {
           type: 'REQUEST_JSON-RPC',
           payload: txnPayload,
         },
       }),
     );
    };

    ```