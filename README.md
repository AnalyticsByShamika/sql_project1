# sql_project1
COVID-19 Reported Patient Impact and Hospital Capacity by State


SELECT date, state,
    SUM (previous_day_admission_adult_covid_confirmed) AS total_admissions
FROM [COVID-19_Reported_Patient]
GROUP BY  date, state
ORDER BY. date, state;


SELECT  state,
    AVG(previous_day_admission_pediatric_covid_confirmed +. 
(previous_day_admission_adult_covid_confirmed + previous_day_admission_pediatric_covid_confirmed) * 100) AS avg_percentage
FROM  
[COVID-19_Reported_Patient]
WHERE
    date >= DATEADD(MONTH, -1, GETDATE()) 
GROUP BY
    state
ORDER BY
    avg_percentage DESC;


SELECT state,
    total_staffed_adult_icu_beds AS total_icu_beds,
    SUM(staffed_adult_icu_bed_occupancy) AS occupied_icu_beds,
    (total_staffed_adult_icu_beds - SUM(staffed_adult_icu_bed_occupancy)) AS available_icu_beds,
    (SUM(staffed_adult_icu_bed_occupancy) / total_staffed_adult_icu_beds) * 100 AS occupancy_rate
FROM
    [COVID-19_Reported_Patient]
WHERE
    staffed_icu_adult_patients_confirmed_and_suspected_covid_coverage > 0
GROUP BY
    state, total_staffed_adult_icu_beds
ORDER BY
    occupancy_rate DESC;


SELECT
    AVG(inpatient_beds_utilization) AS avg_inpatient_utilization,
    AVG(adult_icu_bed_utilization) AS avg_icu_utilization
FROM
   [COVID-19_Reported_Patient]
