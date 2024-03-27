# Decentralized-Freelance-Designer-Marketplace
Créez une place de marché décentralisée pour les graphistes freelances, en utilisant des contrats intelligents pour garantir des paiements équitables et une protection des droits d'auteur.
from web3 import Web3
import json

# Connect to Ethereum testnet
w3 = Web3(Web3.HTTPProvider('https://ropsten.infura.io/v3/YOUR_INFURA_PROJECT_ID'))

# Contract details
contract_address = 'YOUR_CONTRACT_ADDRESS'
with open('FreelanceMarketplaceABI.json', 'r') as file:
    contract_abi = json.load(file)

contract = w3.eth.contract(address=contract_address, abi=contract_abi)

# Sample function to create a job
def create_job(title, price):
    account = w3.eth.account.privateKeyToAccount('YOUR_PRIVATE_KEY')
    nonce = w3.eth.getTransactionCount(account.address)
    txn_dict = contract.functions.createJob(title, price).buildTransaction({
        'gas': 2000000,
        'gasPrice': w3.toWei('40', 'gwei'),
        'nonce': nonce,
    })
    signed_txn = w3.eth.account.signTransaction(txn_dict, private_key=account.privateKey)
    txn_receipt = w3.eth.sendRawTransaction(signed_txn.rawTransaction)
    print(f'Job created, transaction hash: {txn_receipt.hex()}')

# Ensure you replace 'YOUR_INFURA_PROJECT_ID', 'YOUR_CONTRACT_ADDRESS', and 'YOUR_PRIVATE_KEY' with your actual project details.

# Example usage
if __name__ == '__main__':
    create_job('Logo Design', 1000000000000000000)  # 1 ETH for simplicity
