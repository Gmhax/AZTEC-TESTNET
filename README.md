# AZTEC-TESTNET
This guide will walk you through setting up and using the Aztec testnet. By the end, you'll have created an account, deployed a contract, and performed some basic operations.

##Key Terms
- In this guide you will see these terms:

- aztec: a command-line tool for interacting with aztec testnet (& sandbox local environments)
- aztec-nargo: a command-line tool for compiling contracts
- aztec.nr: a Noir library used for writing Aztec smart contracts
- aztec-wallet: A tool for creating and interacting with Aztec wallets
- sandbox: A local development environment


## STEP-BY-STEP AZTEC TESTNET SETUP IN GITHUB CODESPACE
- https://github.com/codespaces

## Step 1.Install Aztec CLI
```bash
bash -i <(curl -s https://install.aztec.network)
```
## Step 2.Make PATH permanent
```bash
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## Step 3.Setup Environment Variables
```bash
export NODE_URL=http://34.107.66.170
export SPONSORED_FPC_ADDRESS=0x0b27e30667202907fc700d50e9bc816be42f8141fae8b9f2281873dbdb9fc2e5
```

## Step 4.Start the Aztec Testnet
```bash
aztec-up alpha-testnet
```

- Create a working directory directly under /home/codespace:
```bash
mkdir -p /home/codespace/aztec-test
cd /home/codespace/aztec-test
```

## Step 5.Create & Register Account
- Create wallet: Save the details on notepad
```bash
aztec-wallet create-account \
    --register-only \
    --node-url $NODE_URL \
    --alias my-wallet
```
- Register contract with sponsor:
```bash
aztec-wallet register-contract \
    --node-url $NODE_URL \
    --from my-wallet \
    --alias sponsoredfpc \
    $SPONSORED_FPC_ADDRESS SponsoredFPC \
    --salt 0
```
- Deploy your account:
```bash
aztec-wallet deploy-account \
    --node-url $NODE_URL \
    --from my-wallet \
    --payment method=fpc-sponsored,fpc=contracts:sponsoredfpc \
    --register-class
```

- If you encounter an error like the one shown in the photo below, just run the deploy account command again until it proceeds successfully.
![image](https://github.com/user-attachments/assets/d92eb52b-9a71-4078-be1c-fcfb362047e6)


- If the status looks like this, it's fine—just proceed to the next phase.
![image](https://github.com/user-attachments/assets/204e5412-5ff7-4310-b34b-921d3098382c)

## Step 6.Deploy Token Contract
```bash
aztec-wallet deploy \
    --node-url $NODE_URL \
    --from accounts:my-wallet \
    --payment method=fpc-sponsored,fpc=contracts:sponsoredfpc \
    --alias token \
    TokenContract \
    --args accounts:my-wallet Token TOK 18
```

## Step 7.Mint Tokens (Private)
```bash
aztec-wallet send mint_to_private \
    --node-url $NODE_URL \
    --from accounts:my-wallet \
    --payment method=fpc-sponsored,fpc=contracts:sponsoredfpc \
    --contract-address last \
    --args accounts:my-wallet accounts:my-wallet 10
```
If you encounter an error like the one shown in the photo below, it's fine your wallet  is still syncing (PXE DATA). Just run the command again until it succeeds. (But if after 20 minutes the error still persists (like 'Error ABI'), just repeat Step 6 to redeploy.)
![image](https://github.com/user-attachments/assets/b1dd0c2b-2c42-43a6-b9e7-854f93b31d5d)

## Step 8.Transfer to Public Balance
```bash
aztec-wallet send transfer_to_public \
    --node-url $NODE_URL \
    --from accounts:my-wallet \
    --payment method=fpc-sponsored,fpc=contracts:sponsoredfpc \
    --contract-address last \
    --args accounts:my-wallet accounts:my-wallet 2 0
```

## Step 9.Check Balances
- Private:
```bash
aztec-wallet simulate balance_of_private \
    --node-url $NODE_URL \
    --from my-wallet \
    --contract-address last \
    --args accounts:my-wallet
```
- Public:
```bash
aztec-wallet simulate balance_of_public \
    --node-url $NODE_URL \
    --from my-wallet \
    --contract-address last \
    --args accounts:my-wallet
```




Summary
Step	      Task
1.	    Install Aztec CLI
2.	    Set testnet variables
3.	    Start the testnet version
4.	    Create and register wallet
5.	    Deploy account
6.	    Deploy token contract
7.	    Mint tokens
8.	    Transfer to public
9.	    Check balances














