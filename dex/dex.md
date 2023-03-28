# imbalance한 유동성 공급 방식의 문제점
# 최영현

### 설명 
토큰의 비율을 고려하지 않아, 유동성을 공급 할 때 공급하는 tokenX와 tokenY에서 tokenY의 값이 작아더라도 tokenX의 비율로 LP 토큰을 발행하기 때문에, 이를 악용해 이득을 얻을 수 있다.



```
function  addLiquidity(uint256 tokenXAmount, uint256 tokenYAmount, uint256 minimumLPTokenAmount) external  returns (uint LPTokenAmount){

require(tokenXAmount > 0 && tokenYAmount > 0);

(uint256 reserveX, ) = amount_update(); 

(, uint256 reserveY) = amount_update();  

if(totalSupply() == 0){ LPTokenAmount = tokenXAmount * tokenYAmount / 10**18;}
else{ LPTokenAmount = totalSupply() * tokenXAmount / reserveX;}
require(minimumLPTokenAmount <= LPTokenAmount);
X.transferFrom(msg.sender, address(this), tokenXAmount);
amountX = reserveX + tokenXAmount;

Y.transferFrom(msg.sender, address(this), tokenYAmount);

amountY = reserveY + tokenYAmount;

_mint(msg.sender, LPTokenAmount);

}
```

```
function  testAddLiquidity12() external {
uint firstLPReturn = dex.addLiquidity(1000  ether, 1000  ether, 0);
emit  log_named_uint("firstLPReturn", firstLPReturn);
uint secondLPReturn = dex.addLiquidity(1000  ether, 1, 0);
emit  log_named_uint("secondLPReturn", secondLPReturn);
(uint  tx, uint ty) = dex.removeLiquidity(1000  ether, 0, 0);
emit  log_named_uint("tx", tx);
emit  log_named_uint("ty", ty);
}
[PASS] testAddLiquidity12() (gas: 215748)
Logs:
  firstLPReturn: 1000000000000000000000000
  secondLPReturn: 1000000000000000000000000
  tx: 1000000000000000000
  ty: 500000000000000000
```

### 파급력 
Critical한 취약점

풀에 있는 토큰의 가격을 조작하여 1:1 비율에서 벗어나게 한 다음 가격 불균형을 악용하여 이익을 얻을 수 있다. 그로 인해 전반적인 유동성 풀 자체가 깨지게 되기 떄문이다.

```

```

### 해결방안
토큰의 비율을 검사하는 로직을 추가해줘야한다.

# RemoveLiquidity 검증 부재
### 설명 

검증부재로 인해 가지고 있는 토큰 보다 더 많은 액수의 돈을 찾아올 수 있다.

```
function  removeLiquidity(uint256 LPTokenAmount, uint256 minimumTokenXAmount, uint256 minimumTokenYAmount) external  returns (uint _tx, uint _ty){
amount_update();  
_tx = amountX * LPTokenAmount / totalSupply();
_ty = amountY * LPTokenAmount / totalSupply();
require(_tx>=minimumTokenXAmount);
require(_ty>=minimumTokenYAmount);
X.transfer(msg.sender, _tx);
Y.transfer(msg.sender, _ty);
_burn(msg.sender, LPTokenAmount);
}
```
### POC
```
function  testRemoveLiquidity1() external {
uint firstLPReturn = dex.addLiquidity(1000  ether, 1000  ether, 0);
emit  log_named_uint("firstLPReturn", firstLPReturn);
uint secondLPReturn = dex.addLiquidity(1, 1, 0);
emit  log_named_uint("secondLPReturn", secondLPReturn);
(uint  tx, uint ty) = dex.removeLiquidity(5000  ether, 0, 0);
emit  log_named_uint("tx", tx);
emit  log_named_uint("ty", ty);
}
```
### 파급력 
Critical한 취약점

해커가 임의로 유동성 풀에서 소정의 돈을 넣고, 추후 넣은 액수보다 더 많은 양의 토큰을 인출할 수 있기 떄문에 LP들만 무한정으로 손해를 볼 수 밖에 없기 떄문이다.

```

```

### 해결방안
인출하려고 하는 액수를 검증하는 로직이 필요하다. 


# 유동성 풀에 적은 금액을 추가할 시 LP Token을 받지 못함
### 설명 

유동성 풀에 적은 금액의 이더를 보내게 될 경우, LPToken으로 받을 수 있는 개수가 0이 되는 문제점이 있다.

```
function  testAddLiquidity1() external {
uint firstLPReturn = dex.addLiquidity(10**8,10**8,0);
emit  log_named_uint("firstLPReturn", firstLPReturn);
uint secondLPReturn = dex.addLiquidity(1000  ether, 1000  ether, 0);
emit  log_named_uint("secondLPReturn", secondLPReturn);

}
```



### 파급력 
informal

실제 엄청난 파급효과가 있는 것은 아니지만, 사용자 입장에서는 적은 금액이 없어져버려 LPToken을 받지 못하기 떄문이다.

