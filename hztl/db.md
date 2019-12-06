# 数据导入

## 财务
1. p_Rece (应收账款主表) ---> financials.receivable_payables
0				--> payment_type
CorpID			--> company_id
ReceType 		--> source_bill_type (正常维修业务\保险业务\保修业务\交车后退货)
ReceNo			--> source_bill_no
Vendorinno		--> cooperator_id
ActforCorpNo	--> settle_cooperator_id
Curr			--> amount 			(Curr=ACurr+ZCurr+FareCurr 表示已结清)
ACurr			--> real_amount
ZCurr			--> discount_amount
FareCurr		--> prepay_amount 	(积分抵扣金额，先记为预收付金额)
PurchaseDate	--> business_date
ConfirmDate		--> confirm_date
ManiputeCode	--> business_man_id	(业务员名称转ID)
Remarks			--> remark
OriginNo 		--> 	源单号？

//应收
P_Rece. CorpID = p_ReceChk.CorpID
P_Rece.ReceNo = p_ReceChk.ReceNo

P_Rece.CorpID = p_Recevir.CorpID
P_Rece.ReceNo = p_Recevir.ReceNo

应收款(虚)= p_Rece.Curr+p_Recevir.VCurr
应收已结(虚)=p_Rece.ACurr+p_Rece.ZCurr+p_Rece.FareCurr+p_ReceVir.VACurr
应收款(实)= p_Rece.Currr
应收已结(实)=p_Rece.ACurr+p_Rece.ZCurr+p_Rece.FareCurr

2. p_ReceChk (应收账款结算明细表) ---> financials.receivable_payable_details
ZfNo 			支付单号, 结算单号为全库统一大流水号
CorpID 			所属分店
ReceNo 			业务单号
VendorInno 		客户内码
Curr 			业务单金额
DCurr 			本次收款金额
ZCurr 			本次优惠金额
Vcurr 			大单结算金额
PayCode 		支付方式
PayNo 			支票号
PaidDate 		冲减日期(结算日期)
PaidMan 		付款人
Operator 		操作者(经办人)
SetCorpID 		结算分店
ActforCorpNo	 代收单位内码
Remarks 		备注
FareCurr 		本次积分抵扣金额
BillMark 		单据标识
SetVendorinno 	结算单位内码


3. p_Pay (应付账款主表)
CorpID 			所属分店
ReceNo 			业务单号
VendorInno 		供应商内码
Curr 			应付金额
ACurr 			付款金额
ZCurr 			优惠金额
PurchaseDate 	业务开单日期
ConfirmDate 	业务财务审核日期
PromDate 		承诺日期
ManiPuteCode 	业务员
Remarks 		备注
ReceType 		单据类型
OriginNo 		原始单号
BillMark 		单据标识
ModiVersion 	时间戳
ActforMark 		代收标识(1 表示代收)
ActforCorpNo 	代收单位内码
RegDate 		应收登记日期
Register 		应收登记人
Operator 		业务单制单人(5.5.9.10)
OriginVendor 	原单位内码(5.5.9.10)
Verifier 		业务单财务审核人(5.5.9.10)

//应付
P_Pay.CorpID = p_PayChk.CorpID
P_Pay.ReceNo = p_PayChk.ReceNo

应付款 = p_Pay.Curr
应付已结 = p_Pay.ACurr+p_Pay.ZCurr

4. p_PayChk (应付账款结算明细表)
ZfNO 			结算单号
CorpID 			所属分店
ReceNo 			业务单号
Vendorinno 		供应商内码
Curr 			应付金额
DCurr 			本次付款金额
ZCurr 			本次优惠金额
PayCode 		支付方式
PayNo 			支票号
PaidDate 		支付日期
PaidMan 		支付人
Operator 		操作者
SetCorpID 		结算分店
Remarks 		备注
Billmark 		单据标识
SetVendorinno 	结算供应商内码


