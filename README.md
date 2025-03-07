# Learn-SQL
Querys de los Ejercicios sql-practice

# Base de Datos Hospital
###### Diagrama 


## Nivel Básico
##### Pregunta 1
Show first name, last name, and gender of patients whose gender is 'M'
```
SELECT 
first_name, last_name, gender 
from patients
where gender = 'M'
```
##### Pregunta 2
Show first name and last name of patients who does not have allergies. (null)
```
SELECT first_name, last_name
from patients
where allergies IS NULL
```
##### Pregunta 3
Show first name of patients that start with the letter 'C'
```
SELECT first_name
from patients
where first_name like 'C%'
```
##### Pregunta 4
Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)
```
SELECT first_name, last_name
from patients
where weight >=100 AND weight <= 120
```
##### Pregunta 5
Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'
```
update patients
 set allergies = 'NKA'
where allergies is NULL;
```
##### Pregunta 6
Show first name and last name concatinated into one column to show their full name.
```
select
concat(first_name,' ' ,last_name) AS full_name
from patients
```
##### Pregunta 7
Show first name, last name, and the full province name of each patient.
Example: 'Ontario' instead of 'ON'
```
select A.first_name, A.last_name, B.province_name
FROM patients AS A
LEFT JOIN province_names AS B ON A.province_id = B.province_id
```
##### Pregunta 8
Show how many patients have a birth_date with 2010 as the birth year.
```
select count(birth_date) AS total_patients
from patients
where birth_date like '2010%'
```
##### Pregunta 9
Show the first_name, last_name, and height of the patient with the greatest height.
```
select first_name, last_name, max( height)
from patients
```
##### Pregunta 10
Show all columns for patients who have one of the following patient_ids:
1,45,534,879,1000
```
select *
from patients
where patient_id in (1,45,534,879,1000)
```
##### Pregunta 11
Show the total number of admissions
```
select count(patient_id) as total_admissisons
from admissions
```
##### Pregunta 12
Show all the columns from admissions where the patient was admitted and discharged on the same day.
```
select *
from admissions
where admission_date = discharge_date
```
##### Pregunta 13
Show the patient id and the total number of admissions for patient_id 579.
```
select 
patient_id, 
count(admission_date)
from admissions
where patient_id = 579
```
##### Pregunta 14
Based on the cities that our patients live in, show unique cities that are in province_id 'NS'.
```
select distinct (city) as unique_cities
from patients
where province_id like 'NS%'
```
##### Pregunta 15
Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70
```
select first_name, last_name, birth_date
from patients
where height > 160 AND weight > 70
```
##### Pregunta 16
Write a query to find list of patients first_name, last_name, and allergies where allergies are not null and are from the city of 'Hamilton'
```
select first_name, last_name, allergies
from patients
where allergies IS not null AND city = 'Hamilton'
```

## Nivel Medio
##### Pregunta 1
Show unique birth years from patients and order them by ascending.
```
select distinct(year (birth_date))
from patients
order by birth_date asc
```
##### Pregunta 2
Show unique first names from the patients table which only occurs once in the list.
For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.
```
SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(first_name) = 1
```
##### Pregunta 3
Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.
```
SELECT patient_id, first_name
FROM patients
WHERE first_name LIKE 'S%' 
  AND first_name LIKE '%S'
  AND LENGTH(first_name) >= 6;
```
##### Pregunta 4
Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.
Primary diagnosis is stored in the admissions table.
```
SELECT
P.patient_id, P.first_name, P.last_name
FROM patients as P
left join admissions as A on A.patient_id = P.patient_id
where A.diagnosis='Dementia'
```
##### Pregunta 5
Display every patient's first_name.
Order the list by the length of each name and then by alphabetically.
```
select first_name 
from patients
order by len(first_name), first_name
```
##### Pregunta 6
Show unique birth years from patients and order them by ascending.
```
select distinct(year (birth_date))
from patients
order by birth_date asc
```
##### Pregunta 7
Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.
```
SELECT 
count(case gender when 'M' then 1 end) as male_count,
count(case gender when 'F' then 1 end) as female_count
from patients
```
##### Pregunta 8
Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.
```
SELECT 
first_name, last_name, allergies
from patients
where allergies = 'Penicillin' or allergies = 'Morphine'
order by allergies asc, first_name asc, last_name asc
```
##### Pregunta 9
Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.
```
select 
patient_id, diagnosis
from admissions
group by patient_id, diagnosis
having count (diagnosis = diagnosis)>1
```
##### Pregunta 10
Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending.
```
select
city,
count(patient_id) as num_patients
from patients
group by city
order by num_patients desc, city asc
```
##### Pregunta 11
Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"
```
select first_name, last_name,'Patient' as role
from patients
union all
select first_name, last_name,'Doctor' as role
from doctors
```
##### Pregunta 12
Show all allergies ordered by popularity. Remove NULL values from query.
```
select allergies, count(*) as total_diagnosis
from patients
where allergies not null
group by allergies
order by total_diagnosis desc
```