```

```

### 해결방안
유동성 풀에서 입급가능한 최소 수량을 확인하고 이에 맞지 않을 경우 revert를 반환하게 변경하면 될것 같다.

# Dos 공격이 가능
### 설명 

공격자는 매우 적은 양의 토큰으로 `addLiquidity` 함수를 반복적으로 호출하여 이를 악용할 수 있으며, 이로 인해 컨트랙트에서 비용이 많이 드는 계산을 많이 수행하고 결국 가스가 고갈되게되서 서비스 이용을 못하게 할 수 있다.

```

```


### 파급력 
informal

해당 공격을 실제로 수행하려면, 가스비가 그만큼 많이 들기 때문에 실제 Dos 공격을 수행하기가 어렵다고 판단했기 떄문이다.

```

```

### 해결방안
require(tokenXAmount >= 1 ether && tokenYAmount >= 1 ether); 형식을 AddLiquidity에 넣어서 최소값을 정해주거나, 혹은  max gas limit로 제한을 둬야한다.


# transfer public 설정 
# 구민재,김지우, 김영운

### 설명 
외부에서 아무나 해당 함수를 이용해 민팅을 할 수 있게 되어, LPToken을 발행할 수 있게 되었다.


```
 function transfer(address to, uint256 lpAmount) public override(ERC20, IDex) returns (bool) {
        _mint(to, lpAmount);
        return true;
    }
      
```

### 파급력
Critical
파급력이 엄청 크고, 아무나 해당 함수를 호출해서 사용해 유동성풀을 헤칠 수 있기 때문이다.

### 해결방안
transfer 함수에 modifier를 적용해, 해당 사용자만 이용할 수 있도록 액세스를 설정

# removeLiquidity에서의 LPToken의 유동성을 제거 하지 않음 
# 김한기

### 설명 
removeLiquidity에서 


```
 require(LPTokenAmount > 0);
        require(minimumTokenXAmount >= 0);
        require(minimumTokenYAmount >= 0);
        require(lpt.balanceOf(msg.sender) >= LPTokenAmount);

        (uint balanceOfX, uint balanceOfY) = pairTokenBalance();

        uint lptTotalSupply = lpt.totalSupply();

        rx = balanceOfX * LPTokenAmount / lptTotalSupply;
        ry = balanceOfY * LPTokenAmount / lptTotalSupply;

        require(rx >= minimumTokenXAmount);
        require(rx >= minimumTokenYAmount);
      
```

### 파급력
Critical
해당 함수를 실행해서, LP들이 공급한 유동성을 회수해 갈려고하지만 회수할 수 없게 되기 떄문에 큰 문제를 불러올 수 있다고 판단했다.


### 해결방안
실제 LPToken을 bun하는 기능과 transferFrom으로 rx,ry값을 transferFrom으로 사용자에게 반환해줘야 한다.



# imbalance한 유동성 공급 방식의 문제점 
# 황준태

### 설명 
토큰의 비율을 고려하지 않아, 유동성을 공급 할 때 공급하는 tokenX와 tokenY에서 tokenY의 값이 작아더라도 tokenX의 비율로 LP 토큰을 발행하기 때문에, 이를 악용해 이득을 얻을 수 있다.



 ```
 if(totalSupply_ ==0 ){ //if first supply 
        LPTokenAmount = _sqrt(tokenXAmount*tokenYAmount);
    }
    else{// calculate over the before
        liqX = _mul(tokenXAmount ,totalSupply_)/amountX;
        liqY = _mul(tokenYAmount ,totalSupply_)/amountY;
        LPTokenAmount = (liqX<liqY) ? liqX:liqY; 
    }
    require(LPTokenAmount >= minimumLPTokenAmount, "Less LP Token Supply");
    transfer_(msg.sender,LPTokenAmount);
    totalSupply_ += LPTokenAmount;
    amountX += tokenXAmount;
    amountY += tokenYAmount;
    tokenX.transferFrom(msg.sender, address(this), tokenXAmount);
    tokenY.transferFrom(msg.sender, address(this), tokenYAmount);
```

```
function  testAddLiquidity12() external {
uint firstLPReturn = dex.addLiquidity(1000  ether, 1000  ether, 0);
emit  log_named_uint("firstLPReturn", firstLPReturn);
uint secondLPReturn = dex.addLiquidity(1000  ether, 1, 0);
emit  log_named_uint("secondLPReturn", secondLPReturn);
(uint  tx, uint ty) = dex.removeLiquidity(1000  ether, 0, 0);
emit  log_named_uint("tx", tx);
emit  log_named_uint("ty", ty);
}
[PASS] testAddLiquidity12() (gas: 215748)
Logs:
  firstLPReturn: 1000000000000000000000000
  secondLPReturn: 1000000000000000000000000
  tx: 1000000000000000000
  ty: 500000000000000000
```


### 파급력 
Critical한 취약점

