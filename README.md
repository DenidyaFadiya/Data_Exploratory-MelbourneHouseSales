# MelbourneHouseSales
**Data Exploratory using SQL**
In this Project I will be exploring the data to see the analysis on house sales across the Melbourne by seeing which House Type, which Region, which Seller has the most house sold.

## Full Query for Data Exploratory Project
**Check the Full Query for Data Exploratory Project [here](https://github.com/DenidyaFadiya/Data_Exploratory-MelbourneHouseSales/blob/main/Full%20Query%20-%20Data%20Exploratory%20Melbourne%20House%20Sales).**

In this Data Exploratory project I will be cleaning data that I took from [Kaggle Data Source](https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot).

## Exploratory Data Project

This Exploratory data I am going to be using the data I have cleaned in the previous Data Cleaning Project
I have 3 tables which are ; SalesData, Seller and Type

**SalesData**
 ![image](https://user-images.githubusercontent.com/129844542/235099687-0bfa9903-036c-4dc5-a592-614c64d34c27.png)

 

**Seller**
 ![image](https://user-images.githubusercontent.com/129844542/235099703-f4fc20fe-1279-4258-8cf0-09bb55e39177.png)


**Type**
 ![image](https://user-images.githubusercontent.com/129844542/235099720-c13ccac9-a246-4ce3-8322-29048ac04dbb.png)


*Optional : Changing the TypeId in SalesData using the data that represents the alphabet
But I am not gonna do it since I will be using join later. But here is the query if you want to change it prior to the project
```
select House_Id, typeId,
case
	when typeId = 'h' then 'House'
	when typeId = 't' then 'Townhouse'
	else 'Apartment'
end as TypeFixed
from dbo.SalesData
order by House_Id
```
 ![image](https://user-images.githubusercontent.com/129844542/235099854-dcc5ff78-f843-4070-a52c-a898723d1407.png)


Update query 
```
update dbo.SalesData
set typeId = case typeId
	when 'h' then 'House'
	when 't' then 'Townhouse'
	else 'Apartment'
end
from dbo.SalesData
```
 ![image](https://user-images.githubusercontent.com/129844542/235099873-5962bef6-de45-4d80-adfa-9a4915cba0c6.png)

![image](https://user-images.githubusercontent.com/129844542/235099887-20db95c2-64e5-4281-9b45-7d503dec1234.png)


### Joining all the tables
This is the cleaned and complied data that I am going to be using, it consists of all the data that I will be analyzing in this Exploratory Project. I filtered the building area and landsize to get rid of the nulls and zeros otherwise the data won’t be too accurate. 
 ```
 select sal.House_Id, 
		Suburb, 
		ty.Type,
		Price, 
		Distance, 
		Date, 
		Landsize, 
		BuildingArea,
		RegionName,
		Seller
from dbo.SalesData sal
join dbo.Seller sel
	on	sal.House_Id = sel.House_id
join dbo.type ty
	on ty.typeId = sal.TypeId
where BuildingArea is not null
and not Landsize = '0'
order by House_Id
```
 ![image](https://user-images.githubusercontent.com/129844542/235099919-ff347995-dcf0-4066-86c2-5e3496669605.png)

![image](https://user-images.githubusercontent.com/129844542/235099929-2c7e9499-8a43-4f28-ae11-8091d4678398.png)

### Count how many houses are in Melbourne
Total houses sold in Melbourne that has full data is 6.077 
 ```
 select count(typeid) as TotalHousesInMelbourne
from dbo.SalesData sal
where BuildingArea is not null
and not Landsize = '0'
```
 ![image](https://user-images.githubusercontent.com/129844542/235099958-0f4415e9-cf67-43f8-ac03-c004d7f56b03.png)

![image](https://user-images.githubusercontent.com/129844542/235099965-7f26a576-4154-4c93-8d3d-05a579b86978.png)





### See which type is mostly sold in Melbourne
From the result below, people in Melbourne tend to buy House compared to other houses types.
From 6.077 houses sold 4.782 belong to House type
```
select ty.type,
		count(type) as TotalHousesInMelbourne
from dbo.SalesData sal
join dbo.type ty
	on ty.typeId = sal.TypeId
where BuildingArea is not null
and not Landsize = '0'
group by type
order by TotalHousesInMelbourne desc
```
![image](https://user-images.githubusercontent.com/129844542/235100016-c3b167e9-990b-485e-9f5b-2d16e3634a96.png)

![image](https://user-images.githubusercontent.com/129844542/235100032-ba87db82-4f68-413e-9386-b5694a61b38c.png)

 
 


### See which Region has the most house sold
Southern Metropolitan has the most houses sold compared to other Region amongs the Melbourne with 1.887 houses
```
select Regionname,
		count(RegionName) as TotalHouseSoldPerRegion
from dbo.SalesData sal
where BuildingArea is not null
and not Landsize = '0'
group by regionname
order by TotalHouseSoldPerRegion desc
```
![image](https://user-images.githubusercontent.com/129844542/235100090-fc76fd2f-51da-4db4-b5f2-d66fe98f50fd.png)
![image](https://user-images.githubusercontent.com/129844542/235100106-549ea6bf-afc1-4bfa-80a4-4af256abc9fe.png)

### See the spreading of houses on each Region.

From the result below, we can see the amount of houses sold per Region separated by the house type
```
select type,
		Regionname,
		count(type) as TotalHouseByType
from dbo.SalesData sal
join dbo.type ty
	on ty.typeId = sal.TypeId
where BuildingArea is not null
and not Landsize = '0'
group by type, regionname
order by type, TotalHouseByType desc
```
![image](https://user-images.githubusercontent.com/129844542/235100183-2296ef67-d1fb-4b51-bfb4-023782f6ee27.png)

 ![image](https://user-images.githubusercontent.com/129844542/235100212-671f31fb-8db7-4d10-b070-a49f53816c4c.png)

 

### Average House Price across the Melbourne
This Query shows that even though Southern Area has the most houses sold, the average price is still the highest amongst the other Region
```
select RegionName,
		avg(price) as AverageHousePricePerRegionName
from dbo.SalesData
where BuildingArea is not null
and not Landsize = '0'
group by regionname
order by AverageHousePricePerRegionName desc
```
![image](https://user-images.githubusercontent.com/129844542/235100273-ae7e2959-899a-4d51-8d2c-4a453ba11a1a.png)

![image](https://user-images.githubusercontent.com/129844542/235100296-e3672c04-ed79-4494-8c0f-6aa9c15b9d0b.png)



### Houses sold in Southern Metropolitan
From the analysis above, we can all asume that Southern Metropolitan is leading in most houses sold, but which Suburb has the most houses sold?
It is Bentleigh East leading with the most houses sold in Southern Metropolitan with 118 houses.
```
select top 10 Suburb, 
	count(type) as SoldPerSuburb
from dbo.SalesData sal
join dbo.type ty
	on ty.typeId = sal.TypeId
where regionname like '%southern%'
and BuildingArea is not null
and not Landsize = '0'
group by Suburb
order by SoldPerSuburb desc
```
![image](https://user-images.githubusercontent.com/129844542/235100329-c89d9243-0b00-4c31-965e-b67baa0d31ea.png)

![image](https://user-images.githubusercontent.com/129844542/235100345-1f2af085-8d9e-4294-b30c-09575d9735fd.png)

### Which Suburb in Southern Metropolitan Region has the most houses sold
Let’s go a little deeper, Bentleigh East is the leading Suburb in Southern Metropolitan Region, but how is the spreading of houses type sold in Bentleigh East out of those 118?
The result says House is leading with 84, Townhouse on the 2nd with 22 and apartment is the least desired with only 12.
```
select suburb,
		type,
		count(type) as SoldInBentEast
from dbo.SalesData sal
join dbo.type ty
	on ty.typeId = sal.TypeId
where Suburb = 'Bentleigh  East'
and BuildingArea is not null
and not Landsize = '0'
group by suburb, type 
order by SoldInBentEast desc
```
![image](https://user-images.githubusercontent.com/129844542/235100365-e673a3f1-3f1f-45fa-8e3e-1407b6168276.png)
![image](https://user-images.githubusercontent.com/129844542/235100379-22c37ee5-7177-4a9b-9b42-f7ee3060ef31.png)


### Top 10 Seller in Quantity
This query will show whose seller sold the most houses in quantity.
Nelson is leading with 726 houses sold and giving almost 12% to the sales.
```
select top 10 Seller,
		count(type) as SellerHouseSold,
		count(*) * 100.0 / sum(count(*)) over() as HouseSoldPercentage
from dbo.SalesData sal
join dbo.Seller sel
	on	sal.House_Id = sel.House_id
join dbo.type ty
	on ty.typeId = sal.TypeId
where BuildingArea is not null
and not Landsize = '0'
group by seller
order by SellerHouseSold desc
```
 ![image](https://user-images.githubusercontent.com/129844542/235100407-ee5c7882-6322-43f7-85b5-0528e8e3f6d7.png)

![image](https://user-images.githubusercontent.com/129844542/235100430-ddfe504e-8423-47e9-a2b1-7a270ebc7a5d.png)

### Top 10 Seller in Revenue
This query will show whose seller sold the most houses in Revenue.
Jellis brought the most revenue to the company with $902.793.922 and giving almost 13% to the company revenue.
```
select top 10 Seller,
		sum(price) as TotalPricePerSeller,
		sum(price) * 100.0 / sum(sum(price)) over() as RevenuePerSeller
from dbo.SalesData sal
join dbo.Seller sel
	on	sal.House_Id = sel.House_id
join dbo.type ty
	on ty.typeId = sal.TypeId
where BuildingArea is not null
and not Landsize = '0'
group by seller
order by TotalPricePerSeller desc
```
 ![image](https://user-images.githubusercontent.com/129844542/235100443-75cbde51-df3e-492f-9f7d-badc6ef57e37.png)

![image](https://user-images.githubusercontent.com/129844542/235100455-fc00d74b-c327-4e67-acee-d756152982c9.png)

### Average Distance from the Property to the downtown
If seen by the distance, Northern Metropolitan has the closest property to the downtown area
```
select RegionName, 
		avg(distance) as AverageDistance
from dbo.SalesData sal
where BuildingArea is not null
and not Landsize = '0'
group by regionname
order by AverageDistance
```
![image](https://user-images.githubusercontent.com/129844542/235100474-50944b34-a9f8-4718-9171-abcd01252110.png)
![image](https://user-images.githubusercontent.com/129844542/235100488-a537e831-67dd-4f09-91d1-5800daf9555e.png)


### Average Landsize per Region Name
Northern Victoria is also leading with the biggest landsize compared to the other Region.
```
select RegionName, 
		avg(Landsize) as AverageLandsize
from dbo.SalesData sal
where BuildingArea is not null
and not Landsize = '0'
group by regionname
order by AverageLandsize desc
```
![image](https://user-images.githubusercontent.com/129844542/235100537-129c540a-9f29-4ae1-b8fe-16be0ce96c41.png)

 ![image](https://user-images.githubusercontent.com/129844542/235100553-6cd0bfa2-a6ab-4032-b101-5d78208e2619.png)


### Average Builiding Area per Region Name
Northern Victoria is also leading with the biggest Builiding Area compared to the other Region.
```
select RegionName, 
		avg(BuildingArea) as AverageBuildingArea
from dbo.SalesData sal
where BuildingArea is not null
and not Landsize = '0'
group by regionname
order by AverageBuildingArea desc
```
 ![image](https://user-images.githubusercontent.com/129844542/235100564-752e332d-3189-4f7e-acdc-b65865583b5d.png)

![image](https://user-images.githubusercontent.com/129844542/235100578-cde5d72a-0c47-4115-adba-e73c7042052a.png)


