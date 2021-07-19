# School_District_Analysis

## Project Overview
A school board notified a client that there were signs of academic dishonesty among the ninth grade students at Thomas High School and has given me the responsibility of analyzing the data to see if these allegations are true. The main approach that I will use is to replace all of the ninth grade reading and math scores with null values to see if the ranking of the school changes among its sister schools and if the average score changes significantly.

### Findings After Analysis

#### How is the district summary affected?
To begin, I used a code that would remove the ninth graders from the data and replace their math and reading scores with NaN's:

```
# Install numpy using conda install numpy or pip install numpy. 
# Step 1. Import numpy as np.
import numpy as np

# Step 2. Use the loc method on the student_data_df to select all the reading scores from the 9th grade at Thomas High School and replace them with NaN.
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"), "reading_score"] = np.nan
student_data_df

#  Step 3. Refactor the code in Step 2 to replace the math scores with NaN.
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"), "math_score"] = np.nan
student_data_df

ninth_grades = school_data_complete_df.loc[(
    school_data_complete_df["school_name"] == "Thomas High School") & 
    (school_data_complete_df["grade"] == "9th") & 
    (school_data_complete_df["math_score"].isnull()), "Student ID"].count()

# Get the total student count 
student_count = school_data_complete_df["Student ID"].count()

# Step 2. Subtract the number of students that are in ninth grade at 
# Thomas High School from the total student count to get the new total student count.
total_count = student_count - ninth_grades
```
Initially, it appears that there was little change when it comes to comparing the before and after effects.

