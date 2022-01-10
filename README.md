# Получение всех tx с erc-20 contract  
- # адрес актуальной версии контракта в rinkeby
  0xCcCF2Fb4B674121526c6b60cF711c1791C0ffE84
- # пример ивентов на этом контракте
  Event в контракте из представленного abi был только один - OwnershipTransferred
  https://tiran132.github.io/json_data/event_info.json
  #
  Транзакции выходит получить все, пример транзакции - https://tiran132.github.io/json_data/tx_reciept.json
    

- # ABI и исходник этого контракта (по крайней мере интерфейсы ивентов)
  Contract - https://tiran132.github.io/json_data/MarsBaseExchange%20(1).json
  ABI - https://tiran132.github.io/json_data/MBABI.json
  ```
  interface MyInterface {
    event OfferAccepted(uint256 offerId, MarsBaseExchange.MBOffer offer);
    event OfferCancelled(uint256 offerId, MarsBaseExchange.MBOffer offer);
    event OfferCreated(uint256 offerId, MarsBaseExchange.MBOffer offer);
    event OfferModified(uint256 offerId, MarsBaseExchange.MBOffer offer);
    event OfferPartiallyAccepted(
        uint256 offerId,
        MarsBaseExchange.MBOffer offer
    );
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    function offers(uint256)
        external
        view
        returns (
            uint256 offerId,
            uint8 offerType,
            address tokenAlice,
            uint256 amountAlice,
            uint256 feeAlice,
            uint256 feeBob,
            uint256 smallestChunkSize,
            uint256 amountRemaining,
            uint256 minimumSize,
            uint256 deadline,
            address offerer,
            address payoutAddress,
            bool active
        );

    function owner() external view returns (address);

    function renounceOwnership() external;

    function transferOwnership(address newOwner) external;

    function setCurrentTime() external;

    function setMinimumFee(uint256 _minimumFee) external;

    function getOffer(uint256 offerId)
        external
        view
        returns (MarsBaseExchange.MBOffer memory);

    function getAllOffers()
        external
        view
        returns (MarsBaseExchange.MBOffer[] memory);

    function price(
        uint256 amountAlice,
        uint256 offerAmountAlice,
        uint256 offerAmountBob
    ) external pure returns (uint256);

    function createOffer(
        address tokenAlice,
        address[] memory tokenBob,
        uint256 amountAlice,
        uint256[] memory amountBob,
        uint256[] memory fees,
        uint256[] memory offerParameters
    ) external payable returns (uint256);

    function cancelOffer(uint256 offerId) external payable returns (uint256);

    function changeOfferPrice(
        uint256 offerId,
        address[] memory tokenBob,
        uint256[] memory amountBob
    ) external payable returns (uint256);

    function changeOfferPricePart(
        uint256 offerId,
        address[] memory tokenBob,
        uint256[] memory amountBob,
        uint256 smallestChunkSize
    ) external payable returns (uint256);

    function acceptOffer(
        uint256 offerId,
        address tokenBob,
        uint256 amountBob
    ) external payable returns (uint256);

    function acceptOfferPart(
        uint256 offerId,
        address tokenBob,
        uint256 amountBob
    ) external payable returns (uint256);

    function acceptOfferPartWithMinimum(
        uint256 offerId,
        address tokenBob,
        uint256 amountBob
    ) external payable returns (uint256);
    }
    
    interface MarsBaseExchange {
        struct MBOffer {
            uint256 offerId;
            uint8 offerType;
            address tokenAlice;
            address[] tokenBob;
            uint256 amountAlice;
            uint256[] amountBob;
            uint256 feeAlice;
            uint256 feeBob;
            uint256 smallestChunkSize;
            uint256 amountRemaining;
            uint256 minimumSize;
            uint256[] minimumOrderAmountsAlice;
            uint256[] minimumOrderAmountsBob;
            address[] minimumOrderAddresses;
            address[] minimumOrderTokens;
            uint256 deadline;
            address offerer;
            address payoutAddress;
            bool[] capabilities;
            bool active;
        }
    }
  ```

- # описание фильтров ивентов, которые надо отслеживать
    | Фильтр | Описание / коментарий |
    | ------ | ------ |
    | Токен | *я предполагаю, что юзеру удобнее будет вводить символ, но вообще можно и адресс контракта* |
    | Объём | value передаваемого токена |
    | Цена Drive | *тут я не понял, нужна цена с оффера на MB или рыночная стоимость* |
    | Дискаунт | разница между рыночной стоимостью и ценой оффера |