풀에 있는 토큰의 가격을 조작하여 1:1 비율에서 벗어나게 한 다음 가격 불균형을 악용하여 이익을 얻을 수 있다. 그로 인해 전반적인 유동성 풀 자체가 깨지게 되기 떄문이다.


### 해결방안
실제 LPToken을 bun하는 기능과 transferFrom으로 rx,ry값을 transferFrom으로 사용자에게 반환해줘야 한다.



# imbalance한 유동성 공급 방식의 문제점 
# 임나라

### 설명 
토큰의 비율을 고려하지 않아, 유동성을 공급 할 때 공급하는 tokenX와 tokenY에서 tokenY의 값이 작아더라도 tokenX의 비율로 LP 토큰을 발행하기 때문에, 이를 악용해 이득을 얻을 수 있다.




 ```
 if(totalSupply_ ==0 ){ //if first supply 
        LPTokenAmount = _sqrt(tokenXAmount*tokenYAmount);
    }
    else{// calculate over the before
        liqX = _mul(tokenXAmount ,totalSupply_)/amountX;
        liqY = _mul(tokenYAmount ,totalSupply_)/amountY;
        LPTokenAmount = (liqX<liqY) ? liqX:liqY; 
    }
    require(LPTokenAmount >= minimumLPTokenAmount, "Less LP Token Supply");
    transfer_(msg.sender,LPTokenAmount);
    totalSupply_ += LPTokenAmount;
    amountX += tokenXAmount;
    amountY += tokenYAmount;
    tokenX.transferFrom(msg.sender, address(this), tokenXAmount);
    tokenY.transferFrom(msg.sender, address(this), tokenYAmount);
```

```
function  testAddLiquidity12() external {
uint firstLPReturn = dex.addLiquidity(1000  ether, 1000  ether, 0);
emit  log_named_uint("firstLPReturn", firstLPReturn);
uint secondLPReturn = dex.addLiquidity(1000  ether, 1, 0);
emit  log_named_uint("secondLPReturn", secondLPReturn);
(uint  tx, uint ty) = dex.removeLiquidity(1000  ether, 0, 0);
emit  log_named_uint("tx", tx);
emit  log_named_uint("ty", ty);
}
[PASS] testAddLiquidity12() (gas: 215748)
Logs:
  firstLPReturn: 1000000000000000000000000
  secondLPReturn: 1000000000000000000000000
  tx: 1000000000000000000
  ty: 500000000000000000
```

### 파급력 
Critical한 취약점

풀에 있는 토큰의 가격을 조작하여 1:1 비율에서 벗어나게 한 다음 가격 불균형을 악용하여 이익을 얻을 수 있다. 그로 인해 전반적인 유동성 풀 자체가 깨지게 되기 떄문이다.

### 해결방안
토큰의 비율을 검사하는 로직을 추가해줘야한다.


# imbalance한 유동성 공급 방식의 문제점 
# 서준원

### 설명 
토큰의 비율을 고려하지 않아, 유동성을 공급 할 때 공급하는 tokenX와 tokenY에서 tokenY의 값이 작아더라도 tokenX의 비율로 LP 토큰을 발행하기 때문에, 이를 악용해 이득을 얻을 수 있다.




 ```
       uint token_amount;
        if(totalSupply() == 0){
            token_liquidity_L = sqrt(reserve_x * reserve_y);
            token_amount = sqrt((tokenXAmount * tokenYAmount));
        } else{
            token_amount = (tokenXAmount * 10 ** 18 * totalSupply() / reserve_x) / 10 ** 18;
        }
        require(token_amount > minimumLPTokenAmount, "token_amount > minimumLPTokenAmount");

        require(ERC20(token_x).transferFrom(msg.sender, address(this), tokenXAmount));
        require(ERC20(token_y).transferFrom(msg.sender, address(this), tokenYAmount));
        _mint(msg.sender, token_amount);

```

```
function  testAddLiquidity12() external {
uint firstLPReturn = dex.addLiquidity(1000  ether, 1000  ether, 0);
emit  log_named_uint("firstLPReturn", firstLPReturn);
uint secondLPReturn = dex.addLiquidity(1000  ether, 1, 0);
emit  log_named_uint("secondLPReturn", secondLPReturn);
(uint  tx, uint ty) = dex.removeLiquidity(1000  ether, 0, 0);
emit  log_named_uint("tx", tx);
emit  log_named_uint("ty", ty);
}
[PASS] testAddLiquidity12() (gas: 215748)
Logs:
  firstLPReturn: 1000000000000000000000000
  secondLPReturn: 1000000000000000000000000
  tx: 1000000000000000000
  ty: 500000000000000000
```

### 파급력 
Critical한 취약점

풀에 있는 토큰의 가격을 조작하여 1:1 비율에서 벗어나게 한 다음 가격 불균형을 악용하여 이익을 얻을 수 있다. 그로 인해 전반적인 유동성 풀 자체가 깨지게 되기 떄문이다.

### 해결방안
토큰의 비율을 검사하는 로직을 추가해줘야한다.

