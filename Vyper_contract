# @version ^0.3.7

# Token metadata
name: public(String[32])  # Token name
symbol: public(String[8])  # Token symbol
decimals: public(uint256)  # Number of decimals
totalSupply: public(uint256)  # Total token supply

# Token balances mapping
balanceOf: public(HashMap[address, uint256])

# Allowances mapping for approve/transferFrom functionality
allowance: public(HashMap[address, HashMap[address, uint256]])

# Event for transfer
event Transfer:
    sender: indexed(address)
    receiver: indexed(address)
    value: uint256

# Event for approval
event Approval:
    owner: indexed(address)
    spender: indexed(address)
    value: uint256

# Constructor to set initial supply, name, symbol, and decimals
@external
def __init__():
    self.name = "PBTC"
    self.symbol = "PBTC"
    self.decimals = 18
    self.totalSupply = 21_000_000_000 * 10**18  # 21 billion tokens with 18 decimals
    self.balanceOf[msg.sender] = self.totalSupply  # Assign total supply to contract creator

# Transfer function
@external
def transfer(_to: address, _value: uint256) -> bool:
    assert self.balanceOf[msg.sender] >= _value, "Insufficient balance"
    self.balanceOf[msg.sender] -= _value
    self.balanceOf[_to] += _value
    log Transfer(msg.sender, _to, _value)
    return True

# Approve function to allow spending on behalf of the token holder
@external
def approve(_spender: address, _value: uint256) -> bool:
    self.allowance[msg.sender][_spender] = _value
    log Approval(msg.sender, _spender, _value)
    return True

# TransferFrom function for approved spenders
@external
def transferFrom(_from: address, _to: address, _value: uint256) -> bool:
    assert self.balanceOf[_from] >= _value, "Insufficient balance"
    assert self.allowance[_from][msg.sender] >= _value, "Allowance exceeded"
    self.balanceOf[_from] -= _value
    self.balanceOf[_to] += _value
    self.allowance[_from][msg.sender] -= _value
    log Transfer(_from, _to, _value)
    return True

# Payable function to accept Ether
@payable
@external
def deposit():
    # Accepts Ether deposits, nothing else needed
    pass

# Function to withdraw Ether from contract
@external
def withdraw(_amount: uint256):
    assert _amount <= self.balanceOf[msg.sender], "Insufficient token balance to withdraw"
    send(msg.sender, _amount)

# Function to receive Ether directly to the contract (fallback)
@payable
@external
def __default__():
    pass