5. P_RpH(收付款流水主表)
CorpID 			分店ID号（业务分店ＩＤ）
RpType 			区分收/付[ 0 收 1 付]
PayType 		收付类型(
采购付款、采购退款、销售收款、销售退款、结算付款、结算收款、调拨结算付款、调拨结算收款、预收款、预付款、采购付订金、销售收订金、采购自付运费、采购代垫运费、销售自付运费、销售代垫运费、正常维修、储值卡充值、洗车、美容、交车后退货)
Rq 				收付款日期
PayCode 		支付方式，如现金、支票、汇票
sumCur 			应收|付金额
DCurr 			收现|付现金额
ZCurr 			免收(优惠)金额
FareCurr 		积分抵扣金额
PreCurr 		预收预付冲减金额
Kmh 			帐户号(暂不填)
AbsTract 		摘要(暂不填)
PurchaseDate 	业务日期
PurchaseNo 		业务单据号
ScoreDate 		结算日期
Vendorinno 		往来结算单位
Corp_hs 		分店(内部往来单位)
sumTax 			税额
sumCb 			成本
sumTrans 		商品成本差价
ManiputeCode 	业务员
Handler 		经办人(结算人)
Checker 		负责人(暂不填)
Remarks 		备注
Pzh 			关联 p_mxk 中PZH
sumCJ
PzCorpID
PzNy
BillMark
ModiVersion
RCorpID
Operator 		单据的制单人（用于辅助核算项目）5.6.0.0
LogNo 			涉及的车牌号（用于辅助核算项目）5.6.0.0


6. P_RphChk(收付款流水细表)
ParentPKID
PayCode
Curr
Kmh
用于存储单笔业务的多结算方式。P_RPH.PKID=P_RphChk.ParentPKID



7. P_rpTransfer(业务转账表)
Rq 				审核日期
CorpID 			分店ID号
PurchaseType 	单据类型(采购、销售、调入、调出、盘点、其它出库|、应收登记、应付登记、正常维修、保修、保险、洗车、美容、交车后退货))
PurchaseNo 		业务单号
PurchaseDate 	业务日期
VendorInno 		往来单位
RemeID 			对方分店
sumCur 			单据金额
sumTax 			税额
sumCb 			成本
sumTrans 		运费等费用
ManiputeCode 	业务员
Operator 		制单人
Remarks
PZh 			关联 p_mxk 中PZH
sumCJ
Zcurr
PzCorpID
PzNy
BillMark
ModiVersion
RCorpID
LogNo 			涉及的车牌号（用于辅助核算项目）5.6.0.0


1、	p_Deme (调拨结算表)
CorpID 			所属分店
DemeNo 			结算单号
PurchaseDate 	结算日期
DemeID 			调拨分店ID
Debit 			借方金额
Credit 			贷方金额
ZfNo
Remarks 		备注
DbType 			调拨类型
ReceType 		货款类型
OriginNo 		原始单号
BillMark 		单据标识

1、	p_DemeAcc (调拨结算明细表)
CorpID 			所属分店
ZfNo 			支付单号
PaidDate 		支付日期
Debit 			借方金额
Credit 			贷方金额
PayCode 		支付方式
InvoiceCode 	发票类型
InvoiceNo 		发票号
PaidMan 		付款人
Operator 		操作者
DemeID 			调拨分店
Remarks 		备注
BillMark 		单据标识

1、	X_OutJobFixCorp (外协单位费用表)
CorpID 			分店名称
RepairNo 		维修工单号
JobID 			维修项目ID,对应X_Repair表的PKID
Vendorinno 		外协单位（内码）
Curr 			应付费用金额
ACurr 			已付费用金额
PayCode 		支付方式



