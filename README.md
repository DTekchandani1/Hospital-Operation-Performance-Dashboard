🏥 Hospital Operation & Performance Dashboard (Power BI)

1.Project Overview
The Hospital Operaion & Performance Dashboard is an interactive analytics solution built using Microsoft Power BI to monitor and analyze hospital operations.
It provides insights into:
•	Patient activity 
•	Doctor performance 
•	Financial metrics 
•	Medicine inventory 
•	Hospital resource utilization 
The dashboard uses bookmarks, slicers, and dynamic visuals to enable seamless navigation across multiple views.
2. Key Objectives
•	Track hospital KPIs in real time 
•	Improve decision-making using data insights 
•	Monitor patient flow and discharge trends 
•	Analyze revenue, costs, and doctor commissions 
•	Optimize medicine stock and hospital resources
3. 📊 Dashboard Pages & Insights
🔹 Overview Dashboard
•	Highest medicine purchases: June (Wednesday = 517 units) 
•	Highest patient discharge: Oct 2023 (3 patients) 
•	Total discharged patients: 22 
•	Discharge rate: 73.33% 
•	Surgery charges: 0.43M 
________________________________________
🔹 Patient Dashboard
•	Highest medicine purchases: September (Thursday = 30) 
•	Top medicine: Atorvastatin (39 units) 
•	Highest surgery charges: 55K 
•	Admission Date: 01-Sep-2023 
•	Discharge Date: 10-Sep-2023 
•	Medicine Quantity: 131 
•	Paid Amount: 74K 
________________________________________
🔹 Doctor Dashboard
•	Patient Fees: 1.65K 
•	Patient Spend: 84.02K 
•	Commission Rate: 10% 
•	Commission Earned: 8.40K 
•	Estimated Commission: 21.21K 
________________________________________
🔹 Hospital Dashboard
•	Highest patient age group: 31–45 
•	Bed availability: 
o	General Ward: 55.55% available / 44.44% occupied 
o	ICU: 100% available 
o	Private: 100% occupied 
________________________________________
🔹 Finance Dashboard
•	Highest monthly medicine sales: June (839 units) 
•	Paracetamol stock: 66.05% | Sales: 33.59% 
•	Surgery revenue: 0.43M 
•	Healthcare supplies stock: 52.49% | Sales: 47.51% 
📈 Key Metrics:
•	Total Patients: 30 
•	Doctors: 15 
•	Staff: 20 
•	Bills: 30 
•	Total Revenue: 714K 
•	Medicine Sales Qty: 4K 
•	Total Doctor Salary: 4M 
•	Avg Doctor Salary: 137.83K 
•	Avg Patient Age: 45.57 
•	Commission: 100K 
•	Doctor Commission: 71.38K 
•	Avg Unit Price: 10.53 
•	Avg Spend: 7.60 
•	Staff Salary: 794K 
•	Avg Staff Salary: 39.70K 
•	Stock Value: 42K 
4. Data Model & Features
🔗 Data Sources
•	Patient Info 
•	Appointments 
•	Bills 
•	Medicine Sales 
•	Medical Stock 
•	Staff & Department 
⚙️ Features Implemented
•	✔️ Data Modeling (Relationships) 
•	✔️ DAX Measures & Calculations 
•	✔️ Dynamic KPIs 
•	✔️ Bookmark Navigation (Patient & Doctor views) 
•	✔️ Conditional Icons (Status Indicators) 
•	✔️ Custom Date Formatting 
•	✔️ Star Rating Visualization 
5. Important DAX Measures
Sum of satisfaction_rating star rating =
VAR __MAX_NUMBER_OF_STARS = 5
VAR __MIN_RATED_VALUE = 0
VAR __MAX_RATED_VALUE = 5
VAR __BASE_VALUE = COALESCE(SUM('patient_info'[satisfaction_rating]), 0)

VAR __NORMALIZED_BASE_VALUE =
    MIN(
        MAX(
            DIVIDE(
                __BASE_VALUE - __MIN_RATED_VALUE,
                __MAX_RATED_VALUE - __MIN_RATED_VALUE,
                0
            ),
            0
        ),
        1
    )

VAR __STAR_RATING =
    ROUND(__NORMALIZED_BASE_VALUE * __MAX_NUMBER_OF_STARS, 0)

RETURN
    REPT(UNICHAR(9733), __STAR_RATING)
        & REPT(UNICHAR(9734), __MAX_NUMBER_OF_STARS - __STAR_RATING)


Appointment_ Time_Date = FORMAT(appointments[appointment_time],"hh:mm AM/PM") &"-"& FORMAT(appointments[appointment_date],"dd-mmm")

