# Polkadot-Balance-Checker_Script   
This script allows you to connect to the Polkadot network and retrieve the balance of a specified account.
const { ApiPromise, WsProvider } = require('@polkadot/api');
const { Keyring } = require('@polkadot/keyring');

// Connect to the Polkadot network
async function connectToPolkadot() {
  // Create an instance of WsProvider with the specified Polkadot network WebSocket URL
  const provider = new WsProvider('wss://rpc.polkadot.io');

  // Create an instance of ApiPromise using the WsProvider
  const api = await ApiPromise.create({ provider });

  return api;
}

// Get the balance of an account
async function getAccountBalance(address) {
  const api = await connectToPolkadot();

  // Create a keyring instance
  const keyring = new Keyring({ type: 'sr25519' });

  // Get the account from the address
  const account = keyring.addFromAddress(address);

  // Retrieve the account balance
  const { data: { free: balance } } = await api.query.system.account(account.address);

  return balance;
}

// Usage example
(async () => {
  const address = '5E5hG7gfUq3J2p6wKZYFUDYs7CfnkT5bNwL7x9D6Y4sL6cUJ';

  // Get the balance of the specified account
  const balance = await getAccountBalance(address);

  console.log(`Account Balance: ${balance}`);
})();