1、	X_OutJobFixCorpChk (外协费用支付表)
ZFNO 			支付单号  (支付单号的取值使用存储过程 pro_ GetBillsNextNO)
SetCorpID 		支付分店
PaidDate 		付款日期
Vendorinno 		外协单位内码
RepairNo 		维修工单号
CorpID 			业务单分店ID
JobID 			维修项目ID,对应X_Repair表的PKID
DCurr 			付款金额
PayCode 		支付方式
Operator 		付款人(操作员)
Remarks 		付款备注


1、	p_CashReg (交款登记表)
CorpID 			所属分店
CashType 		交款类型
Rq 				交款日期
DCurr 			交款额
Handler 		交款人
Collecter 		收款人
Checker 		确认人
Remarks 		备注
CheckDate 		确认日期
RpType 			收付类型
Kmh 			科目号


1、	p_Commis (代销结算表)
CorpID 			所属分店
PuchaseNo 		业务单号
PurchaseDate 	业务日期
VendorInno 		客户内码
PayCode 		支付方式
PayNo 			支票号
InvoiceCode 	发票方式
InvoiceNo 		发票号
InvoiceMark 	收票标识
SettleQty 		结算数量
SettleCurr 		结算金额
PaidCurr 		支付金额
SumCurr 		应收金额
ZCurr 			免收金额
ManiputeCode 	业务员
Operator 		操作者
sCorer 			结算员
sCoreDate 		结算日期
Receiver 		审核员
ConfirmDate 	审核日期
Paidman 		付款人
Remarks 		备注
Status 			状态
InvoiceCurr 	已开票金额
ACurr 			已结金额
FareCurr 		结算积分抵扣
PriCurr 		结算优惠
PrintCount 		打印次数


1、	p_CommisChk (代销结算明细表)
OriginPKID 		源明细PKID
PurchaseID 		关联主表PKID
PartInno 		配件内码
SettleQty 		结算数量
SettleIprc 		结算单价
SettleCurr 		结算金额
Oprc 			实结算单价
Curr 			实结算金额
Remarks 		备注


1、	p_Entrust (受托结算表)
CorpID 			所属分店
PuchaseNo 		业务单号
PurchaseDate 	业务日期
VendorInno 		客户内码
PayCode 		支付方式
PayNo 			支票号
InvoiceCode 	发票方式
InvoiceNo 		发票号
InvoiceMark 	开票标识
SettleQty 		结算数量
SettleCurr 		结算金额
PaidCurr 		支付金额
SumCurr 		应付金额
ZCurr 			免付金额
ManiputeCode 	业务员
Operator 		操作者
sCorer 			结算员
sCoreDate 		结算日期
Receiver 		审核员
ConfirmDate 	审核日期
Paidman 		收款人
Remarks 		备注
Status 			状态
InvoiceCurr 	已收票金额
ACurr 			已付金额
PriCurr 		结算优惠
PrintCount 		打印次数



1、	p_EntrustChk (受托结算明细表)
OriginPKID 			源明细PKID
PurchaseID 			关联主表PKID
PartInno 			配件内码
SettleQty 			结算数量
SettleIprc 			结算单价
SettleCurr 			结算金额
Iprc 				实结算单价
Curr 				实结算金额
Remarks 			备注



1、	p_Redeploy (请调主表)
PurchaseID
PurchaseNo 			业务单号
CorpID
所属分店
PurchaseDate
业务日期
PurchaseType
单据类型
RemeID
对方分店
RemeDate
对方日期
DeployID
对方单ID
sumQty
合计数量
sumCur
合计金额
sumCb
合计成本
Operator
操作者
Verifier
审核
Remarks
备注
Status
状态
confirmDate
确认日期
Intransact
事务标志
sumTranset
合计运费
PackNo
包装方式
TransNo
运输方式
SendType
提货方式
PrintCount
打印次数
sumHcb
合计除税成本
Receiver
请调人
VeriDate
请调日期


1、	p_Redeploy _Sub (请调细表)
字段名称
类型
长度
主键/索引
说明
PKID
Int

*

PurchaseID
Int

Not null
关联主表ID
PartInno
Int