Ap_Status_Icon =
VAR statusValue = SELECTEDVALUE(appointment[status])
RETURN
IF(
    statusValue = "completed",
    [Check_Icon],
    [Calender_Icon]
)

Doctor_Fees = CALCULATE(SUM(bills[Value]),bills[Charge_Type]="Fees")

Total_Bill_Amt =
VAR Discount =
    CALCULATE(
        SUM(bills[Value]),
        bills[Charge_Type] = "Discount"
    )
VAR TotalAmount =
    SUM(bills[Value])
RETURN
    TotalAmount – Discount

Total_Bill_Amt = 
VAR Discount =
    CALCULATE(
        SUM(bills[Value]),
        bills[Charge_Type] = "Discount"
    )
VAR TotalAmount =
    SUM(bills[Value])
RETURN
    TotalAmount - Discount
Total_Med_Sales_Qty = SUM(medicine_patient[qty])

DR_Commission_Rate = 0.10

DR_Commission_Amt = [Total_Bill_Amt]*[DR_Commission_Rate]

Esitmated_ Commision = VAR Patient_Amt=MAX('Esitmated_ Patient_Amt'[Esitmated_ Patient_Amt])VAR  Commision_Rate =MAX('Esitmated_ Commision _Rate'[Esitmated_ Commision _Rate])VAR Commision_Per=DIVIDE(Commision_Rate,100) RETURN  Patient_Amt*Commision_Per

Surgery_Appointment_ Time_Date = FORMAT(patient_info[surgery_appointment_time],"hh:mm AM/PM") &"-"& FORMAT(patient_info[surgery_appointment_date],"dd-mmm")

Surgery_Status_Icon = 
VAR statusValue = SELECTEDVALUE(patient_info[surgery_status])
RETURN
IF(
    statusValue = "completed",
    [Check_Icon],
    [Calender_Icon]
)
Check_Icon = "https://i.ibb.co/wZg6WTDB/check-mark.png" 
Calender_Icon = "https://i.ibb.co/V0vkPwT2/calendar-1.png" 

Age_Category = VAR Age=patient_info[patient_age] RETURN SWITCH(TRUE(),Age>=18&&Age<=30,"18 To 30",Age>=31&&Age<=45,"31 To 45",Age>=46&&Age<=60,"46 To 60",Age>60,"60+",Age<18,"Under18",BLANK())

Patient_Count=DISTINCTCOUNT(patient_info[patient_id])

Average_Med_Sales_Qty = AVERAGE(medicine_patient[qty])

Total_Stock_Qty = SUM(medical_stock_info[stock_qty])

Stock Value = 
SUM(medical_stock_info[stock_qty]) * AVERAGE(medical_stock_info[unit_price])

Format_Discharge_Date = FORMAT(patient_info[patient_discharge_date],"mmm-yy")

Discharge_Index = MONTH(patient_info[patient_discharge_date])

Admitted_status = "Total" & CALCULATE(patient_info[Patient_Count],patient_info[patient_status] ="Admitted")&"Patient Admitted."

Discharge_Patient = CALCULATE(patient_info[Patient_Count],patient_info[patient_status]="Discharged")
Discharge% = VAR All_Patient=CALCULATE(patient_info[Patient_Count],ALL(patient_info[patient_status])) RETURN DIVIDE([Discharge_Patient],All_Patient)


6. UI/UX Highlights
•	Clean hospital-themed layout 
•	Icon-based indicators for status 
•	Interactive filters and slicers 
•	Bookmark-based navigation 
•	Responsive and user-friendly design 
7. 💡 Business Recommendations
🏥 Operations
•	Increase capacity in Private wards (100% occupied) 
•	Optimize underutilized ICU resources 
💊 Inventory
•	Maintain buffer stock for high-demand medicines like Atorvastatin 
•	Improve stock planning using monthly trends 
💰 Finance
•	Focus on high-revenue services like surgery 
•	Optimize discount strategy to improve net revenue 
👨⚕️ Doctors
•	Review commission structure to align incentives 
•	Monitor doctor-wise performance and patient handling 
👥 Patients
•	Reduce discharge delays to improve bed turnover 
•	Enhance patient satisfaction tracking using rating system


 
 
 
 
 
 
9. Conclusion
This dashboard provides a 360° view of hospital performance, helping stakeholders make data-driven decisions in operations, finance, and patient care.




Deepak Kumar Tekchandani
 Power BI|DAX (Data Analysis Expressions)|Data Modeling|Data Cleaning & Transformation|Interactive Data Visualization

📧 deepakkumartekchandani@gmail.com  
🔗 LinkedIn: www.linkedin.com/in/deepak-kumar-tekchandani-b7200a146
💻 GitHub: https://github.com/DTekchandani1/Hospital-Operation-Performance-Dashboard.git








