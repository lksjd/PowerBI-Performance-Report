A Power BI dashboard analyzing **Plant Co.'s performance**, providing insights into Sales, Quantity, and Gross Profit, and comparing Year-to-Date (YTD) performance with Prior Year-to-Date (PYTD).

---

The dashboard allows users to explore:
- YTD vs PYTD performance across **Sales, Quantity, and Gross Profit**
- Trends by **Month, Country, and Product**
- Account profitability segmentation using **GP% and selected metrics**
- Dynamic insights driven by slicers for easy metric selection  

---

**Source**: Excel → Power BI

**Key Transformations and Calculations:**
- Calculated **Gross Profit**: `Sales - COGS`  
- Created **YTD metrics** using `TOTALYTD()` for Sales, Quantity, and Gross Profit  
- Created **PYTD metrics** using `CALCULATE()` with `SAMEPERIODLASTYEAR()` filtered by past dates  
- Implemented **dynamic measures** to switch between metrics using slicers  
- Built **dynamic titles** for visuals and the report based on selected metric and year  

---

## Data Model

**Fact Table**: `Fact_Sales`  
- Columns: Sales, Quantity, COGS, Date  
- Measures: Sales, Quantity, Gross Profit, GP%, YTD, PYTD, YTD vs PYTD  

**Dimension Tables**:
- `Dim_Account` with columns: country_code, Account, Master_id, Account_id, latitude, longitude, country, Postal_code, street_name, Street_number
- `Dim_Product` with columns: Product_Family, Product_Family_Id, Product_Group, Product_Group_id, Product_Name, Product_name_id, Product_Size, Product_Type   
- `Dim_Date`: Date hierarchy with Year, Month, and `InPast` flag  
- `Slc_Values`: Slicer values for metric selection (Sales, Quantity, Gross Profit)  

**Relationships**:
- `Fact_Sales[Date_Time]` → `Dim_Date[Date]` - Many to One
- `Fact_Sales[Account_id]` → `Dim_Account[Account_id]` - Many to One
- `Fact_Sales[Product_id]` → `Dim_Product[Product_id]` - Many to One
- Slicer `Slc_Values[Values]` connected to dynamic measures  

---

## Visuals

- **Column Chart**: YTD vs PYTD by Month  
- **Scatter Plot**: Account profitability segmentation by GP% and selected metric  
- **Waterfall Chart**: YTD vs PYTD by Month, Country, and Product  
- **KPI Cards**: Display Sales, Quantity, Gross Profit, and GP%  
- All visuals have **dynamic titles** based on slicer selections  

---

## Dynamic Titles (Examples)
```DAX
_Column Chart title = SELECTEDVALUE(Slc_Values[Values]) & " YTD vs PYTD | Month"
_Report title = "Plant Co. " & SELECTEDVALUE(Slc_Values[Values]) & " Performance " & SELECTEDVALUE(Dim_Date[Date].[Year])
_Scatter title = "Account Profitability Segmentation | GP% and " & SELECTEDVALUE(Slc_Values[Values])
_Waterfall title = SELECTEDVALUE(Slc_Values[Values]) & " YTD vs PYTD | Month - Country - Product"
