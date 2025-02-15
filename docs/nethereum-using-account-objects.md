# Using Account Objects with Nethereum

This document is a Workbook, an interactive document where you can run code.
To run workbooks natively, you can:

* [Install the runtime](https://docs.microsoft.com/en-us/xamarin/tools/workbooks/install)

* [Download the native file for this document](http://docs.nethereum.com/en/latest/Nethereum.Workbooks/docs/nethereum-using-account-objects.workbook)

The entirety of Nethereum workbooks can be found [here](https://github.com/Nethereum/Nethereum.Workbooks)

Documentation about Nethereum can be found at: <https://docs.nethereum.com>

## What happens when you send a transaction with Ethereum?

Transactions can be put in two categories, those sent "manually" (without using an Ethereum client) and those that are managed by an Ethereum client.

To address those two cases, Nethereum provides `account` and `managed account` objects.

The following will demonstrate how to use Nethereum to send a transaction with Nethereum `managed` and `regular` accounts.

## Prerequisites:

First, let's download the test chain matching your environment from <https://github.com/Nethereum/Testchains>

Start a Geth chain (geth-clique-linux\\, geth-clique-windows\\ or geth-clique-mac\\) using **startgeth.bat** (Windows) or **startgeth.sh** (Mac/Linux). The chain is setup with the Proof of Authority consensus and will start the mining process immediately.

```csharp
#r "Nethereum.Web3"
```

```csharp
#r "Nethereum.Accounts"
```

Then, let's add the using statement to Nethereum.Web3.

```csharp
using Nethereum.Web3;
using Nethereum.Hex.HexTypes;
using Nethereum.Web3.Accounts;
using Nethereum.Web3.Accounts.Managed;
```

We can now create a new instance of Web3:

```csharp
var web3 = new Web3();
```

## Sending a transaction

To send a transaction you will either manage your account and sign the raw transaction locally, or the account will be managed by the client (Parity / Geth), requiring to send the password at the time of sending a transaction or unlock the account before hand.

In Nethereum.Web3, to simplify the process, there are two types of accounts that you can use. An "Account" object or a "ManagedAccount" object. Both store the account information required to send a transaction, private key, or password.

At the time of sending a transaction, the right method to deliver the transaction will be chosen. If using Nethereum `TransactionManager`, deploying a contract or using a contract function, the transaction will either be signed offline using the private key or a personal\_sendTransaction message will be sent using the password.

The below explains how to:

### Sending a transaction with an `account` object

Here is how to set up a new account by creating an `account` object instance:

```csharp
var privateKey = "0xb5b1870957d373ef0eeffecc6e4812c0fd08f554b37b233526acc331bf1544f7";
var account = new Account(privateKey);
```

```csharp
var toAddress = "0x12890D2cce102216644c59daE5baed380d84830c";
var transaction = await web3.TransactionManager.SendTransactionAsync(account.Address, toAddress, new Nethereum.Hex.HexTypes.HexBigInteger(1));
```

### Sending a transaction with a `managed account` object

As said earlier: Nethereum's managed accounts are maintained by the Ethereum client (geth/parity), allowing the automatic signing of transactions and the secure management of the account's private key:

```csharp
var senderAddress = "0x12890d2cce102216644c59daE5baed380d84830c";
var addressTo = "0x13f022d72158410433cbd66f5dd8bf6d2d129924";
var password = "password";
var managedAccount = new ManagedAccount(senderAddress, password);
var web3ManagedAccount = new Web3(managedAccount);
```

We can now perform our transaction using the `TransactionManager`, the signing will be made automatically by the Ethereum client in use:

```csharp
var transactionManagedAccount = await web3.TransactionManager.SendTransactionAsync(account.Address, addressTo, new HexBigInteger(20));
```