![DisSum_After](https://user-images.githubusercontent.com/46951897/126096386-7c1e0dd6-d070-41e5-9c41-1ed6db5d7f9a.JPG)

![DisSum_Before](https://user-images.githubusercontent.com/46951897/126096318-a12f4e44-5dc2-4b3b-8a8e-884b4245eade.JPG)

However, upon further reflection, it must be noted that the ninth graders within Thomas High School are only 461 students out of 39,170, or 1.1% of the student population. Upon taking their numbers out of the student count, the figures either remained the same or dropped.

#### How is the school summary affected?
To begin this stage of the analysis, I had to isolate the 10th, 11th, and 12th graders so that the 9th grade scoers did not affect the analysis so I used this code for the final tally:

```
# Step 5.  Get the number of 10th-12th graders from Thomas High School (THS).
non_ninth_graders = school_data_complete_df.loc[
    (school_data_complete_df["school_name"] == "Thomas High School") & 
    (school_data_complete_df["grade"] != "9th"), "Student ID"].count()
non_ninth_graders

# Step 6. Get all the students passing math from THS
kids_passing_math_THS = school_data_complete_df.loc[
    (school_data_complete_df["math_score"] >= 70) & 
    (school_data_complete_df["school_name"] == "Thomas High School"), "Student ID"].count()
kids_passing_math_THS

# Step 7. Get all the students passing reading from THS
kids_passing_reading_THS = school_data_complete_df.loc[
    (school_data_complete_df["reading_score"] >= 70) & 
    (school_data_complete_df["school_name"] == "Thomas High School"), "Student ID"].count()
kids_passing_reading_THS

# Step 8. Get all the students passing math and reading from THS
kids_passing_math_reading_THS = school_data_complete_df.loc[
    (school_data_complete_df["math_score"] >= 70) & 
    (school_data_complete_df["reading_score"] >= 70) & 
    (school_data_complete_df["school_name"] == "Thomas High School"), "Student ID"].count()
kids_passing_math_reading_THS
```

This is where the grades become a little more suspicious among the ninth grade students. 

![SchSum_Before](https://user-images.githubusercontent.com/46951897/126097330-1d62f94e-a05f-48bc-8c55-97dcab93ffcb.JPG)

![SchSum_After](https://user-images.githubusercontent.com/46951897/126097336-5d46657a-f450-4ccf-a41f-11e7f3cd986d.JPG)

When the ninth graders are removed, the school's performance suffers heavily by experiencing an approximate drop of 23%, 28%, and 25% in math, reading, and overall passing percent, respectively. In addition, while these figures increase dramatically, the  average grade for these subjects barely change.

#### How does replacing the ninth graders’ math and reading scores affect Thomas High School’s performance relative to the other schools?
When it comes to examining the performance of the school district, in terms of "% Overall Passing", Thomas High School once again suffers heavily when taking its ninth graders out of the picture. 

![Perf_Before](https://user-images.githubusercontent.com/46951897/126098156-8400817e-2fd9-42e2-942d-baf10c16e170.JPG)

![Perf_After](https://user-images.githubusercontent.com/46951897/126098167-f9b4b3a0-28a4-4049-bf7d-7696188a9fc3.JPG)

Compared to its rival charter schools, Thomas goes from being 2nd place prior to the exclusion of its ninth graders to 8th place, which is last place amongst the charter schools.

![Total_Perf_Before](https://user-images.githubusercontent.com/46951897/126101378-de73a363-3c12-4e4f-94fb-e4ba81fb95fa.JPG)

![Total_Perf_After](https://user-images.githubusercontent.com/46951897/126101388-0e2021ef-a7e5-4ef2-afe1-6beba7a784fe.JPG)

To display this DataFrame, I used the following code:

```
# Sort and show top five schools.
top_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending = False)
top_schools
```

#### How does replacing the ninth-grade scores affect the following:
  - Math and reading scores by grade: The grades are relatively unaffected since only the ninth graders for Thomas High School are excluded.
  - Scores by school spending:The only score affected in this category is the $630-$644 per student spending bin. The changes observable are slight decreases within the Average Math Score, Average Reading Score, and % Passing Math; but larger changes are observed in % Passing Reading and % Overall Passing where post-exclusion of the ninth graders results in a 0.08% drop in both figures, compared to the 0.01% decrease in the others.

![Spend_Before](https://user-images.githubusercontent.com/46951897/126099282-f8508d0c-2083-477f-b3ec-c1aad3869fa9.JPG)

![Spend_After](https://user-images.githubusercontent.com/46951897/126099299-88693fdc-7dac-4eb2-b4bb-e9ea8cc7cda1.JPG)

  - Scores by school size: Thomas falls within the medium sized school category, which experiences little changes within Average Math Score, Average Reading Score, and % Passing Math; less than 0.01% decrease. However, within % Passing Reading and % Overall Passing, post-exclusion results in an approximate 0.06% decrease. 

![Scho_Size_Before](https://user-images.githubusercontent.com/46951897/126099604-9e05b173-4295-4539-a863-a269e9da4880.JPG)

![Scho_Size_After](https://user-images.githubusercontent.com/46951897/126099611-21756dbe-4f2c-47ac-a90b-49a4867d9ee7.JPG)

  - Scores by school type: Within the School Type DataFrame, there are similar changes as observed in the previous DataFrames where Average Math Score, Average Reading Score, and % Passing Math experienced less than a 0.02% decrease post-exclusion. % Passing Reading and % Overall Passing experienced 0.03% and 0.04% decrease, respectively.

![Sco_Type_Before](https://user-images.githubusercontent.com/46951897/126100499-f1dab483-38dc-42fa-844c-d053a19a6ba2.JPG)

![Sco_Type_After](https://user-images.githubusercontent.com/46951897/126100504-be91182f-fb35-427e-b709-485f4b868d4c.JPG)

Summary: Summarize four changes in the updated school district analysis after reading and math scores for the ninth grade at Thomas High School have been replaced with NaNs.

Deliverable 3 Requirements
Structure, Organization, and Formatting (7 points)
The written analysis has the following structure, organization, and formatting:

There is a title, and there are multiple sections (2 pt).
Each section has a heading and subheading (3 pt).
Links to images are working, and code is formatted and displayed correctly (2 pt).
Analysis (18 points)
The written analysis has the following:

Overview of the school district analysis:

The purpose of this analysis is well defined (3 pt).
Results:

There is a bulleted list that addresses how each of the seven school district metrics was affected by the changes in the data (10 pt).
Summary:

There is a statement summarizing four changes to the school district analysis after reading and math scores have been replaced (5 pt).
