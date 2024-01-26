# Customer Retention Dashboard (Churn)
<p align = "center">
  <img src="https://github.com/Icaro92/PBI_AW/assets/58118599/de3d6640-760c-47c3-83af-0732cbb43f23">

## Objectives
This dashboard aims to provide a better understanding of consumer profiles to avoid churn. Currently, the company only gets in touch with customers who have already terminated their contract and, based on this dashboard, it would be possible to contact customers who are at risk of terminating their contract based on their profile.

KPIs include:
  * Customers who left within the last month
  * Services each customer has signed for: Phone, Multiple lines, Internet, Online security, etc.
  * Customer Account Information: Contract, Payment Method, Monthly Charges, etc.
  * Demographic info about customers

You can **access the dashboard** in the link below:
* [Churn Dasboard](https://app.powerbi.com/view?r=eyJrIjoiZjI4MDE5MmMtNjdlMS00ZDUyLTlmMDItMDJjNGViMGYyNDNiIiwidCI6IjAyODQyZDljLWVhNTAtNGVkNy1iMWY1LWI2MDIwOGIwM2YzNyJ9)

## Data Source

This project was based on the [PwC in Switzerland](https://www.theforage.com/simulations/pwc-ch/power-bi-cqxg) job simulation from The Forage. The simulation provided data and directions into what was expected for the analysis.

## Business Request 
Janet, the Retention Manager Manager at PhoneNow requested our services in the e-mail below. She's looking for a better understanding of the customer profile correlated with Churns. She wants to be able to predict a churn based on the customer profile and act before it happens. She provided us with some input:

<p align = "left">
  <img src="(https://github.com/Icaro92/PBI_AW/assets/58118599/3ea7c1b6-9073-4516-bec7-392d310bec27" width="600" alt="Claire Business Request">

## Power BI Data Transformation

To provide accurate data and make the visualizations meaningful, a few Measures were created using DAX:

* _Churn rate %_ = DIVIDE (CALCULATE(COUNT('01 Churn-Dataset'[Churn]), '01 Churn-Dataset'[Churn] = "yes" ),CALCULATE ( COUNT ('01 Churn-Dataset'[Churn]), ALLSELECTED ('01 Churn-Dataset'[Churn] ) ))
*  _Count of Churn for Yes_ = CALCULATE(COUNTA('01 Churn-Dataset'[Churn]), '01 Churn-Dataset'[Churn] IN { "Yes" })
*  _Dependents in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[Dependents]), '01 Churn-Dataset'[Dependents]="Yes", '01 Churn-Dataset'[Churn]= "Yes"), CALCULATE(COUNT('01 Churn-Dataset'[Dependents]), '01 Churn-Dataset'[Churn]="Yes"),0)
* _Device Protection in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[DeviceProtection]), '01 Churn-Dataset'[DeviceProtection]="Yes", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[DeviceProtection]),'01 Churn-Dataset'[Churn]="Yes"),0)
* _Multiple lines "No" in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[MultipleLines]), '01 Churn-Dataset'[MultipleLines]="No", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[MultipleLines]),'01 Churn-Dataset'[Churn]="Yes", '01 Churn-Dataset'[MultipleLines] <> "No phone service"),0)
*  _Multiple lines "Yes" in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[MultipleLines]), '01 Churn-Dataset'[MultipleLines]="Yes", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[MultipleLines]),'01 Churn-Dataset'[Churn]="Yes", '01 Churn-Dataset'[MultipleLines] <> "No phone service"),0)
*  _Online Backup in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[OnlineBackup]), '01 Churn-Dataset'[OnlineBackup]="Yes", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[OnlineBackup]),'01 Churn-Dataset'[Churn]="Yes"),0)
*  _Online Sec. in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[OnlineSecurity]), '01 Churn-Dataset'[OnlineSecurity]="Yes", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[OnlineSecurity]),'01 Churn-Dataset'[Churn]="Yes"),0)
*  _Partner in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[Partner]), '01 Churn-Dataset'[Partner]="Yes", '01 Churn-Dataset'[Churn]= "Yes"), CALCULATE(COUNT('01 Churn-Dataset'[Partner]), '01 Churn-Dataset'[Churn]="Yes"),0)
*  _Phone Service in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[PhoneService]), '01 Churn-Dataset'[PhoneService]="Yes", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[PhoneService]),'01 Churn-Dataset'[Churn]="Yes"),0)
*  _Senior Citizen in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[SeniorCitizen]), '01 Churn-Dataset'[SeniorCitizen]=1, '01 Churn-Dataset'[Churn]= "Yes"), CALCULATE(COUNT('01 Churn-Dataset'[SeniorCitizen]), '01 Churn-Dataset'[Churn]="Yes"),0)
* _Streaming Movies in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[StreamingMovies]), '01 Churn-Dataset'[StreamingMovies]="Yes", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[StreamingMovies]),'01 Churn-Dataset'[Churn]="Yes"),0)
* _Streaming TV in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[StreamingTV]), '01 Churn-Dataset'[StreamingTV]="Yes", '01 Churn-Dataset'[Churn] ="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[StreamingTV]),'01 Churn-Dataset'[Churn]="Yes"),0)
* _Tech Support in %_ = DIVIDE(CALCULATE(COUNT('01 Churn-Dataset'[TechSupport]), '01 Churn-Dataset'[TechSupport]="Yes", '01 Churn-Dataset'[Churn]="Yes"),CALCULATE(COUNT('01 Churn-Dataset'[TechSupport]),'01 Churn-Dataset'[Churn]="Yes"),0)
* **The table Lolalty** was also created for classification of tenure
  * = Table.AddColumn(#"Changed Type", "Loyalty", each if [tenure] < 12 then "< 1 year" 
      else if [tenure] < 24 then "< 2 years" 
      else if [tenure] < 36 then "< 3 years" 
      else if [tenure] < 48 then "< 4 years" 
      else if [tenure] < 60 then "< 5 years" 
      else "< 6 years")  

## The Dashboard

The finished dashboard has two pages: "Churn"  and "Customer Risk Analysis". The first page provides an overview of the customer profile of those who churned, including demographics, customer account information, and services they signed for. The second page provides an overview of all Customers, classifying churn rates by services, product and financial categories. 

You can find the dash in the link below:
 * [Customer Retention Dashboardn](https://app.powerbi.com/view?r=eyJrIjoiZjI4MDE5MmMtNjdlMS00ZDUyLTlmMDItMDJjNGViMGYyNDNiIiwidCI6IjAyODQyZDljLWVhNTAtNGVkNy1iMWY1LWI2MDIwOGIwM2YzNyJ9)


  
