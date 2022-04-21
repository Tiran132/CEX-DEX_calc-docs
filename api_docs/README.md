# **Calc API documintation**
# 
# **Endpoints**: 
 

- /[tokens1](##/tokens1)
- /[amount_token2](##/amount_token2)
- /[with_token1](##/with_token1)
- /[circulating_supply](##/circulating_supply)
- /[get_USD](##/get_USD)
- /[calculate](##/calculate)

## /tokens1
Возвращает все токены, по которым работает кальк

**Type**: GET

#### Response Example:
`https://marsbase.zlumer.com/calc-api/tokens1`
```
["ZKS", "USDN", "TRIBE", "DAI", ... "POWR", "QNT"]
```
**Bad result**:
```
[]
```

## /amount_token2
Возвращает предполагаемый (опираясь на курс в $) объём token2

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен А|
|token2| токен Б|
|amount| объём токена А|

#### Response Example:
`https://marsbase.zlumer.com/calc-api/amount_token2?token1=ETH&token2=USDT&amount=100`
```
315388.6703608296
```
**Bad result**:
```
0
```
## /with_token1
Возвращает все токены, с которыми есть обмен у токена А

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен А|

#### Response Example:
`https://marsbase.zlumer.com/calc-api/with_token1?token1=1INCH`
```
["USDC", "BUSD", ... "ALUSD", "CUSDC"]
```
**Bad result**:
```
["USDC", "BUSD", ... "ALUSD", "CUSDC"]  - only Stables
```
## /circulating_supply

Возвращает circulating_supply токена с CoinMaretCap

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен для которого узнаём значение|

#### Response Example:
`hhttps://marsbase.zlumer.com/calc-api/circulating_supply?token1=PAXG`
```
318453.941
```
**Bad result**: (бывает, что в cmc нет значенией circulating_supply)
```
1000000
``` 


## /get_USD
Возвращает курс токенов + их /amount_token2

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен А|
|token2| токен Б|
|amount| объём токена А|

#### Response Example:
`https://marsbase.zlumer.com/calc-api/get_USD?token1=ETH&token2=USDT&amount=100`
```
{
   "token1_USD":3150.1439255734153,
   "token2_USD":1.000234901945823,
   "exp_USD":315014.39255734155,
    "status": True
}
```
**Bad result**:
```
{
   "token1_USD":1,
   "token2_USD":1,
   "exp_USD":100,
    "status": False
}
```
## /calculate
Возвращает все расчёты на CEX и DEX

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен А|
|token2| токен Б|
|amount| объём токена А|

#### Response Example:
`https://marsbase.zlumer.com/calc-api/calculate?token1=USDT&token2=1INCH&amount=100000`
```
{
   "status_CEX":true,
   "status_DEX":true,
   "result_CEX":{
      "token1_USD":1.000234901945823,
      "token2_USD":1.5994576383806924,
      "expected_USD":100023.4901945823,
      "real_exchanged_token":61868.40690942929,
      "real_exchange_USD":98955.8960057315,
      "slippage_USD":1067.594188850795,
      "txcost_USD":98.9558960057315,
      "all_spendings_USD":1166.5500848565266,
      "spending_percent":1.0673434677933489,
      "all_spendings_percent":1.1662761243255557
   },
   "result_DEX":{
      "token1_USD":1.000234901945823,
      "token2_USD":1.5994576383806924,
      "expected_USD":100023.4901945823,
      "real_exchanged_token":61620.75940295791,
      "real_exchange_USD":98559.79430987992,
      "slippage_USD":1463.695884702378,
      "liquidity_provider_fee":295.67938292963976,
      "gasFees_USD":438.6487212331064,
      "all_spendings_USD":2198.023988865124,
      "spending_percent":1.463352140437165,
      "all_spendings_percent":2.19750779000929
   }
}
```
**Bad result**:
```
{
   "status_CEX":false,
   "status_DEX":false,
   "result_CEX":{},
   "result_DEX":{}
}
```

#### Response Owerview:
| name | description |
|---|---|
| token1_USD | курс токна А |
| token2_USD | курс токна Б |
| expected_USD | /amount_token2 |
| real_exchanged_token | Расчитайнный объём токена Б, который получится в результате обмена |
| real_exchange_USD | Расчитайнный объём токена Б в USD |
| slippage_USD | слипэдж в USD |
| txcost_USD (CEX_only) | комиссия биржи на транзакцию в USD |
| all_spendings_USD | Все расходы на сделку в USD |
| spending_percent | Процент потерь |
| all_spendings_percent | Процент потерь учитывая все расходы |
| liquidity_provider_fee (DEX_only) | комиссия провайдера в USD |
| gasFees_USD (DEX_only) | комиссия на gas в USD |

Routes in dev
