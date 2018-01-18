### Elementrem ERC 20 Token (Coin) contract

You can create a new Token (Coin) using the `Elementrem platform.`            
It is very easy and fast to deploy, and above all, you can use your Coin in a wide variety of areas. 
All of these work in the Elementrem platform.

It is called Token (Coin) for you to explain easily, but basically it deals with a `numbers` which has a meaning and purpose.
It can be used for `any fungible tradable good` very simply, and furthermore it can be used for `coins, loyalty points, gold certificates, IOUs, in game items, state alt money, etc.`, as well as replacing existing `national currency with ELE token, Enterprise-internal token`.    
All of these can be achieved using the decentralization of the Elementrem ecosystem to achieve high reliability, safety and security.

***
- Deploying a simple Token(Coin) contract Lab - Step by Step
***

**1. Run the Elementrem Mist Wallet, click on `Contracts Tab`, and go to `deploy new contract.`**
![](img/1.png)
You do not have to write `Element Amount`. All contracts require only gas(commission).     
      
------------------------------      
      
**2. Write the `solidity contract source code.`**     
Copy and paste the following code very simply.
```
pragma solidity ^0.4.17;

interface tokenRecipient { function receiveApproval(address _from, uint256 _value, address _token, bytes _extraData) public; }

contract TokenERC20 {
    // Public variables of the token
    string public name;
    string public symbol;
    uint8 public decimals = 18;
    // 18 decimals is the strongly suggested default, avoid changing it
    uint256 public totalSupply;

    // This creates an array with all balances
    mapping (address => uint256) public balanceOf;
    mapping (address => mapping (address => uint256)) public allowance;

    // This generates a public event on the blockchain that will notify clients
    event Transfer(address indexed from, address indexed to, uint256 value);

    // This notifies clients about the amount burnt
    event Burn(address indexed from, uint256 value);

    /**
     * Constrctor function
     *
     * Initializes contract with initial supply tokens to the creator of the contract
     */
    function TokenERC20(
        uint256 initialSupply,
        string tokenName,
        string tokenSymbol
    ) public {
        totalSupply = initialSupply * 10 ** uint256(decimals);  // Update total supply with the decimal amount
        balanceOf[msg.sender] = totalSupply;                // Give the creator all initial tokens
        name = tokenName;                                   // Set the name for display purposes
        symbol = tokenSymbol;                               // Set the symbol for display purposes
    }

    /**
     * Internal transfer, only can be called by this contract
     */
    function _transfer(address _from, address _to, uint _value) internal {
        // Prevent transfer to 0x0 address. Use burn() instead
        require(_to != 0x0);
        // Check if the sender has enough
        require(balanceOf[_from] >= _value);
        // Check for overflows
        require(balanceOf[_to] + _value > balanceOf[_to]);
        // Save this for an assertion in the future
        uint previousBalances = balanceOf[_from] + balanceOf[_to];
        // Subtract from the sender
        balanceOf[_from] -= _value;
        // Add the same to the recipient
        balanceOf[_to] += _value;
        Transfer(_from, _to, _value);
        // Asserts are used to use static analysis to find bugs in your code. They should never fail
        assert(balanceOf[_from] + balanceOf[_to] == previousBalances);
    }

    /**
     * Transfer tokens
     *
     * Send `_value` tokens to `_to` from your account
     *
     * @param _to The address of the recipient
     * @param _value the amount to send
     */
    function transfer(address _to, uint256 _value) public {
        _transfer(msg.sender, _to, _value);
    }

    /**
     * Transfer tokens from other address
     *
     * Send `_value` tokens to `_to` on behalf of `_from`
     *
     * @param _from The address of the sender
     * @param _to The address of the recipient
     * @param _value the amount to send
     */
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= allowance[_from][msg.sender]);     // Check allowance
        allowance[_from][msg.sender] -= _value;
        _transfer(_from, _to, _value);
        return true;
    }

    /**
     * Set allowance for other address
     *
     * Allows `_spender` to spend no more than `_value` tokens on your behalf
     *
     * @param _spender The address authorized to spend
     * @param _value the max amount they can spend
     */
    function approve(address _spender, uint256 _value) public
        returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        return true;
    }

    /**
     * Set allowance for other address and notify
     *
     * Allows `_spender` to spend no more than `_value` tokens on your behalf, and then ping the contract about it
     *
     * @param _spender The address authorized to spend
     * @param _value the max amount they can spend
     * @param _extraData some extra information to send to the approved contract
     */
    function approveAndCall(address _spender, uint256 _value, bytes _extraData)
        public
        returns (bool success) {
        tokenRecipient spender = tokenRecipient(_spender);
        if (approve(_spender, _value)) {
            spender.receiveApproval(msg.sender, _value, this, _extraData);
            return true;
        }
    }

    /**
     * Destroy tokens
     *
     * Remove `_value` tokens from the system irreversibly
     *
     * @param _value the amount of money to burn
     */
    function burn(uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value);   // Check if the sender has enough
        balanceOf[msg.sender] -= _value;            // Subtract from the sender
        totalSupply -= _value;                      // Updates totalSupply
        Burn(msg.sender, _value);
        return true;
    }

    /**
     * Destroy tokens from other account
     *
     * Remove `_value` tokens from the system irreversibly on behalf of `_from`.
     *
     * @param _from the address of the sender
     * @param _value the amount of money to burn
     */
    function burnFrom(address _from, uint256 _value) public returns (bool success) {
        require(balanceOf[_from] >= _value);                // Check if the targeted balance is enough
        require(_value <= allowance[_from][msg.sender]);    // Check allowance
        balanceOf[_from] -= _value;                         // Subtract from the targeted balance
        allowance[_from][msg.sender] -= _value;             // Subtract from the sender's allowance
        totalSupply -= _value;                              // Update totalSupply
        Burn(_from, _value);
        return true;
    }
}

```       
      
