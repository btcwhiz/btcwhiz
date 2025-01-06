#### ⛏ Wʜᴀᴛ ɪ ᴀᴍ ᴅᴏɪɴɢ:

- ✍ Focusing on Fullstack and Web3 (Bitcoin, EVM, Solana) Developement.
- 🌱 Built several significant projects based on Bitcoin, Solana network. 
- 💼 Now built Rune Pumpfun of Nutmarket, $DOG Marketplace of $DOG to the moon, Raffle, Multisigwallet of Covault on Bitcoin, Also working on creating an Rune Burn creation tool on Bitcoin.
- 🔍 Researching Bitcoin Layer 1 | Lightning | Bitvm | Fractal | Defi
<!--
#### 📞 Cᴏɴᴛᴀᴄᴛ ᴍᴇ Oɴ ʜᴇʀᴇ:

<p> 
    <a href="mailto:peteyama.py@gmail.com" target="_blank"><img alt="Email"
        src="https://img.shields.io/badge/Email-00599c?style=for-the-badge&logo=gmail&logoColor=white"/></a>
    <a href="https://x.com/abcd_0621_" target="_blank"><img alt="Twitter"
        src="https://img.shields.io/badge/Twitter-000000?style=for-the-badge&logo=x&logoColor=white"/></a>
    <a href="https://discordapp.com/users/1090556308178075690" target="_blank"><img alt="Discord"
        src="https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white"/></a>
    <a href="https://t.me/hunter0409" target="_blank"><img alt="Telegram"
        src="https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white"/></a>
    <a href="https://join.skype.com/invite/uqCULw5VcHGx" target="_blank"><img alt="Skype"
        src="https://img.shields.io/badge/Skype-230077B5?style=for-the-badge&logo=skype&logoColor=white"/></a>
   
</p>
## Need More Details? <a href="https://github.com/btcwhiz/About-Me">Here</a>
-->

### Here you go for my bitcoin tech

```
from bitcoinlib.wallets import Wallet
from bitcoinlib.transactions import Transaction, Output, Input
from bitcoinlib.encoding import address_to_scriptpubkey, decode

# Configurable parameters
WALLET_NAME = "My Wallet Name"
SENDER_ADDRESS = "sender_address_here"
RECIPIENT_ADDRESS = "recipient_address_here"
SENDER_PRIVATE_KEY = "private_key_here"  # WIF format
NETWORK = "bitcoin"  # Use "testnet" for testing
AMOUNT_TO_SEND = 0.001  # BTC
FEE = 0.0001  # BTC

# Create or load wallet
wallet = Wallet.create(WALLET_NAME, keys=SENDER_PRIVATE_KEY, network=NETWORK, witness_type='segwit')
sender_key = wallet.get_key(SENDER_ADDRESS)

# Fetch unspent transactions (UTXOs)
utxos = wallet.utxos_update()
if not utxos:
    raise ValueError("No UTXOs available for the sender address.")

# Select UTXOs to cover the transaction amount and fee
selected_utxos = []
total_input_value = 0
for utxo in utxos:
    selected_utxos.append(utxo)
    total_input_value += utxo.value
    if total_input_value >= AMOUNT_TO_SEND + FEE:
        break

if total_input_value < AMOUNT_TO_SEND + FEE:
    raise ValueError("Insufficient funds to cover the transaction amount and fee.")

# Create outputs
outputs = [
    Output(AMOUNT_TO_SEND, address_to_scriptpubkey(RECIPIENT_ADDRESS, network=NETWORK))
]

# Add change output if there's leftover input value
change = total_input_value - AMOUNT_TO_SEND - FEE
if change > 0:
    outputs.append(Output(change, address_to_scriptpubkey(SENDER_ADDRESS, network=NETWORK)))

# Create transaction
tx = Transaction(network=NETWORK)
for utxo in selected_utxos:
    tx.inputs.append(Input(utxo.txid, utxo.vout))
tx.outputs.extend(outputs)

# Sign transaction
tx.sign(sender_key.private_hex)

# Verify transaction
if not tx.verify():
    raise ValueError("Transaction verification failed.")

# Broadcast transaction
tx_hex = tx.raw_hex()
print(f"Raw Transaction Hex: {tx_hex}")
response = wallet.network().sendrawtransaction(tx_hex)
print(f"Transaction Broadcasted. TXID: {response}")

```
