# Sql
use [Ahan]
select * from Position
select * from dbo.SaleProfit
select * from dbo.SaleTabel
------ Question الف
------ Question1
select sum(t.Quantity*t.UnitPrice) SumSale 
from dbo.SaleTabel t-------جمع فروش:35050


------ Question2

select  count(distinct t.Customer) CountCustomer 
from dbo.SaleTabel t----تعداد مشتریان متمایزی که خرید داشته اند:9

------ Question3

select  t.Product,sum(t.Quantity*t.UnitPrice) [فروش هر شرکت]   
from dbo.SaleTabel t
group by t.Product-----Product1:3900 , Product2:4050 , Product3:4800, Product4:11550, Product5:8800, Product6:1950

------ Question4
select  t.Customer,sum(t.Quantity*t.UnitPrice) [خرید هر مشتری] ,count(t.Product) [تعداد آیتم] ,count(t.orderID) [تعداد فاکتور]
from dbo.SaleTabel t
group by t.Customer
having sum(t.Quantity*t.UnitPrice)>1500----
/*Customer	خرید هر مشتری	تعداد آیتم	  تعداد فاکتور
C1	        5150	     5	                5
C2	        9150	     11	                11
C3	        4150	     4	                4
C4	        5100	     5	                5
C5	        3300	     5	                5
C6	        3150	     3	                3
C8	        3500	     3	                3*/

------ Question5

select sum(p.ProfitRatio) [مبلغ سود کل] into dbo.CTE from dbo.SaleProfit p 
select dbo.CTE.[مبلغ سود کل] from  dbo.CTE


select sum(t.Quantity*t.UnitPrice)*0.8 [مبلغ سود],(sum(t.Quantity*t.UnitPrice)*0.8)/sum(t.Quantity*t.UnitPrice) [سود از مبلغ کل] from dbo.SaleTabel t
inner join dbo.SaleProfit p  on p.Product=t.Product
group by t.Product,p.ProfitRatio------مقدار سود:28040و  سود از مبلغ کل:0.8

------ Question6
select  count( t.Date) over(partition by t.Date) CountCustomer 
from dbo.SaleTabel t----سفارش در روز اول:17و روز دوم:14و روز سوم:9

------ Question ب
select * from Position
select p.Id,p.name,p.manager,p.Manager_Id,po.name as nameManager,case when po.manager is  NULL then 'Level1' else 'Level2' end as [بالاترین مدیر] 
from dbo.Position p
left join dbo.Position po on p.Manager_Id=po.Id
