**THIS CHECKLIST IS NOT COMPLETE**. Use `--show-ignored-findings` to show all the results.
Summary
 - [unchecked-transfer](#unchecked-transfer) (8 results) (High)
 - [divide-before-multiply](#divide-before-multiply) (2 results) (Medium)
 - [incorrect-equality](#incorrect-equality) (1 results) (Medium)
 - [reentrancy-no-eth](#reentrancy-no-eth) (3 results) (Medium)
 - [events-maths](#events-maths) (1 results) (Low)
 - [reentrancy-benign](#reentrancy-benign) (2 results) (Low)
 - [reentrancy-events](#reentrancy-events) (2 results) (Low)
 - [pragma](#pragma) (1 results) (Informational)
 - [dead-code](#dead-code) (1 results) (Informational)
 - [solc-version](#solc-version) (6 results) (Informational)
 - [naming-convention](#naming-convention) (4 results) (Informational)
 - [similar-names](#similar-names) (5 results) (Informational)
 - [immutable-states](#immutable-states) (2 results) (Optimization)
## unchecked-transfer
Impact: High
Confidence: Medium
 - [ ] ID-0
[Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45) ignores return value by [X.transferFrom(msg.sender,address(this),tokenXAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L32)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45


 - [ ] ID-1
[Dex.removeLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78) ignores return value by [Y.transfer(msg.sender,_ty)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L76)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78


 - [ ] ID-2
[Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64) ignores return value by [Y.transferFrom(msg.sender,address(this),tokenYAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L60)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


 - [ ] ID-3
[Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45) ignores return value by [Y.transferFrom(msg.sender,address(this),tokenYAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L42)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45


 - [ ] ID-4
[Dex.removeLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78) ignores return value by [X.transfer(msg.sender,_tx)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L75)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78


 - [ ] ID-5
[Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45) ignores return value by [Y.transfer(msg.sender,outputAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L33)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45


 - [ ] ID-6
[Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64) ignores return value by [X.transferFrom(msg.sender,address(this),tokenXAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L58)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


 - [ ] ID-7
[Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45) ignores return value by [X.transfer(msg.sender,outputAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L43)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45


## divide-before-multiply
Impact: Medium
Confidence: Medium
 - [ ] ID-8
[Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45) performs a multiplication on the result of a division:
	- [x_value = tokenXAmount / 1000 * 999](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L26)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45


 - [ ] ID-9
[Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45) performs a multiplication on the result of a division:
	- [y_value = tokenYAmount / 1000 * 999](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L36)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45


## incorrect-equality
Impact: Medium
Confidence: High
 - [ ] ID-10
[Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64) uses a dangerous strict equality:
	- [totalSupply() == 0](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L53)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


## reentrancy-no-eth
Impact: Medium
Confidence: Medium
 - [ ] ID-11
Reentrancy in [Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64):
	External calls:
	- [X.transferFrom(msg.sender,address(this),tokenXAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L58)
	- [Y.transferFrom(msg.sender,address(this),tokenYAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L60)
	State variables written after the call(s):
	- [_mint(msg.sender,LPTokenAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L63)
		- [_totalSupply += amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L264)
	[ERC20._totalSupply](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L40) can be used in cross function reentrancies:
	- [ERC20._burn(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L285-L301)
	- [ERC20._mint(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L259-L272)
	- [ERC20.totalSupply()](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L94-L96)
	- [amountY = reserveY + tokenYAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L61)
	[Dex.amountY](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L8) can be used in cross function reentrancies:
	- [Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64)
	- [Dex.amountY](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L8)
	- [Dex.amount_update()](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L80-L85)
	- [Dex.removeLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78)
	- [Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


 - [ ] ID-12
Reentrancy in [Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64):
	External calls:
	- [X.transferFrom(msg.sender,address(this),tokenXAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L58)
	State variables written after the call(s):
	- [amountX = reserveX + tokenXAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L59)
	[Dex.amountX](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L7) can be used in cross function reentrancies:
	- [Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64)
	- [Dex.amountX](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L7)
	- [Dex.amount_update()](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L80-L85)
	- [Dex.removeLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78)
	- [Dex.swap(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18-L45)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


 - [ ] ID-13
Reentrancy in [Dex.removeLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78):
	External calls:
	- [X.transfer(msg.sender,_tx)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L75)
	- [Y.transfer(msg.sender,_ty)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L76)
	State variables written after the call(s):
	- [_burn(msg.sender,LPTokenAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L77)
		- [_totalSupply -= amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L295)
	[ERC20._totalSupply](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L40) can be used in cross function reentrancies:
	- [ERC20._burn(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L285-L301)
	- [ERC20._mint(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L259-L272)
	- [ERC20.totalSupply()](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L94-L96)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78


## events-maths
Impact: Low
Confidence: Medium
 - [ ] ID-14
[Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64) should emit an event for: 
	- [amountX = reserveX + tokenXAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L59) 
	- [amountY = reserveY + tokenYAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L61) 

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


## reentrancy-benign
Impact: Low
Confidence: Medium
 - [ ] ID-15
Reentrancy in [Dex.removeLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78):
	External calls:
	- [X.transfer(msg.sender,_tx)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L75)
	- [Y.transfer(msg.sender,_ty)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L76)
	State variables written after the call(s):
	- [_burn(msg.sender,LPTokenAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L77)
		- [_balances[account] = accountBalance - amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L293)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78


 - [ ] ID-16
Reentrancy in [Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64):
	External calls:
	- [X.transferFrom(msg.sender,address(this),tokenXAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L58)
	- [Y.transferFrom(msg.sender,address(this),tokenYAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L60)
	State variables written after the call(s):
	- [_mint(msg.sender,LPTokenAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L63)
		- [_balances[account] += amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L267)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


## reentrancy-events
Impact: Low
Confidence: Medium
 - [ ] ID-17
Reentrancy in [Dex.addLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64):
	External calls:
	- [X.transferFrom(msg.sender,address(this),tokenXAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L58)
	- [Y.transferFrom(msg.sender,address(this),tokenYAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L60)
	Event emitted after the call(s):
	- [Transfer(address(0),account,amount)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L269)
		- [_mint(msg.sender,LPTokenAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L63)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47-L64


 - [ ] ID-18
Reentrancy in [Dex.removeLiquidity(uint256,uint256,uint256)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78):
	External calls:
	- [X.transfer(msg.sender,_tx)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L75)
	- [Y.transfer(msg.sender,_ty)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L76)
	Event emitted after the call(s):
	- [Transfer(account,address(0),amount)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L298)
		- [_burn(msg.sender,LPTokenAmount)](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L77)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66-L78


## pragma
Impact: Informational
Confidence: High
 - [ ] ID-19
Different versions of Solidity are used:
	- Version used: ['^0.8.0', '^0.8.13']
	- [^0.8.0](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4)
	- [^0.8.0](node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4)
	- [^0.8.0](node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4)
	- [^0.8.0](node_modules/@openzeppelin/contracts/utils/Context.sol#L4)
	- [^0.8.13](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L2)

node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4


## dead-code
Impact: Informational
Confidence: Medium
 - [ ] ID-20
[Context._msgData()](node_modules/@openzeppelin/contracts/utils/Context.sol#L21-L23) is never used and should be removed

node_modules/@openzeppelin/contracts/utils/Context.sol#L21-L23


## solc-version
Impact: Informational
Confidence: High
 - [ ] ID-21
Pragma version[^0.8.0](node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4) allows old versions

node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4


 - [ ] ID-22
Pragma version[^0.8.0](node_modules/@openzeppelin/contracts/utils/Context.sol#L4) allows old versions

node_modules/@openzeppelin/contracts/utils/Context.sol#L4


 - [ ] ID-23
Pragma version[^0.8.0](node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4) allows old versions

node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4


 - [ ] ID-24
Pragma version[^0.8.0](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4) allows old versions

node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4


 - [ ] ID-25
solc-0.8.13 is not recommended for deployment

 - [ ] ID-26
Pragma version[^0.8.13](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L2) allows old versions

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L2


## naming-convention
Impact: Informational
Confidence: High
 - [ ] ID-27
Variable [Dex.X](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L10) is not in mixedCase

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L10


 - [ ] ID-28
Function [Dex.amount_update()](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L80-L85) is not in mixedCase

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L80-L85


 - [ ] ID-29
Variable [Dex.Y](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L11) is not in mixedCase

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L11


 - [ ] ID-30
Parameter [Dex.removeLiquidity(uint256,uint256,uint256).LPTokenAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66) is not in mixedCase

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66


## similar-names
Impact: Informational
Confidence: Medium
 - [ ] ID-31
Variable [Dex.swap(uint256,uint256,uint256).tokenXAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18) is too similar to [Dex.swap(uint256,uint256,uint256).tokenYAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18


 - [ ] ID-32
Variable [Dex.swap(uint256,uint256,uint256).tokenXAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18) is too similar to [Dex.addLiquidity(uint256,uint256,uint256).tokenYAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18


 - [ ] ID-33
Variable [Dex.addLiquidity(uint256,uint256,uint256).tokenXAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47) is too similar to [Dex.addLiquidity(uint256,uint256,uint256).tokenYAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47


 - [ ] ID-34
Variable [Dex.addLiquidity(uint256,uint256,uint256).tokenXAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47) is too similar to [Dex.swap(uint256,uint256,uint256).tokenYAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L18)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L47


 - [ ] ID-35
Variable [Dex.removeLiquidity(uint256,uint256,uint256).minimumTokenXAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66) is too similar to [Dex.removeLiquidity(uint256,uint256,uint256).minimumTokenYAmount](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66)

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L66


## immutable-states
Impact: Optimization
Confidence: High
 - [ ] ID-36
[Dex.Y](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L11) should be immutable 

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L11


 - [ ] ID-37
[Dex.X](audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L10) should be immutable 

audit_Dex_Lending/dex/choiyounghyeon/src/Dex.sol#L10


