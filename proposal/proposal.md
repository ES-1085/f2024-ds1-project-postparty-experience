Project proposal
================
Postparty Experience - Willow, Tikyra, & Emmy

``` r
library(tidyverse)
library(broom)
library(readxl)
Postpartum_Experience_Survey_Responses_raw_data <- read_excel("../data/Postpartum Experience Survey (Responses) - raw data.xlsx")
#View(Postpartum_Experience_Survey_Responses_raw_data)
```

### 1. Introduction

The postpartum experience survey was created by Willow Gibson for a
research project assigned in an undergraduate course titled “History of
Midwifery and Women’s Healthcare in the US” at College of the Atlantic
in 2022. The survey was used to collect data about the postpartum
experiences of anyone who gave birth between 2013 and 2023. The purpose
of collecting this information was to observe the state of postpartum
care in the US with aim of learning what gaps in care exist.

Data was collected to through a Google form survey that was circulated
via email and briefly on social media. This dataset contains 788 cases
and 15 variables. One variable (`emails`) is not being included in our
analysis as it refers only to the email addresses of survey takers that
wished to learn more following analysis of the data.

We expect to add one or two additional variables to our dataset through
the process of mutating the types of postpartum care into quality
categories: good, better, and best. Definitions for these categories
will be determined as we work to analyze our data. While we recognize
that these definitions may be subjective in nature, we will be using
published references to help us quantify what types of support are most
fundamental to long term health vs what types of care are more optional.

For this project, our reserach question concerns the state of postpartum
care in the US. We want to look more deeply at several of the
relationships between variables in this dataset to evaluate some key
aspects of the postpartum experience. Some of the questions we hope to
explore are:

1.  If we categorize postpartum care into “good”, “better”, and “best”
    categories, what quality of postpartum care is the average birther
    receiving?

2.  Does quality of postpartum care vary from state to state?

3.  Is there a correlation between age and quality of postpartum care
    received/offered?

4.  What states require postpartum care to be covered by insurance? Does
    that requirement for coverage extend to Medicare recipients only, or
    does it extend to private insurance companies?

5.  What types of postpartum support did respondents say were most
    critical to their household within the first year following birth?
    Are these types of care typically covered by insurance or are
    families paying for these services out of pocket?

6.  Critical care vs ideal care - what are the differences?

### 2. Data

``` r
Postpartum_Experience_renamed_variables <- as_tibble(Postpartum_Experience_Survey_Responses_raw_data) %>% 
  rename( 
         `respondent` = `Timestamp`,
         `state` = `Which US state or country did you give birth in?`,
          `age` = `What was your age when you gave birth?`, 
      `birth_location` = `Where did you give birth?`,
        `informed_by` = `When you were pregnant, did your care provider give you information about postpartum care services?`,
    `other_info_sources` = `Did you learn about postpartum care services from any other sources?`, 
      `support_type`= `What types of support did you have access to postpartum?`,
         `provider` = `Who provided this support for you?`,
    `ins_covered_services` = `What postpartum care services did your insurance cover?`,
    `cost_factor` = `Was cost a factor in how much support you received?`,
        `if_insurance` = `If insurance covered postpartum care services, would you take advantage of it?`,
    `critical_support` = `What support was most critical to you/your household in first year following birth?`,
    `ideal_support` = `In a best case scenario, list all types of care and support you would need or want in the first year postpartum.`,
    `comments` = `Is there anything else you want to share about your postpartum experience?`,
    `emails` = `If you are open to follow up questions or want the results from this research project, please enter your email below.`)
```

``` r
Postpartum_renamed_variables_US <- Postpartum_Experience_renamed_variables %>%
  filter(state != "Uk") %>%
  filter(state != "Norway") %>%
  filter(state != "Puerto Rico")
```

``` r
dim(Postpartum_renamed_variables_US)
```

    ## [1] 784  15

Variables: 15

``` r
glimpse(Postpartum_renamed_variables_US)
```

    ## Rows: 784
    ## Columns: 15
    ## $ respondent           <dttm> 2023-02-05 23:57:39, 2023-02-06 09:57:13, 2023-0…
    ## $ state                <chr> "California", "Pennsylvania", "Michigan", "Massac…
    ## $ age                  <chr> "30.0", "26.0", "21.0", "36 and 38", "36.0", "40 …
    ## $ birth_location       <chr> "At home", "Hospital, Was supposed to go to birth…
    ## $ informed_by          <chr> "Yes", "No", "No", "Yes", "No", "Yes", "No", "No"…
    ## $ other_info_sources   <chr> "Birth education programs, Family members or frie…
    ## $ support_type         <chr> "Lactation support, Emotional support, In-home he…
    ## $ provider             <chr> "Midwife, Bodyworker, chiropractor, etc., Your pa…
    ## $ ins_covered_services <chr> "The same as while I was pregnant; chiro, normal …
    ## $ cost_factor          <chr> "No", "Yes", "I received care from friends and fa…
    ## $ if_insurance         <chr> "Yes", "Yes", "If I was in a position where I nee…
    ## $ critical_support     <chr> "Help with little things so I can nap while baby …
    ## $ ideal_support        <chr> "Chiro, massage, a portion of hours per week of n…
    ## $ comments             <chr> "Gradually things became easier, we are at 11.5 m…
    ## $ emails               <chr> NA, NA, NA, NA, NA, "rvaurio@gmail.com", NA, NA, …

