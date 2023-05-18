
<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a name="readme-top"></a>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->


![Power BI](https://img.shields.io/static/v1?style=for-the-badge&message=Power+BI&color=222222&logo=Power+BI&logoColor=F2C811&label=)
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/JoshuaOlubori/UK-Road-Accident-Casualties/blob/f47c7d604613183d31617d101d14ef5c96503f1d/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/joshua-edun


<!-- PROJECT LOGO -->
<br />
<div align="center">

  <h3 align="center">UK Road Accidents and Casualties Tracking Dashboard (2021 - 2022)</h3>

  <p align="center">
    Featuring data modeling in Power BI, 
    and Dax <br />



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ul>
    <li><a href="#requirement">Requirement gathering</a></li>
        <li><a href="#stakeholders">Identifying stakeholders</a></li>
        <li> <a href="#raw-data">Understanding raw data</a></li>
        <li><a href="#data-cleansing">Data cleansing </a></li>
        <li><a href="#data-processing">Data processing</a></li> 
<li><a href="#modeling">Data modeling</a></li>
   <li><a href="#visualization">Data visualization</a></li> 
    <li><a href="#insights">Insights</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
</details>


<div align="left">
<!-- ABOUT THE PROJECT -->
  
## About The Project 🍪 

![Dashboard](https://github.com/JoshuaOlubori/UK-Road-Accident-Casualties/blob/ddb49a64610e84d36ded41d2096050e7a2f3c183/report.png)

Tracking Accidents and Casualties across UK Roads in 2021 - 2022
<a name="requirement"/>
### Requirement Gathering

Client wants to create a dashboard on road accidents for the year 2021 and 2022.

#### a. Primary KPIs
- Total Casualties and Total Accident Values:
  - Current Year
  - Year-over-Year (YoY) Growth

- Total Casualties by Accident Severity:
  - Current Year
  - Year-over-Year (YoY) Growth

#### b. Secondary KPIs
- Total Casualties with Respect to Vehicle Type:
  - Current Year

- Monthly Trend Comparison of Casualties:
  - Current Year vs. Previous Year

- Casualties by Road Type:
  - Current Year

- Current Year Casualties by Area/Location & Day/Night

- Total Casualties and Total Accidents by Location
Total casualties and total accidents by location

<!-- -->
  <a name="stakeholders"/>
  
## Identifying Stakeholders 🧑🏽‍💼

- Emergency Services Departments
- Road Safety Corps
- Traffic Management Agencies
- Police Force
- General public
  
<a name="raw-data"/>
  
### Understanding Raw Data 🥩
  
Data Grain: A single instance of a reported accident event in the UK in 2021 amd 2022

| Fields | description (datatype) |
| ------------- | ------------- |
| Accident_Index  | unique row indentifier (string)  |
| Accident Date  | date of accident event (date)  |
| Day_of_Week  | day of the week (string)  |
| Junction_Control  | mechanism of traffic control at junction (string)  |
| Junction_Detail  | type of junction (string)  |
| Accident_Severity  | severity of accident (string)  |
| Latitude  | latitude (float)  |
| Light_Conditions  | light conditions at accident scene (string)  |
| Local_Authority_(District)  | name of district (string)  |
| Carriageway_Hazards  | hazards on the road if any  |
| Number_of_Casualties  | number of causaulties (integer)  |
| Number_of_Vehicles  | number of vehicles involved in the accident (integer)  |
| Police_Force  | Police force jurisdiction (string) |
| Road_Surface_Conditions  | condition of road surface (string)  |
| Road_Type  | road type (string) |
| Speed_limit  | road speed limit in mph (string)  |
| Time  | time of day accident occurred (time)|
| Urban_or_Rural_Area  | Whether accident occurred in an urban or rural area (string)  |
| Weather_Conditions | weather condition at time of accident (string)  |
| Vehicle_Type | type of vehicle involved in the accident  |


  <a name="data-cleansing"/>
  
### Data cleansing 🧹
  
Issues
- Junction_Control column has, among others, two values "Auto traffic sigl" and "Auto traffic signal" of which the former is a misspelling of the latter

- Instances where "Fatal" is misspelled as "Fetal" in Accident_Severity column

- "Time" columnwas represented as a datetime data type in Power BI

Fixes
- Used the Replace Values feature to correct the misspellings of Issues 1 & 2
- Changed the data type to time accordingly
- All fixes were done in Power Query

  <a name="data-processing"/> 
### Data Processing ⚙️

- The data needs a calendar table so as to use Time Intelligence functions further down the line.

- Using the CALENDAR function, a new table with calculated, dynamic columns of Date, Month and Year was generated.

  <a name="modeling"/>
### Data Modeling 🏛 

- a one-to-many active relationship was established between the calendar table and the data table 
![Schema](https://github.com/JoshuaOlubori/UK-Road-Accident-Casualties/blob/5e13ba670d1fef8f97a8643586e15cbc5a55f74e/data-model.png)

  <a name="visualization"/>
### Data Visualization 🎨

- Power BI magic!✨ The report pbix file is available in this repo to explore design decisions
  The following DAX measures were used:
  
```
Count of Accidents = DISTINCTCOUNT(Data[Accident_Index])

Current Year Accident Count = TOTALYTD(COUNT(Data[Accident_Index]),'Calendar'[Date])

Current Year Casualties = TOTALYTD(SUM(Data[Number_of_Casualties]),'Calendar'[Date])

Previous Year Accident Counts = CALCULATE(COUNT(Data[Accident_Index]), SAMEPERIODLASTYEAR('Calendar'[Date]))

Previous Year Casualties = CALCULATE(SUM(Data[Number_of_Casualties]),
SAMEPERIODLASTYEAR('Calendar'[Date]))

YoY Accident Count = DIVIDE([Current Year Accident Count] - [Previous Year Accident Counts], [Previous Year Accident Counts])

YoY Casualties = (DIVIDE([Current Year Casualties] - [Previous Year Casualties],[Previous Year Casualties]))
```
  
  <a name="insights"/>
  
### Deriving Insights

- The vehicle type most involved in accidents was cars. This can be explained by the fact that most vehicles plying UK roads are cars

- There is a general decrease in casualty counts in 2022 compared to 2021
 

<!-- CONTACT  ☎️ -->

  <a name="contact"/>
  
## Contact

Edun Joshua Olubori - [connect on linkedin](https://www.linkedin.com/in/joshua-edun) - joshuaolubori@gmail.com

<p align="right">(<a href="#readme-top">back to top</a>)</p>



