# data

Place data file(s) in this folder.

Then, include codebooks (variables, and their descriptions) for your data file(s)
using the following format.


## Postpartum_renamed_variables_US


- `respondent`: Refers to a single survey respondent, including the date and time survey was taken.
- `state`: Which US state or country did you give birth in? (open-ended answer)
- `age`: What was your age when you gave birth? (open-ended answer)
- `birth_location`: Where did you give birth? (multi-answer multiple choice)
- `informed_by`: When you were pregnant, did your care provider give you information about postpartum care services? (single-answer multiple choice)
- `other_info_sources`: Did you learn about postpartum care services from any other sources? (multi-answer multiple choice)
- `support_type`: The question asked was "What types of support did you have access to postpartum?" (multi-answer multiple choice)
- `provider`: Who provided this support for you? (multi-answer multiple choice)
- `ins_covered_services`: What postpartum care services did your insurance cover? (open-ended answer)
- `cost_factor`: Was cost a factor in how much support you received? (single-answer multiple choice)
- `if_insurance`: If insurance covered postpartum care services, would you take advantage of it? (single-answer multiple choice)
- `critical_support`: What support was most critical to you/your household in first year following birth? (open-ended answer)
- `ideal_support`: In a best case scenario, list all types of care and support you would need or want in the first year postpartum. (open-ended answer) 
- `comments`: Is there anything else you want to share about your postpartum experience? (open-ended answer)
- `emails`: Survey participants were given the option to include their email if they were interested in answering follow up questions or wanted the results from the original research project.

Rows: 784
Columns: 15

[1] "respondant"           "state"                "age"                  "birth_location"      
[5] "informed_by"          "other_info_sources"   "support_type"         "provider"            
[9] "ins_covered_services" "cost_factor"          "if_insurance"         "critical_support"    
[13] "ideal_support"        "comments"             "emails"

Glimpse():

$ respondant           <dttm> 2023-02-05 23:57:39, 2023-02-06 09:57:13, 2023-02-06 11:54:12, 2023-02-06 21:04:…

$ state                <chr> "California", "Pennsylvania", "Michigan", "Massachusetts", "DC", "Maryland", "Sou…

$ age                  <chr> "30.0", "26.0", "21.0", "36 and 38", "36.0", "40 (also gave birth at 35, outside …

$ birth_location       <chr> "At home", "Hospital, Was supposed to go to birth center", "At home", "Hospital",…

$ informed_by          <chr> "Yes", "No", "No", "Yes", "No", "Yes", "No", "No", "Yes", "No", "Yes", "Yes", "Ye…

$ other_info_sources   <chr> "Birth education programs, Family members or friends, Social media, Books", "Soci…

$ support_type         <chr> "Lactation support, Emotional support, In-home help with tasks like laundry, cook…

$ provider             <chr> "Midwife, Bodyworker, chiropractor, etc., Your partner/spouse, Family members, Cl…

$ ins_covered_services <chr> "The same as while I was pregnant; chiro, normal visits", "Follow up appointments…

$ cost_factor          <chr> "No", "Yes", "I received care from friends and family, had I not had that I would…

$ if_insurance         <chr> "Yes", "Yes", "If I was in a position where I needed it, yes!", "Only if needed",…

$ critical_support     <chr> "Help with little things so I can nap while baby naps", "Help with meals and hous…

$ ideal_support        <chr> "Chiro, massage, a portion of hours per week of nanny coverage, gym, etc", "Chiro…

$ comments             <chr> "Gradually things became easier, we are at 11.5 m and I’m feeling better at it al…

$ emails               <chr> NA, NA, NA, NA, NA, "rvaurio@gmail.com", NA, NA, "Thecolongirl@gmail.com", NA, "j…