### 3. Data analysis plan

The variables we will be visualizing to explore the research questions
include (the numbers reference the questions listed in the
introduction):

1.  `respondent` and `quality`. The `quality` variable will be created
    through defining the good, better, and best categories of care.
2.  `age` and `quality`.
3.  `state` and `quality`.
4.  `ins_covered_services` plus data from additional source(s)
5.  `critical_support` , `respondent` , `ins_covered_services` , and
    `cost_factor` We may also want to create a new variable called
    `out_of_pocket` to quantify what services were being paid for
    outside of insurance coverage.
6.  `critical_care` and `ideal_care`.

Other data needed:

- State by state postpartum insurance coverage requirements

- What factors are most important in postpartum care according to other
  sources (i.e. what services are equated with the best long term health
  outcomes)

- Distribution of quality of postpartum care across states

- Possible data source: Guarnizo, Tomás. “Doula Services in Medicaid:
  State Progress in 2022.” Center For Children and Families, Georgetown
  University Health Policy Institute, 2 June 2022,
  <https://ccf.georgetown.edu/2022/06/02/doula-services-in-medicaid-state-progress-in-2022>.

Types of graphs we may want to use:

- Pie chart - to show the distribution of quality of care within the
  data.

- Histograms - to compare experiences depending on `birth_location`

- More histograms - to show how different age groups experienced their
  care, filling with color for quality of care.

- Bubble plot - to juxtapose `critical_support` and
  `ins_covered_services`

- Density plots - to compare how experiences vary with age

- Wordclouds - to visualize the repetitions within our qualitative data

- Maps - to compare results between US states

- Waffle plot - comparing `ideal_support` with `critical_support`

## Preliminary exploratory data analysis and cleaning strategies:

Visualization of filtered age distribution in California

``` r
library(stringr)
```

``` r
#Postpartum_renamed_variables_US <- #Postpartum_renamed_variables_US |>
#  mutate(age = as.numeric(age))
```

``` r
age_of_first_birth <- Postpartum_renamed_variables_US %>% 
  mutate(age_of_first_birth = substr(age, start = 1, stop = 2)) %>% 
  mutate(age_of_first_birth = if_else(age_of_first_birth == "On", "35", age_of_first_birth)) %>% 
  mutate(age_of_first_birth = as.numeric(age_of_first_birth)) %>% 
arrange(age_of_first_birth)
```

``` r
yes_cost_a_factor <- c("I received care from friends and family, had I not had that I wouldn’t have been able to afford to pay for postpartum care services","To some extent, but luckily I could afford almost whatever I wanted/needed.","We strongly considered going to a birth center for my first birth, but they did not accept insurance and it was cost prohibitive. I feel we would have had more postpartum support with my first if we had gone that route.","Had already paid max out of pocket w labor","No referral","lactation specialist not covered, couldn’t afford otherwise","I definitely knew to look for support covered by insurance.","Did on seek in home help due to cost", "Did on seek in home help due to cost","Partially. Had i known there was support, I probably couldnt afford it.", "Yes but we had a generous budget and family to help financially","Unsure of question.... did it cost,  some of it did", "It was definitely a luxury to afford therapist and lactation help but anything other than that I viewed as “extra.”", "No doula $", "Yes but not as much as just not knowing it was available", "If I could afford food delivery, laundry service, etc, I would have hired the help!", "Do not have insurance","Cost to an extent but also Covid played a role- birth in December 2019, brought son home from NICU in 2020 and recovery was long into 2020)", "Copays sometimes.", "Yes in terms of acupuncture etc I didn’t budget for it and wish I did", "My husband and I prioritized Postpartum care highly so yes and no")

cost_not_a_factor <- c("It was more not knowing what services I should have had access to. I know more know with Instagram follows.", "No. I just was unaware of the services that might be helpful. If I had another kid I’d invest in more.", "Covid was, not cost", "Not since we hit our deductible", "Main factor was pandemic. 6 week was nearly canceled, counseling went virtual but took months to initiate")

cost_not_applicable <- c("Did not know post partum services other than the follow up appointment were covered services", "Didn’t even know what type of support was available because no one told me!","The pandemic significantly impacted my lack of access to postpartum support, especially in-person support", "I don't know, I wasn't aware of many services")


#Postpartum_Experience_renamed_variables_US %>% 
 # mutate(cost_factor = case_when(cost_factor %in% yes_cost_a_factor ~ "Yes",
                                     # cost_factor %in% cost_not_a_factor ~ "No",
                                     # cost_factor %in% cost_not_applicable ~ "N/A",
       #   TRUE ~ cost_factor))
```