##### Pregunta 13
Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.
```
select first_name, last_name, birth_date
from patients
where year(birth_date) >= 1970 AND year(birth_date) < 1980
order by birth_date asc
```
##### Pregunta 14
We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane
```
select
CONCAT(upper(last_name),',',lower(first_name)) as new_name_format
from patients
order by first_name desc
```
##### Pregunta 15
Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.
```
SELECT 
province_id,
sum(height) as sum_height
FROM patients
group by province_id
having sum(height) >= 7000
```
##### Pregunta 16
Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'
```
SELECT
max(weight) - min(weight) as weight_delta
from patients
where last_name = "Maroni"
```
##### Pregunta 17
Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.
```
SELECT
day(admission_date) as day_number,
count(*) as number_of_admissions
from admissions
group by day_number
order by number_of_admissions desc
```
##### Pregunta 18
Show all columns for patient_id 542's most recent admission_date.
```
SELECT 
patient_id,
max(admission_date) as admission_date,
discharge_date,
diagnosis,
attending_doctor_id
from admissions
where patient_id=542
```
##### Pregunta 19
Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.
```
SELECT 
    patient_id,
    attending_doctor_id,
    diagnosis
FROM 
    admissions
WHERE 
    patient_id % 2 <> 0
    AND attending_doctor_id IN (1, 5, 19) 
         OR (attending_doctor_id LIKE "%2%" AND LENGTH(patient_id) = 3)
```
##### Pregunta 20
Show first_name, last_name, and the total number of admissions attended for each doctor.
Every admission has been attended by a doctor.
```
select
first_name,
last_name,
count(admission_date) as admissions_total
from admissions 
join doctors on doctors.doctor_id = admissions.attending_doctor_id
group by first_name, last_name
```
##### Pregunta 21
For each doctor, display their id, full name, and the first and last admission date they attended.
```
SELECT 
doctors.doctor_id, 
concat(doctors.first_name,' ', doctors.last_name) as full_name,
max(admissions.admission_date) as first_admission_date,
min(admissions.admission_date) as last_admission_date

FROM doctors
join admissions on  doctors.doctor_id = admissions.attending_doctor_id
group by doctor_id, full_name
```
##### Pregunta 22
Display the total amount of patients for each province. Order by descending.
```
select 
count(patients.patient_id) as patient_count,
province_names.province_name as province_names
from patients
join province_names on patients.province_id = province_names.province_id
group by province_names 
order by patient_count desc
```
##### Pregunta 23
For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.
```
select 
concat(patients.first_name,' ', patients.last_name) as patient_name,
admissions.diagnosis as diagnosis,
concat(doctors.first_name,' ', doctors.last_name) as doctor_name
from admissions
left join patients on admissions.patient_id = patients.patient_id
left join doctors on admissions.attending_doctor_id = doctors.doctor_id
```
##### Pregunta 24
display the first name, last name and number of duplicate patients based on their first name and last name.
Ex: A patient with an identical name can be considered a duplicate.
```
select
first_name,
last_name,
count(patient_id) as num_of_duplicates
from patients
group by first_name, last_name
having num_of_duplicates>1
```
##### Pregunta 25
Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date,
gender non abbreviated.

