---
title: Open Enrollment Analysis
---

<Details title="How to edit this page">
  This page can be found in your project at `/pages/your-page.md`. 
  Make a change to the markdown file and save it to see the change take effect in your browser.
</Details>

```sql all_years
SELECT DISTINCT "Year" AS year
FROM enrollcsv.enrollment
ORDER BY year;
```

```sql all_enrolling_districts
SELECT DISTINCT "Enrolling District" AS enrolling_district
FROM enrollcsv.enrollment;
```

<Dropdown data={all_years} name="selected_year" label="Year" value="year"> <DropdownOption value="%" valueLabel="All Years"/> </Dropdown>

<!-- <Dropdown data={all_enrolling_districts} name="selected_district" label="Enrolling District" value="enrolling_district"> <DropdownOption value="%" valueLabel="All Districts"/> </Dropdown> -->

<Dropdown data={all_enrolling_districts} name=selected_district value=enrolling_district/>



```sql enrollment_year_count
SELECT 
  "Year",
  "Enrolling District",
  SUM("Count of Open Enrolled") AS total_open_enrolled
FROM enrollcsv.enrollment
WHERE "Year" LIKE '${inputs.selected_year.value}'
  AND "Enrolling District" LIKE '${inputs.selected_district.value}'
GROUP BY "Year", "Enrolling District"
ORDER BY total_open_enrolled DESC;
```

<DataTable data={enrollment_year_count}/>


```sql open_enrollment_by_resident
SELECT 
  "Resident District" AS resident_district,
  SUM("Count of Open Enrolled") AS total_outgoing
FROM enrollcsv.enrollment
WHERE "Year" LIKE '${inputs.selected_year.value}'
  AND "Enrolling District" LIKE '${inputs.selected_district.value}'
GROUP BY "Resident District"
ORDER BY total_outgoing DESC;
```

<BarChart data={open_enrollment_by_resident} title="Open Enrolled Students by Resident District" x="resident_district" y="total_outgoing" />

<LineChart 
    data={open_enrollment_by_resident}
    x=resident_district
    y=total_outgoing 
    yAxisTitle="Open Enrolled Students by Resident District"
/>