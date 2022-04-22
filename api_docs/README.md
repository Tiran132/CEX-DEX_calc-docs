# **Calc API documintation**
# 
# **Endpoints**: 
 

- /[tokens1](##/tokens1)
- /[amount_token2_json](##/amount_token2_json)
- /[with_token1](##/with_token1)
- /[circulating_supply_json](##/circulating_supply_json)
- /[get_USD](##/get_USD)
- /[calculate](##/calculate)
- /[calculate_path_cex](##/calculate_path_cex)
- /[calculate_path_dex](##/calculate_path_dex)

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

## /amount_token2_json
Возвращает предполагаемый (опираясь на курс в $) объём token2

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен А|
|token2| токен Б|
|amount| объём токена А|

#### Response Example:
`https://marsbase.zlumer.com/calc-api/amount_token2_json?token1=ETH&token2=USDT&amount=100`
```
{
    "result": 315388.6703608296,
    "status": true
}

```
**Bad result**:
```
{
    "status": false
}
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
## /circulating_supply_json

Возвращает circulating_supply токена с CoinMaretCap

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен для которого узнаём значение|

#### Response Example:
`hhttps://marsbase.zlumer.com/calc-api/circulating_supply_json?token1=PAXG`
```
{
    "result": 318453.941,
    "status": True,
}

```
**Bad result**: (бывает, что в cmc нет значенией circulating_supply)
```
{
    "status": false,
}
``` 


## /get_USD
Возвращает курс токенов + их /amount_token2_json

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
| expected_USD | /amount_token2_json |
| real_exchanged_token | Расчитайнный объём токена Б, который получится в результате обмена |
| real_exchange_USD | Расчитайнный объём токена Б в USD |
| slippage_USD | слипэдж в USD |
| txcost_USD (CEX_only) | комиссия биржи на транзакцию в USD |
| all_spendings_USD | Все расходы на сделку в USD |
| spending_percent | Процент потерь |
| all_spendings_percent | Процент потерь учитывая все расходы |
| liquidity_provider_fee (DEX_only) | комиссия провайдера в USD |
| gasFees_USD (DEX_only) | комиссия на gas в USD |




## /calculate_path_cex
Возвращает все расчёты на CEX (+ routes)

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен А|
|token2| токен Б|
|amount| объём токена А|

#### Response Example:
`https://marsbase.zlumer.com/calc-api/calculate_path_cex?token1=USDT&token2=1INCH&amount=100000`
```
{
   "result":{
      "token1_USD":1.0004183799317068,
      "token2_USD":1.7893579162422115,
      "expected_USD":100041.83799317067,
      "real_exchanged_token":55562.83432231578,
      "real_exchange_USD":99421.7974434902,
      "slippage_USD":620.0405496804742,
      "txcost_USD":99.4217974434902,
      "all_spendings_USD":719.4623471239644,
      "spending_percent":0.6197812456452481,
      "all_spendings_percent":0.7191614643996027,
      "routes":[
         {
            "path":[
               "USDT",
               "1INCH"
            ],
            "end_amounts":[
               55562.83432231578
            ],
            "slippages":[
               0.6197812456452481
            ],
            "end_slippage":0.6197812456452528
         }
      ]
   },
   "status":true
}
```
**Bad result**:
```
{
   "status":false,
   "result":{},
}
```

#### Response Owerview:
| name | description |
|---|---|
| token1_USD | курс токна А |
| token2_USD | курс токна Б |
| expected_USD | /amount_token2_json |
| real_exchanged_token | Расчитайнный объём токена Б, который получится в результате обмена |
| real_exchange_USD | Расчитайнный объём токена Б в USD |
| slippage_USD | слипэдж в USD |
| txcost_USD | комиссия биржи на транзакцию в USD |
| all_spendings_USD | Все расходы на сделку в USD |
| spending_percent | Процент потерь |
| all_spendings_percent | Процент потерь учитывая все расходы |
| routes | Вся информация о лучшем пути (везде 100%) |




## /calculate_path_dex
Возвращает все расчёты на  DEX (+ routes)

**Type**: GET, POST
**Params**
| name | description |
|---|---|
|token1| токен А|
|token2| токен Б|
|amount| объём токена А|

#### Response Example:
`https://marsbase.zlumer.com/calc-api/calculate_path_dex?token1=USDT&token2=1INCH&amount=100000`
```
{
   "status":true,
   "result":{
      "token1_USD":1.0004183799317068,
      "token2_USD":1.7893579162422115,
      "expected_USD":100041.83799317067,
      "real_exchanged_token":64901.38430120595,
      "real_exchange_USD":116131.80577444086,
      "slippage_USD":0,
      "liquidity_provider_fee":348.3954173233226,
      "gasFees_USD":255.22578084768375,
      "all_spendings_USD":603.6211981710063,
      "spending_percent":0.0,
      "all_spendings_percent":0.603368760790073,
      "routes":[
         [
            "USDT",
            "ETH",
            "1INCH"
         ]
      ]
   }
}
```
**Bad result**:
```
{
   "status":false,
   "result":{},
}
```

#### Response Owerview:
| name | description |
|---|---|
| token1_USD | курс токна А |
| token2_USD | курс токна Б |
| expected_USD | /amount_token2_json |
| real_exchanged_token | Расчитайнный объём токена Б, который получится в результате обмена |
| real_exchange_USD | Расчитайнный объём токена Б в USD |
| slippage_USD | слипэдж в USD |
| all_spendings_USD | Все расходы на сделку в USD |
| spending_percent | Процент потерь |
| all_spendings_percent | Процент потерь учитывая все расходы |
| liquidity_provider_fee | комиссия провайдера в USD |
| gasFees_USD | комиссия на gas в USD |
| routes | Вся информация о лучшем пути (везде 100%) |