Convert CM to feet by dividing by 30.48.
Convert KG to pounds by multiplying by 2.205.
```
select
concat(first_name, ' ', last_name),
ROUND(height / 30.48, 1) AS height,
ROUND(weight * 2.205,0) AS weight,
birth_date,
case 
	when gender = 'M' then 'MALE'
    when gender = 'F' then 'FEMALE'
end gender_type
from patients
```
##### Pregunta 26
Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)
```
SELECT 
    patients.patient_id,
    patients.first_name,
    patients.last_name
FROM 
    patients
LEFT JOIN 
    admissions 
ON 
    patients.patient_id = admissions.patient_id
WHERE 
    admissions.patient_id IS NULL;
```
##### Pregunta 27
Display a single row with max_visits, min_visits, average_visits where the maximum, minimum and average number of admissions per day is calculated. Average is rounded to 2 decimal places.
```
select 
	max(number_of_visits) as max_visits, 
	min(number_of_visits) as min_visits, 
  round(avg(number_of_visits),2) as average_visits 
from (
  select admission_date, count(*) as number_of_visits
  from admissions 
  group by admission_date
)
```
## Nivel Avanzado
##### Pregunta 1
Show all of the patients grouped into weight groups.
Show the total amount of patients in each weight group.
Order the list by the weight group decending.
For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.
```
SELECT
    FLOOR(weight / 10) * 10 AS weight_group,
    COUNT(*) AS total_patients
FROM patients
GROUP BY FLOOR(weight / 10)
ORDER BY FLOOR(weight / 10) DESC
)
```
##### Pregunta 2
Show patient_id, weight, height, isObese from the patients table.
Display isObese as a boolean 0 or 1.
Obese is defined as weight(kg)/(height(m)2) >= 30.
weight is in units kg.
height is in units cm
```
select
patient_id,
weight,
height,
case
    when weight/(POWER((height / 100.0), 2)) >= 30 then 1
    else 0
end as isObese
from patients
)
```
##### Pregunta 3
Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'
```
select
a.patient_id,
a.first_name as patient_first_name,
a.last_name as patient_last_name,
c.specialty as attending_doctor_speciality
from admissions as b
left join patients as a on a.patient_id = b.patient_id
left join doctors as c on c.doctor_id = b.attending_doctor_id
where b.diagnosis = 'Epilepsy' and 
c.first_name = 'Lisa'
)
```
##### Pregunta 4
All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.
The password must be the following, in order:
1. patient_id
2. the numerical length of patient's last_name
3. year of patient's birth_date
```
select
a.patient_id as patient_id, 
concat(a.patient_id,len(b.last_name),year(b.birth_date)	)as temp_password
from admissions as a
join patients as b on b.patient_id = a.patient_id
group by a.patient_id
```
##### Pregunta 5
Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.
Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.
```
SELECT 
    CASE 
        WHEN patient_id % 2 = 0 THEN 'Yes'
        ELSE 'No'
    END AS has_insurance,
    SUM(CASE 
            WHEN patient_id % 2 = 0 THEN 10
            ELSE 50
        END) AS cost_after_insurance
FROM admissions
GROUP BY has_insurance
)
```
##### Pregunta 6
Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name
```
SELECT a.province_name
FROM province_names a
         JOIN patients p ON a.province_id = p.province_id
GROUP BY province_name
HAVING SUM(CASE WHEN p.gender = 'M' THEN 1 ELSE 0 END) > SUM(CASE WHEN p.gender = 'F' THEN 1 ELSE 0 END)
)
```
##### Pregunta 7
We are looking for a specific patient. Pull all columns for the patient who matches the following criteria:
- First_name contains an 'r' after the first two letters.
- Identifies their gender as 'F'
- Born in February, May, or December
- Their weight would be between 60kg and 80kg
- Their patient_id is an odd number
- They are from the city 'Kingston'
```
SELECT * 
FROM patients
WHERE 
    LENGTH(first_name) >= 2 AND
    SUBSTRING(first_name, 3, 1) = 'r' AND
    gender = 'F' AND 
    birth_date IS NOT NULL AND
    MONTH(birth_date) IN (2, 5, 12) AND 
    weight BETWEEN 60 AND 80 AND 
    patient_id % 2 <> 0 AND 
    city = 'Kingston';
)
```
##### Pregunta 8
Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.
```
WITH porcentaje_T AS (
    SELECT COUNT(*) AS sexo_T
    FROM patients
),
porcentaje_M AS (
    SELECT COUNT(*) AS sexo_M
    FROM patients
    WHERE gender LIKE '%M%'
)
SELECT 
   concat(ROUND(CAST(porcentaje_M.sexo_M AS FLOAT) / porcentaje_T.sexo_T * 100, 2),'%') AS percent_of_male_patients
FROM porcentaje_T, porcentaje_M;
)
```
##### Pregunta 9
For each day display the total amount of admissions on that day. Display the amount changed from the previous date.
```
select
admission_date, 
count(*) as admission_day,
COUNT(*) - LAG(COUNT(admission_date)) OVER (ORDER BY admission_date) as admission_count_change
from admissions
group by admission_date
)
```
##### Pregunta 10
Sort the province names in ascending order in such a way that the province 'Ontario' is always on top.
```
select province_name
from province_names
order by province_name = 'Ontario' desc
)
```
##### Pregunta 11
We need a breakdown for the total amount of admissions each doctor has started each year. Show the doctor_id, doctor_full_name, specialty, year, total_admissions for that year.
```
WITH conteo AS (
    SELECT
        attending_doctor_id,
        COUNT(*) AS admisiones,
        YEAR(admission_date) AS anio
    FROM admissions
    GROUP BY attending_doctor_id, YEAR(admission_date)
)
SELECT
    a.doctor_id,
    CONCAT(a.first_name, ' ', a.last_name) AS doctor_full_name,
    a.specialty,
    b.anio AS year,
    b.admisiones AS total_admissions
FROM doctors AS a
JOIN conteo AS b ON a.doctor_id = b.attending_doctor_id
ORDER BY b.anio, a.doctor_id;
```
