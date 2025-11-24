_In Beta - Let me know if you find specific issues_

# XDC Masternode Rewards Calculator

Live site is at: https://s4njk4n.github.io/XDC_Masternode_Rewards_Calculator/

![XDC Masternode Rewards Checker](XDC_Masternode_Rewards_Checker2.jpg)

This is a simple web tool to figure out the rewards your XDC masternode earned over a specific time period. It looks at your wallet's balance changes and subtracts out any transactions or fees to infer what must have come from masternode rewards on the XDC network.

I built this because tracking masternode rewards manually is a pain, especially if you're not deep into blockchain.

## What It Does

The tool calculates "inferred" rewards for a masternode owner address between two dates. "Inferred" means it doesn't directly query rewards from the network; instead, it looks at how your balance changed and accounts for any sends, receives, or gas fees in that period. Whatever's left over is assumed to be from masternode payouts.

It works on the XDC mainnet (chain ID 50). You provide:
- A start and end date (like 2023-01-01 to 2023-12-31).
- Your masternode owner address (starts with 0x or xdc).
- An Etherscan API key (more on that below).

It spits out the total inferred rewards in XDC, plus a breakdown of balances, transfers, and fees for transparency.

## How It Works

1. **Date to Blocks Conversion**: It takes your start and end dates, converts them to Unix timestamps, and uses binary search on the blockchain to find the exact block numbers where your period starts and ends.

2. **Balance Checks**: It grabs your wallet balance right before the start block and at the end block using RPC calls to an XDC node (default is a public one from Ankr).

3. **Transaction History**: Using the Etherscan API, it pulls all normal and internal transactions for your address in that block range. It tallies up:
   - Money coming in (transfers to you).
   - Money going out (transfers from you).
   - Gas fees you paid for transactions.

4. **Reward Calculation**:
   - Net change = End balance - Start balance.
   - Adjustments = Incoming transfers - Outgoing transfers - Fees.
   - Rewards = Net change - Adjustments.

   Everything's handled in wei (the smallest unit of XDC, like satoshis in Bitcoin) to avoid rounding errors, then converted to readable XDC amounts.

It shows a log of everything in the output box so you can see what's happening.

Note: This assumes all unexplained balance increases are from masternode rewards. If there's other stuff like airdrops or whatever hitting your wallet, it might skew things. Also, it only works for XDC mainnet.

## How to Use It

1. **Open the Page**

2. **Fill in the Fields**:
   - **RPC URL**: This is the endpoint to talk to the XDC blockchain. Default is https://rpc.ankr.com/xdc, which is free and public. You can change it if you have your own node or prefer another provider.
   - **Start Date**: The beginning of the period, format YYYY-MM-DD (e.g., 2024-01-01).
   - **End Date**: The end, same format. Make sure it's after the start date.
   - **Masternode Owner Address**: Your wallet address that owns the masternode. Can start with "xdc" or "0x" (both prefixes are supported on the page).
   - **Etherscan API Key**: Required for transaction data. See below on how to get one.

3. **Hit "Calculate Rewards"**: It'll run the calc and show progress in the output box below the form. Scroll down to see the final rewards at the end.

If something goes wrong (bad input, network error), it'll show an error in red.

### What is an API Key and Why Do I Need It?

An API key is like a password that lets this tool access data from Etherscan's servers without abusing their service. Etherscan is a blockchain explorer (like a search engine for XDC transactions). Without the key, they won't let you pull transaction lists, which are needed to account for sends/receives/fees.

The key keeps track of your usage so they can limit free users and charge heavy ones. For this tool, the free tier is plenty—you get 100,000 requests per day, and a single calc might use a handful.

### How to Get an Etherscan API Key

1. Go to https://etherscan.io/register (it's free).
2. Sign up with an email and password. Confirm your email if asked.
3. Once logged in, go to https://etherscan.io/myapikey.
4. Click "Add" to create a new key. Give it a name like "XDC Rewards Calc".
5. Copy the long string it gives you—that's your API key.
6. Paste it into the tool's input field.

That's it. The key doesn't cost anything for basic use, and you can make multiple if needed. Keep it private; don't share it.

## Contact

If you run into issues, contact me on Telegram (@s4njk4n), Discord (@s4njk4n), X (@s4njk4n), or GitHub (s4njk4n).

Back to the main outpost: [XDC Outpost](https://s4njk4n.github.io/XDCOutpost/)

© 2025 XDC Outpost. Powered by s4njk4n.
