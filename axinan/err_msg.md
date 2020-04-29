# csv上传

## order
1. 所有平台
Existing order/item (订单已经存在)
Fail to create order with error: %+v (订单创建失败)
	- empty request (CreateQuoteRequest数量为0)
	- premium: missing required fields: currency, from, to, services, logistics
	- need service (service为空)
	- mismatch service (传入的service不是 transit/gadget)
	- premium: should contain at least one item (---shopeeMy: 包裹项数0)
	- premium: missing id(%s), quantity(%v), weight(%v), tracking number(%v) fields (---shopeeMy)
	- only one service is supported now
	- only one logistic is supported now
	- GetExchange requires the specific date for fxRateAt
	- CurrencyClient.GetExchangeRate (获取汇率错误)
	- PricingService.Pricing (定价请求错误)
	- total amount is zero
	- amount not equal, item total amount: %v, service total amount: %v
	- requires orderID
	- there should has only one premium for order based premium
	- There should has only one premium for order based premium. OrderId: %v
	- Unable to convert transaction %v (---bukalapak)
	- Platform %v not support get policy status from transaction (除了bukalapak/shippit，其他平台都不支持获取保单交易状态)
	- Can't change from status %v to status %v for %#v (不能改变保单状态)
	- Unable to find logistic in premiumQuote %#v for item %#v
	- The order %v already exists. The order level policy doesn't allow duplicated order.
	- Can't find order for policy %v
	- Unknown policy status: %v to create invoice. Policy: %#v (保单状态必须是created/received/activated/cancelled/expired/ineligible中的一种)
	- No data in invoiceSlice
	- Invoice batch for owner email: %v is duplicated. The first one: %v, the second one %v
	- unsupported ownerType %v for getPlaceHolderAndCreateIfNotExisting (ownerType必须是ownerMailBased/platformBased中的一种)
	- There should be only only invoice batch place holder for the platform %v
	- unsupported InvoiceBatchOwnerType %v (ownerType必须是ownerMailBased/platformBased/claimBased中的一种)
	- DAO:
		+ CreateQuote (创建Quote)
		+ GetQuotes (获取Quotes)
		+ getOwnerEmailAndShopIdToOwnerMailMapFromInvoice (获取商家)
		+ getPlaceHolder (获取placeHolder)
		+ getPlaceHolderAndCreateIfNotExisting (创建placeHolder)
		+ Get existing corresponding premium invoice (获取invoice)
		+ CreateOrUpdate (创建或更新 Orders/Policy/InvoiceV2)

DAO:
	- GetShopList (获取商家列表)
	- GetOnlyOrders (获取订单)

2. shippit
Order's shippit date is before shop activated date (订单出货日期早于激活日期)

3. lazadaXb/shopeeXb
No shop info in order (没有订单或商家信息)
No shippited at in order (没有订单出货时间)
Shop doesn't exist (DAO中的商家列表中找不到该商家)
Shop is invalid (订单出货时间晚于Frozen时间/订单出货时间晚于UnRegistered时间/订单出货时间晚于Activated时间)



## order-update
1. 所有平台
Platform %v doesn't support order update (除了bukalapak/shippit，其他平台都不支持订单更新)
Platform %v doesn't support check transation cancelled (除了bukalapak/shippit)
Platform %v doesn't support update policy from partner (除了bukalapak/shippit)
Unknown policy status: %v to create invoice. Policy: %#v (保单状态必须是created/received/activated/cancelled/expired/ineligible中的一种)
No data in invoiceSlice
unsupported InvoiceBatchOwnerType %v (ownerType必须是ownerMailBased/platformBased/claimBased中的一种)
DAO:
	 GetOrders (获取Orders)
	 getPolicy (获取policy)
	 Claims (获取claims)
	 InvoiceV2 (获取InvoiceV2)
	 CreateOrUpdate (创建或更新 InvoiceV2/Policy/Claim)

2. bukalapak/shippit
invalid request (传入的订单条数为0)
invalid order id (订单id为空)
need service (订单服务为空)
mismatch service (传入的服务不是 transit/gadget)
Can't change from status %v to status %v for %#v (不能改变交易状态)
Can't find order for policy %v  (为取消的保单创建发票时无法找到对应的保单)

3. bukalapak
Unable to convert transaction %v (交易状态异常，不是已定义的)



## claim
1. 所有平台
order or policy not fount
without valid claim item to be created, it could be (%s) (多个错误通过‘，‘分隔符合在一起)
	- Policy is empty for claim
	- For item based claim, one claim should have at least one item and one policy. Policy: %v, Order: (%v, %v), len(claimItemsInOrder): %v (--- shopeeMy/shopeeXb/shippit)
	- For item based claim, one claim is only to one item and one policy. Policy: %v, Order: (%v, %v), len(claimItemsInOrder): %v (---除了shopeeMy/shopeeXb/shippit)
	- Item based policy %#v should have only one item (---shopeeMy/shopeeXb/shippit)
	- Order is empty for claim
	- Policy %#v should have claim item input (---shopeeXb: ClaimItemInput数量为0)
	- Third party platform proof is different from %#v to %#v (---shopeeXb: ThirdPartyPlatformProof不相同)
	- not support this claim by proof data. Policy Id: %v, Proof: %#v (---shopeeXb: claimType为空)
	- PricingService.Pricing (定价请求错误) (---shopeeXb)
	- not support payout by proof data for claim type %v (---shopeeXb: 不支持某种索赔类型的支付证明)
DAO:
	GetOrders (获取Orders)
	Shop (获取Shop)
	Policies (获取Policies)
	CoverCoverages (获取CoverCoverages)
	createCoverCoverage (创建CoverCoverages)
	updateCoverCoverage (更新CoverCoverages)
	Claims (创建Claims)



## shop
1. 所有平台
Shop id already exists (商家ID已经存在)
Country not found (商家地区找不到)
Invalid platform %v to set default shop plan (除了lazadaXb/shopeeXb，其他平台不支持shop plan)
no such template for registration (除了lazadaXb/shopeeXb，其他平台没有物流保障服务启用通知邮件模版)
no such template for login (除了lazadaXb/shopeeXb，其他平台没有物流保障服务管理后台登录邀请邮件模版)
DAO:
	GetShopList (获取商家列表)
	CreateShop (创建商家)
	CreateUser (创建用户错误)
	AddOrUpdateUserAppRole (新增或更新用户的app角色错误)
	SendEmail (发送物流保障服务启用通知邮件/物流保障服务管理后台登录邀请邮件错误)

2. lazadaXb
Invalid shop plan name %v to set default shop plan (ShopPlan不是fullCover/halfCover中的一种)

3. shopeeXb
Invalid shop plan name %v to set default shop plan (ShopCountry不是SG/MY/PH/ID/TH/TW/VN中的一种)