``` r
hospital_cleanup <- data.frame(birth_location = Postpartum_renamed_variables_US$birth_location) %>% 
  
     mutate(birth_location = ifelse(birth_location == "Hospital, Was supposed to go to birth center", "Hospital", birth_location)) %>%  
  
mutate(birth_location = ifelse(birth_location == "At home", "Non-Hospital", birth_location)) %>% 
  
       mutate(birth_location = ifelse(birth_location == "Birthing Center attached to hospital", "Non-Hospital", birth_location)) %>% 
  
    mutate(birth_location = ifelse(birth_location == "Free standing birth center", "Non-Hospital", birth_location)) %>% 
  
       mutate(birth_location = ifelse(birth_location == "At home, I’m our car in an empty church parking lot", "Non-Hospital", birth_location)) %>% 
  
       mutate(birth_location = ifelse(birth_location == "Began at a free standing birth center, had to transfer to a midwife center at a hospital", "Hospital", birth_location)) %>% 
  
  mutate(birth_location = ifelse(birth_location == "Birthing center attached to a hospital", "Non-Hospital", birth_location)) %>% 
  
  mutate(birth_location = ifelse(birth_location == "Birthing Center attached to a hospital", "Non-Hospital", birth_location)) %>% 
  
  mutate(birth_location = ifelse(birth_location == "Free standing birth center, At home", "Non-Hospital", birth_location)) %>% 
  
  mutate(birth_location = ifelse(birth_location == "Birthing center within a hospital", "Hospital", birth_location))
```

``` r
Postpartum_renamed_variables_US <- Postpartum_renamed_variables_US |>
  mutate(state = str_replace(state, "CO", "Colorado")) |>
  mutate(state = str_replace(state, "IA", "Iowa")) |>
  mutate(state = str_replace(state, "DC", "District of Columbia")) |>
  mutate(state = str_replace(state, "TN", "Tennessee")) |>
  mutate(state = str_replace(state, "MS", "Mississippi")) |>
  mutate(state = str_replace(state, "Mass", "Massachusetts")) |>
  mutate(state = str_replace(state, "Massachusettsachusetts", "Massachusetts")) |>
  mutate(state = str_replace(state, "RI", "Rhode Island")) |>
  mutate(state = str_replace(state, "NY", "New York")) |>
  mutate(state = str_replace(state, "Tennesee", "Tennessee")) |>

  #In order to correct specific mistakes/misspellings etc across the data frame.
#using stringr package  

  filter(state != "United States",
         state != "US",
         state != "USA")
```

``` r
#which(Postpartum_renamed_variables_US$age > 100)

#`which` for identifying rows of a dataset.
```

``` r
#Postpartum_renamed_variables_US %>%
#  filter(state == "California") %>%
#  ggplot(aes(x = age)) +
#  geom_histogram() +
#  xlim(0,50) #Additional outliers and NAs ignored for the sake of the visualization (still present in data frame)
```

Example of table:

``` r
Postpartum_renamed_variables_US |>
  count(informed_by) |>
  arrange(desc(n))
```

    ## # A tibble: 44 × 2
    ##    informed_by                                                                 n
    ##    <chr>                                                                   <int>
    ##  1 No                                                                        438
    ##  2 Yes                                                                       281
    ##  3 Some information                                                            2
    ##  4 Apart from the follow up at 6 weeks no                                      1
    ##  5 As defined by?                                                              1
    ##  6 Baby 1/2 MDs-no, baby 3 midwife, yes                                        1
    ##  7 Breastfeeding groups                                                        1
    ##  8 CO- no. IA- yes.                                                            1
    ##  9 Don’t recall but I don’t think so                                           1
    ## 10 I actually can’t remember, if they did, it was instructions for self c…     1
    ## # ℹ 34 more rows

`Age` clean up:

``` r
Postpartum_by_child <- Postpartum_renamed_variables_US |> 
  mutate(birth_age_year = str_replace_all(age, "\\d{4}", replacement = "")) |>
  mutate(birth_age = map_chr(str_extract_all(birth_age_year, "\\d{2}"),  ~ str_c(.x, collapse = ","))) |> 
   separate_longer_delim(birth_age, delim = ",") # this line of the code makes each birth age a separate entry in the spreadsheet
```

Map Draft

``` r
#library(maps)
#library(mapproj)
#library(tidyverse)

#states <- map_data("state")
#states <- rename(states, state = region)
#statessurveyed <- `states`

#Postpartum_renamed_variables_US <- #Postpartum_renamed_variables_US %>% 
# mutate(state = tolower(Postpartum_renamed_variables_US$state))

#We will need to finish creating the good/better/best categories for the `quality` variable to determine portions of quality of care before this map can be completed.

#Use lecture material from week 6 - leaflet
```

## 4. Data Ethics Review

The data ethics review section will be introduced in a separate class
and is not part of the original proposal deadline.