Not null
配件内码
Depot
nvarchar
10
Not null
仓库
Ware
Nvarchar
20

货位
Qty
numeric
18,3
Not null
数量
Iprc
Numeric
18,6
Not null
含税价
Cbprc
Numeric
18,6

除税价
Oprc
Numeric
18,6

零售价
Oprp
Numeric
18,6

批发价
Oprd
Numeric
18,6

调拨价
RetQty
Numeric
18,3

退还数量
RetFromPurchaseID
Int


退还单ID
InputNo
nvarchar
16

批次号
Oprp1
Numeric
18,6

批价1
Oprp2
Numeric
18,6

批价2
Oprp3
Numeric
18,6

批价3
Oprp4
Numeric
18,6

批价4
Remarks
nvarchar
10

备注
Transetprc
Numeric


运费单价
InputID
Int


入库ID
CbNiprc
Numeric


除税成本价
StockFlags
nvarchar
10

库存标识
sOrderPKID
int


客户订货标识
Oprs
Numeric
18,6

索赔价
CIFprc
Numeric
18,6

CIF单价
TExRate
Numeric
18,6

通关汇率
CExRate
Numeric
18,6

成本汇率
TTaxRate
Numeric
18,6

通关税率
TariffCurr
Numeric
18,4

关税税额
AddTaxCurr
Numeric
18,4

增值税税额



1、	p_Remelody (调出主表)
字段名称
类型
长度
主键/索引
说明
PurchaseID
Int

*

PurchaseNo
nvarchar
16
Not null
业务单号
CorpID
Int

Not null
所属分店
PurchaseDate
Datetime

Not null
业务日期
PurchaseType
Nchar
1

单据类型
RemeID
Int


对方分店
RemeDate
Datetime


对方日期
DeployID
Int


对方单ID
sumQty
numeric


合计数量
sumCur
Numeric


合计金额
sumCb
Numeric


合计成本
Operator
Nvarchar


操作者
Verifier
Nvarchar


审核
Remarks
Nvarchar


备注
Status
Int


状态
confirmDate
Datetime


确认日期
Intransact
Int


事务标志
PackNo
Nvarchar


包装方式
TransNo
Nvarchar


运输方式
SendType
Nvarchar


提货方式
SumTranset
Numeric
18,4


PrintCount
int


打印次数
sumHcb
Numeric


合计除税成本


1、	p_Remelody _Sub (调出细表)
字段名称
类型
长度
主键/索引
说明
PKID
Int

*

PurchaseID
Int

Not null
关联主表ID
PartInno
Int

Not null
配件内码
Depot
nvarchar
10
Not null
仓库
Ware
Nvarchar
20

货位
Qty
numeric
18,3
Not null
数量
Iprc
Numeric
18,6
Not null
含税价
Cbprc
Numeric
18,6

除税价
Oprc
Numeric
18,6

零售价
Oprp
Numeric
18,6

批发价
Oprd
Numeric
18,6

调拨价
InputNo
nvarchar
16

批次号
RetQty
Numeric
18,3

退还数量
RetFromPurchaseID
Int


退还单ID
Oprp1
Numeric
18,6

批价1
Oprp2
Numeric
18,6

批价2
Oprp3
Numeric
18,6

批价3
Oprp4
Numeric
18,6

批价4
InputID
Int


入库ID
CbNiprc
Numeric


除税成本价
StockFlags
nvarchar
10

库存标识
sOrderPKID
int


客户订货标识
Remarks
nvarchar
10

备注
Transetprc
Numeric


运费单价
Oprs
Numeric
18,6

索赔价
CIFprc
Numeric
18,6

CIF单价
通关汇率
TExRate
Numeric
18,6

CExRate
Numeric
18,6

成本汇率
通关税率
关税税额
TTaxRate
Numeric
18,6

TariffCurr
Numeric
18,4


AddTaxCurr
Numeric
18,4