------------------------------      
      
**3. Specify the parameters.**      
On the right column you'll see all the parameters you need to personalize your own token. You can tweak them as you please.     
Scroll to the end of the page and you'll see an estimate of the computation cost of that contract and you can select a fee on how much ether you are willing to pay for it. Any excess ether you don't spend will be returned to you so you can leave the default settings if you wish. Press "deploy", type your account password and wait a few seconds for your transaction to be picked up.

![](img/ERC%2020%20contract%201.png)      
      
------------------------------      
      


**4. Deploy Contract.**     
![](img/ERC%2020%20contract%202.png)      
      
------------------------------      
      

**5. Waiting for confirmation**     
![](img/4.png)
You'll be redirected to the front page where you can see your transaction waiting for confirmations. Click the account named `Element base` and after no more than a minute you should see that your account will show that you have 100% of the shares you just created.     
![](img/5.png)    
      
------------------------------      
      

**6. Identify the address of the Token Contract.**      
Now go to the `Contracts tab` and Click on Token created in `CUSTOM TOKENS` to see Token information.     
![](img/6.png)      

Since this is a very simple Token informatio page there isn't much to do here, just copy `contract address`.
This address exists forever in the Elementrem ecosystem, which allows other users to add the Token to their wallet.     
      
------------------------------      
      

**7. Add the generated Token to another user's wallet.**  
Other users will not see anything in their wallet yet. This is because the wallet only tracks tokens it knows about, and you have to add these manually. 
![](img/7.png)    
Now go to the `Contracts tab` and Click on `Watch token` in the` CUSTOM TOKENS` menu.

To add a token to watch, go to the contracts page and then click "Watch Token". A pop-up will appear and you only need to paste the contract address.   
![](img/8.png)    

From now on, other users can use tokens.
![](img/9.png)    
      
------------------------------      
      


***Congratulations! Both the user who created the token and the user who added the token can see a menu that allows them to select and send tokens.***      
![](img/10.png)   
![](img/11.png) 


* Tracking of the Token will be activated in the new BlockExplorer.
