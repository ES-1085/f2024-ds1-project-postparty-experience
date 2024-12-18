Project memo
================
Postparty Experience - Tikyra, Emmy, and Willow

``` r
library(tidyverse)
library(broom)
library(readxl)
library(stringr)
library(forcats)
library(gridExtra)
library(dplyr)
library(openintro)
library(countrycode)
library(ggpattern)
library(ggplot2)
library(RColorBrewer)
library(ggridges)
library(visdat)
```

``` r
Postpartum <- read.csv("Postpartum.csv")
```

## Data Clean Up Steps for Overall Data

### Step 1: Rename variables

``` r
# This step occurred in the proposal, but we wanted to ensure all clean up steps were included in the memo.

# Postpartum_Experience_renamed_variables <- as_tibble(Postpartum_Experience_Survey_Responses_raw_data) |> 
 # rename( 
    #  `respondent` = `Timestamp`,
     # `state` = `Which US state or country did you give birth in?`,
     # `age` = `What was your age when you gave birth?`, 
     # `birth_location` = `Where did you give birth?`,
    #  `informed_by` = `When you were pregnant, did your care provider give you information about postpartum care services?`,
    #  `other_info_sources` = `Did you learn about postpartum care services from any other sources?`, 
    #  `support_type`= `What types of support did you have access to postpartum?`,
    #  `provider` = `Who provided this support for you?`,
    #  `ins_covered_services` = `What postpartum care services did your insurance cover?`,
    #  `cost_factor` = `Was cost a factor in how much support you received?`,
    # `if_insurance` = `If insurance covered postpartum care services, would you take advantage of it?`,
    # `critical_support` = `What support was most critical to you/your household in first year following birth?`,
    # `ideal_support` = `In a best case scenario, list all types of care and support you would need or want in the first year postpartum.`,
    # `comments` = `Is there anything else you want to share about your postpartum experience?`,
    # `emails` = `If you are open to follow up questions or want the results from this research project, please enter your email below.`)
```

### Step 2: Manual translation of ’critical_care\`

Each entry in `critical_care` was manually translated to match the
terminology used in the multiple choice answers listed in the
`support_type` variable. This resulted in a new “clean” version of
`critical_care` variable called `critical`. The purpose of this cleaning
step was to make the open ended answers easier to graph and compare to
other variables. Consistent language allows for clearer and more
meaningful visualizations.

``` r
# This step occurred in the proposal, but we wanted to ensure all clean up steps were included in the memo.

# Postpartum_combined <- bind_cols(Postpartum_Experience_renamed_variables, Clean_critical_care)

# head(Postpartum_combined)
```

### Step 3: Clean `age`

There were a lot of variations in the data, like varying number of
births and birth locations. We decided to only use the first birth for
each respondent by creating a new variable only containing the first
birth age.

``` r
Postpartum <- Postpartum |> 
  mutate(first_age = substr(age, start = 1, stop = 2)) |> 
  mutate(first_age = if_else(first_age == "On", "35", first_age)) |> 
  mutate(first_age = as.numeric(first_age)) |> 
  arrange(first_age) |> 

relocate(first_age, .after = age)
```

### Step 4: Clean `state`

We prepared the `state` variable to use in visualizing geographical
differences in care quality, for instance with a leaflet map. We
excluded international entries in the proposal and responses not
indicating state (e.g. USA). We also used the `stringr` package to
ensure all states be written fully and with correct spelling.

``` r
Postpartum <- Postpartum |>
  mutate(state = str_replace(state, "CO", "Colorado")) |>
  mutate(state = str_replace(state, "IA", "Iowa")) |>
  mutate(state = str_replace(state, "DC", "Washington, D.C.")) |>
  mutate(state = str_replace(state, "TN", "Tennessee")) |>
  mutate(state = str_replace(state, "MS", "Mississippi")) |>
  mutate(state = str_replace(state, "Mass", "Massachusetts")) |>
  mutate(state = str_replace(state, "Massachusettsachusetts", "Massachusetts")) |>
  mutate(state = str_replace(state, "RI", "Rhode Island")) |>
  mutate(state = str_replace(state, "NY", "New York")) |>
  mutate(state = str_replace(state, "Tennesee", "Tennessee")) |>

  filter(state != "United States",
         state != "United states",
         state != "US",
         state != "USA")
```

As we decided to only use the first birth for analysis, we excluded
state entries for the second, third birth etc.

``` r
Postpartum <- Postpartum |> 
  mutate(first_state = sub("(,|and|1|/|-|\\(|&).*", "", state)) |>
  arrange(first_state) |> 

relocate(first_state, .after = state)

Postpartum <- Postpartum |>
  mutate(first_state = str_replace(first_state, "Arizona ", "Arizona")) |>
  mutate(first_state = str_replace(first_state, "Arkansas ", "Arkansas")) |>
  mutate(first_state = str_replace(first_state, "California ", "California")) |>
  mutate(first_state = str_replace(first_state, "california ", "California")) |>
  mutate(first_state = str_replace(first_state, "california", "California")) |>
  mutate(first_state = str_replace(first_state, "Colorado ", "Colorado")) |>
  mutate(first_state = str_replace(first_state, "First", "Tennessee")) |>
  mutate(first_state = str_replace(first_state, "Tennessee ", "Tennessee")) |>
  mutate(first_state = str_replace(first_state, "Indiana ", "Indiana")) |>
  mutate(first_state = str_replace(first_state, "Kentcky", "Kentucky")) |>
  mutate(first_state = str_replace(first_state, "Massachusetts ", "Massachusetts")) |>
  mutate(first_state = str_replace(first_state, "Massachusettsechusetts", "Massachusetts")) |>
  mutate(first_state = str_replace(first_state, "Michigan ", "Michigan")) |>
  mutate(first_state = str_replace(first_state, "Minesotta", "Minnesota")) |>
  mutate(first_state = str_replace(first_state, "Minnesotta", "Minnesota")) |>
  mutate(first_state = str_replace(first_state, "New York ", "New York")) |>
  mutate(first_state = str_replace(first_state, "New york", "New York")) |>
  mutate(first_state = str_replace(first_state, "North carolina", "North Carolina")) |>
  mutate(first_state = str_replace(first_state, "Oregon ", "Oregon")) |>
  mutate(first_state = str_replace(first_state, "Rhode isl", "Rhode Island")) |>
  mutate(first_state = str_replace(first_state, "Rhode Isl", "Rhode Island")) |>
  mutate(first_state = str_replace(first_state, "Rhode Islandand", "Rhode Island")) |>
  mutate(first_state = str_replace(first_state, "Texas ", "Texas")) |>
  mutate(first_state = str_replace(first_state, "Washington ", "Washington")) |>
  mutate(first_state = str_replace(first_state, "New jersey", "New Jersey")) |>
  mutate(first_state = str_replace(first_state, "wisconsin", "Wisconsin")) |>
  mutate(first_state = str_replace(first_state, "virginia", "Virginia")) |>
  mutate(first_state = str_replace(first_state, "Virginia ", "Virginia")) |>
  mutate(first_state = str_replace(first_state, "Maryl", "Maryland")) |>
  mutate(first_state = str_replace(first_state, "North dakota", "North Dakota")) |>
  mutate(first_state = str_replace(first_state, "pennsylvania", "Pennsylvania")) 
  
 unique(Postpartum$first_state)
```

    ##  [1] "Alabama"              "Alaska"               "Arizona"             
    ##  [4] "Arkansas"             "California"           "Colorado"            
    ##  [7] "Connecticut"          "Delaware"             "District of Columbia"
    ## [10] "Tennessee"            "Florida"              "Georgia"             
    ## [13] "Hawaii"               "Illinois"             "Indiana"             
    ## [16] "Iowa"                 "Kansas"               "Kentucky"            
    ## [19] "Louisiana"            "Maine"                "Maryland"            
    ## [22] "Massachusetts"        "Michigan"             "Minnesota"           
    ## [25] "Mississippi"          "Missouri"             "Nebraska"            
    ## [28] "Nevada"               "New Hampshire"        "New Jersey"          
    ## [31] "New Mexico"           "New York"             "North Carolina"      
    ## [34] "North Dakota"         "Ohio"                 "Oklahoma"            
    ## [37] "Oregon"               "Pennsylvania"         "Rhode Island"        
    ## [40] "South Carolina"       "South Dakota"         "Texas"               
    ## [43] "Utah"                 "Vermont"              "Virginia"            
    ## [46] "Washington"           "West Virginia"        "Wisconsin"

### Step 5: Clean `support_type`

Below is an account of creating and standardizing the `support type`
variable entries.

``` r
Postpartum <- Postpartum |>
  mutate(support_type = str_replace(support_type, pattern = "In-home help with tasks like laundry, cooking, cleaning, and child care", "In-home help with care tasks")) 
```

``` r
Postpartum <- Postpartum |>
  separate_longer_delim(support_type, delim = ",")
```

``` r
  unique(Postpartum$support_type)
```

    ##  [1] "Help with meals"                       
    ##  [2] "Lactation support"                     
    ##  [3] "In-home help with care tasks"          
    ##  [4] "Hospital/office follow up appointments"
    ##  [5] "Emotional support"                     
    ##  [6] "New parent groups"                     
    ##  [7] "Massage or chiropractic"               
    ##  [8] "Pelvic floor PT"                       
    ##  [9] "In-home follow up appointments"        
    ## [10] "Acupuncture"                           
    ## [11] "None of the above"                     
    ## [12] "Family support"                        
    ## [13] "Other"                                 
    ## [14] NA                                      
    ## [15] "Overnight help"

``` r
Postpartum <- Postpartum|>
  mutate(support_type = ifelse(support_type == " Mommy and me yoga and classes", "Other", support_type)) |>
  mutate(support_type = ifelse(support_type == " follow-up appointment with midwife; overnight doula (1 night per week)", "In-home wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " we didn’t have anyone else and I often wished we had someone to hold the both of us while we bonded with our baby.", "None of the above", support_type)) |>
  mutate(support_type = ifelse(support_type == " My partner was incredibly helpful. He handled all of the emotional support and meal and house prep and cleaning. However", "Other", support_type)) |>  
  mutate(support_type = ifelse(support_type == " any supports I had access to I had to find myself.", "NA", support_type)) |>
  mutate(support_type = ifelse(support_type == " As a note", "NA", support_type)) |>
  mutate(support_type = ifelse(support_type == "Yes there was a postpartum follow up but didnt seem like she did nuch other than repeat what she was reading off a pamphlet.", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |> 
  mutate(support_type = ifelse(support_type == " 1 PP appointment at 6 weeks", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Virtual postpartum follow up visits due to COVID", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " In—home baby education", "Other", support_type)) |>
  mutate(support_type = ifelse(support_type == " Therapy (for PPD with second kid)", "Emotional support", support_type)) |>
  mutate(support_type = ifelse(support_type == " Postpartum doula", "In-home wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Home visiting (Maine Families and public health nursing)", "In-home wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " I didn't really receive support. I know my pediatrician has lactation services but I didn't need.", "Lactation support", support_type)) |>
  mutate(support_type = ifelse(support_type == " Besides my required postpartum checkup and family care", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Some of these were offered but not helpful. Saw a behaviorist bc my dad died right before birth but they just said I “seemed fine” and were no help", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Ovia Parenting App coaches", "Other", support_type)) |>
  mutate(support_type = ifelse(support_type == " Baby was in NICU so we also had access to a social worker and classes with fellow nicu parents on development/feeding/etc", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Lactation support pre-covid. Post covid nothing in hospital", "Lactation support", support_type)) |>
  mutate(support_type = ifelse(support_type == " Stay at home spouse", "Other", support_type)) |>
  mutate(support_type = ifelse(support_type == " Lactation support was virtual only due to pandemic", "Lactation support", support_type)) |>
  mutate(support_type = ifelse(support_type == " One postpartum appointment with my OBGYN", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == "Gave birth in 2020. Everything I thought I’d have access to was shut down", "None of the above", support_type)) |>
  mutate(support_type = ifelse(support_type == " Except for my second  was born March 2020. I had no follow up", "None of the above", support_type)) |>
  mutate(support_type = ifelse(support_type == " most of my concerns were ignored by my ob.", "None of the above", support_type)) |>
  mutate(support_type = ifelse(support_type == " the process wasn’t easy or clear. After my 6-week checkup", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Had to seek out lactation help on my own", "Lactation support", support_type)) |>
  mutate(support_type = ifelse(support_type == " Ped PT and dentist for tie release + torticollis (for LO but her care is critical to my well being + BF journey)", "Other", support_type)) |>
  mutate(support_type = ifelse(support_type == "", "NA", support_type)) |>
  mutate(support_type = ifelse(support_type == " I probably could have had access to some of the other things on this list but didn't look into it", "None of the above", support_type)) |>
  mutate(support_type = ifelse(support_type == " Doula support", "In-home wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " I had access to lactation/new parent group but didn’t have the knowledge to go to them", "Lactation support", support_type)) |>
  mutate(support_type = ifelse(support_type == " Therapy for Anxiety", "Emotional support", support_type)) |>
  mutate(support_type = ifelse(support_type == " 6 week follow up pp with doctor", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Lactation support was VERY hard to access", "Lactation support", support_type)) |>
  mutate(support_type = ifelse(support_type == " 1x nurse check-in over phone (through hospital)", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Nighttime support night nurse", "Overnight help", support_type)) |>
  mutate(support_type = ifelse(support_type == " Postpartum doula and a night doula", "In-home wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " Therapist but not specializing in postpartum concerns", "Emotional support", support_type)) |>
  mutate(support_type = ifelse(support_type == " Family", "Family support", support_type)) |>
  mutate(support_type = ifelse(support_type == " Mental health therapy", "Emotional support", support_type)) |>
  mutate(support_type = ifelse(support_type == " but the second hospital I delivered at has a robust new parent program", "New parent groups - in person", support_type)) |>
  mutate(support_type = ifelse(support_type == " I didn’t attend any of the new parent groups", "NA", support_type)) |>
  mutate(support_type = ifelse(support_type ==" I probably had access to pelvic floor therapy but didn’t know about it in order to access it.", "Pelvic floor rehabilitation", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Doula. Also", "In-home wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Emotional support", "Emotional support", support_type)) |>
  mutate(support_type = ifelse(support_type ==" None of the above", "None of the above", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Grief support", "Grief support", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Pelvic floor rehabilitation", "Pelvic floor rehabilitation", support_type)) |>
  mutate(support_type = ifelse(support_type ==" New parent groups - online", " New parent groups - online", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Acupuncture", "Acupuncture", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Massage or chiropractic", "Massage or chiropractic", support_type)) |>
  mutate(support_type = ifelse(support_type ==" New parent groups - in person", "New parent groups - in person", support_type)) |>
  mutate(support_type = ifelse(support_type ==" New parent groups - online", "New parent groups - online", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Community meal train or other meal service", "Community meal train or other meal service", support_type)) |>
  mutate(support_type = ifelse(support_type ==" Hospital or office based wellness services and postpartum follow up appointments", "Hospital or office based wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " In-home wellness services and postpartum follow up appointments", "In-home wellness services and postpartum follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == "Hospital or office based wellness services and postpartum follow up appointments", "Hospital/office follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " In-home wellness services and postpartum follow up appointments", "In-home follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == " In-home help with care tasks", "In-home help with care tasks", support_type)) |>
  mutate(support_type = ifelse(support_type == "Pelvic floor rehabilitation", "Pelvic floor PT", support_type)) |>
  mutate(support_type = ifelse(support_type == "New parent groups - in person", "New parent groups", support_type)) |>
  mutate(support_type = ifelse(support_type == "New parent groups - online", "New parent groups", support_type)) |>
  mutate(support_type = ifelse(support_type == "In-home wellness services and postpartum follow up appointments", "In-home follow up appointments", support_type)) |>
  mutate(support_type = ifelse(support_type == "Community meal train or other meal service", "Help with meals", support_type)) |> 
  mutate(support_type = ifelse(support_type == "Grief support", "Emotional support", support_type))
```

``` r
unique(Postpartum$support_type)
```

    ##  [1] "Help with meals"                       
    ##  [2] "Lactation support"                     
    ##  [3] "In-home help with care tasks"          
    ##  [4] "Hospital/office follow up appointments"
    ##  [5] "Emotional support"                     
    ##  [6] "New parent groups"                     
    ##  [7] "Massage or chiropractic"               
    ##  [8] "Pelvic floor PT"                       
    ##  [9] "In-home follow up appointments"        
    ## [10] "Acupuncture"                           
    ## [11] "None of the above"                     
    ## [12] "Family support"                        
    ## [13] "Other"                                 
    ## [14] NA                                      
    ## [15] "Overnight help"

### Step 6: Clean `birth_location`

``` r
Postpartum <- Postpartum |>
  mutate(birth_location = ifelse(birth_location == "Hospital, Was supposed to go to birth center", "Hospital", birth_location)) |>  
  mutate(birth_location = ifelse(birth_location == "At home", "Non-Hospital", birth_location)) |>
  mutate(birth_location = ifelse(birth_location == "Birthing Center attached to hospital", "Non-Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Free standing birth center", "Non-Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "At home, I’m our car in an empty church parking lot", "Non-Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Began at a free standing birth center, had to transfer to a midwife center at a hospital", "Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Birthing center attached to a hospital", "Non-Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Planned home birth, transferred to hospital", "Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Free standing birth center, At home", "Non-Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Hospital, Homebirth transfer to hospital csection", "Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Natural birthing center attached to hospital", "Non-Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Hospital based birth center (across a sky bridge)", "Non-Hospital", birth_location)) |>
  mutate(birth_location = ifelse(birth_location == "Hospital, Hospital x2", "Hospital", birth_location)) |>
  mutate(birth_location = ifelse(birth_location == "Hospital, began care at birth center all 3 times & transferred", "Hospital", birth_location)) |>
  mutate(birth_location = ifelse(birth_location == "Birthing center within a hospital", "Hospital", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Hospital, Free standing birth center", "NA", birth_location)) |> 
  mutate(birth_location = ifelse(birth_location == "Hospital, At home", "NA", birth_location)) |>
  mutate(birth_location = ifelse(birth_location ==  "Hospital, Birth center room in the hospital", "NA", birth_location)) |>
  mutate(birth_location = ifelse(birth_location ==  "Hospital, Car", "NA", birth_location))
```

### Step 7: Start cleaning `cost_factor`

``` r
yes_cost_a_factor <- c("I received care from friends and family, had I not had that I wouldn’t have been able to afford to pay for postpartum care services","To some extent, but luckily I could afford almost whatever I wanted/needed.","We strongly considered going to a birth center for my first birth, but they did not accept insurance and it was cost prohibitive. I feel we would have had more postpartum support with my first if we had gone that route.","Had already paid max out of pocket w labor","No referral","lactation specialist not covered, couldn’t afford otherwise","I definitely knew to look for support covered by insurance.","Did on seek in home help due to cost", "Did on seek in home help due to cost","Partially. Had i known there was support, I probably couldnt afford it.", "Yes but we had a generous budget and family to help financially","Unsure of question.... did it cost,  some of it did", "It was definitely a luxury to afford therapist and lactation help but anything other than that I viewed as “extra.”", "No doula $", "Yes but not as much as just not knowing it was available", "If I could afford food delivery, laundry service, etc, I would have hired the help!", "Do not have insurance","Cost to an extent but also Covid played a role- birth in December 2019, brought son home from NICU in 2020 and recovery was long into 2020)", "Copays sometimes.", "Yes in terms of acupuncture etc I didn’t budget for it and wish I did", "My husband and I prioritized Postpartum care highly so yes and no")

cost_not_a_factor <- c("It was more not knowing what services I should have had access to. I know more know with Instagram follows.", "No. I just was unaware of the services that might be helpful. If I had another kid I’d invest in more.", "Covid was, not cost", "Not since we hit our deductible", "Main factor was pandemic. 6 week was nearly canceled, counseling went virtual but took months to initiate")

cost_not_applicable <- c("Did not know post partum services other than the follow up appointment were covered services", "Didn’t even know what type of support was available because no one told me!","The pandemic significantly impacted my lack of access to postpartum support, especially in-person support", "I don't know, I wasn't aware of many services")


#Postpartum_Experience_renamed_variables_US |> 
 # mutate(cost_factor = case_when(cost_factor %in% yes_cost_a_factor ~ "Yes",
                                     # cost_factor %in% cost_not_a_factor ~ "No",
                                     # cost_factor %in% cost_not_applicable ~ "N/A",
       #   TRUE ~ cost_factor))
```

## Plots

### Plot 1: Bar chart

Incorporating feedback from the plot critique, we made several versions
of the bar chart displaying different selections of support types to
make the charts more readable. We used colour to highlight certain
support type categories.

``` r
Postpartum |>
  filter(!is.na(support_type)) |>
  filter(support_type != "New parent groups - in person",
         support_type != "NA",
         support_type != "Overnight help",
         support_type != "Family support",
         support_type != "New parent groups",
         support_type != "In-home help with care tasks",
         support_type != "Other",
         support_type != "Help with meals") |>
  ggplot(mapping = aes(x = fct_rev(fct_infreq(support_type)), fill = support_type)) +
  geom_bar() +
  scale_fill_manual(
    values = c(
      "In-home follow up appointments" = "#d95f02",
      "Massage or chiropractic" = "#d95f02",
      "Acupuncture" = "#d95f02")
    ) +
  theme_bw() +
  theme_ridges() +
  theme(legend.position = 'none') + 
  coord_flip() +
  labs(title = "Formal types of Postpartum Care Accessed", 
       subtitle = "In the US Between 2013-2023 by Survey Respondents", 
       x = "Support Type", 
       y = "Number of Survey Respondents",
       caption = "The highlighted care types have been defined as care that increases the birthing parent and baby’s physical and emotional wellbeing beyond baseline services."
       ) 
```

![](memo_files/figure-gfm/draft-formal-care-type-bar-chart-1.png)<!-- -->

``` r
#ggsave("example-postpartum-wide-2.png", width = 10, height = 4)
```

``` r
Postpartum |>
  filter(!is.na(support_type)) |>
  filter(support_type != "New parent groups - in person",
         support_type != "NA",
         support_type != "Overnight help",
         support_type != "Family support",
         support_type != "New parent groups",
         support_type != "In-home help with care tasks",
         support_type != "Other",
         support_type != "Help with meals") |>
  ggplot(mapping = aes(x = fct_rev(fct_infreq(support_type)), fill = support_type)) +
  geom_bar() +
  scale_fill_manual(
    values = c(
      "Lactation support" = "gold",
      "Emotional support" = "gold",
      "Pelvic floor PT" = "gold",
      "Hospital/office follow up appointments" = "gold")
    ) +
  theme_bw() +
  theme_ridges() +
  theme(legend.position = 'none') + 
  coord_flip() +
  labs(title = "Formal types of Postpartum Care Accessed", 
       subtitle = "In the US Between 2013-2023 by Survey Respondents", 
       x = "Support Type", 
       y = "Number of Survey Respondents",
       caption = "The highlighted care types have been defined as the baseline care needed to prevent worst long term
       health outcomes for the birthing parent and baby."
       ) 
```

![](memo_files/figure-gfm/draft-V2-formal-care-type-bar-chart-1.png)<!-- -->

``` r
#ggsave("example-postpartum-wide-3.png", width = 10, height = 4)
```

We based the bar chart on percentage of respondents instead of
frequency. Knowing the number of total respondents (in the US) made it
easier to calculate percentage based on this set number rather than
having code do the full calculation.

``` r
Postpartum |>
  drop_na(support_type) |>
  filter(support_type != "Lactation support",
         support_type != "Hospital/office follow up appointments",
         support_type != "Emotional support",
         support_type != "Pelvic floor PT",
         support_type != "Massage or chiropractic",
         support_type != "In-home follow up appointments",
         support_type != "Acupuncture",
         support_type != "None of the above") |>
  select(respondent, support_type) |>
  count(support_type) |>
  mutate(percentage = n / 784 *100) |>
  ggplot(mapping = aes(x = fct_rev(fct_infreq(support_type)), y = percentage, fill = support_type)) +
  geom_col() +
  theme_bw() +
  theme_ridges() +
  theme(legend.position = 'none') + 
  coord_flip() +
  labs(title = "Informal types of Postpartum Care Accessed", 
       subtitle = "In the US Between 2013-2023 by Survey Respondents", 
       x = "Support Type", 
       y = "Percentage of Survey Respondents",
     #  caption = "The highlighted care types have been defined as care that increases the birthing parent and baby’s  physical and emotional wellbeing beyond baseline services."
       ) 
```

![](memo_files/figure-gfm/draft-informal-care-type-bar-chart-1.png)<!-- -->

### Plot 2: Leaflet map

#### Data cleanup steps specific to plot 2

``` r
Postpartum <- Postpartum |>
  mutate(quality_critical = case_when(
    critical %in% c("Lactation support", "Pelvic floor PT", "Emotional support", 
                    "Hospital/office follow up appointments", "Unpaid parental leave") ~                      "baseline support",
    critical %in% c("In-home follow up appointments", 
                    "In-home help with care tasks", "Massage or chiropractic", 
                    "Acupuncture", "Overnight help") ~ "physical support",
    critical %in% c("Help with meals", "New parent groups", "Paid parental leave", 
                    "Family support") ~ "community support",
    TRUE ~ NA_character_
  ))

unique(Postpartum$quality_critical)
```

    ## [1] "community support" "physical support"  "baseline support" 
    ## [4] NA

``` r
Postpartum <- relocate(Postpartum, quality_critical, .after = critical)
```

``` r
Postpartum <- Postpartum |>
  mutate(quality_access = case_when(
    support_type %in% c("Lactation support", "Pelvic floor PT", "Emotional support", 
                    "Hospital/office follow up appointments", "Unpaid parental leave") ~                      "baseline support",
    support_type %in% c("In-home follow up appointments", 
                    "In-home help with care tasks", "Massage or chiropractic", 
                    "Acupuncture", "Overnight help") ~ "physical support",
    support_type %in% c("Help with meals", "New parent groups", "Paid parental leave", 
                    "Family support") ~ "community support",
    TRUE ~ NA_character_
  ))

unique(Postpartum$quality_access)
```

    ## [1] "community support" "baseline support"  "physical support" 
    ## [4] NA

``` r
Postpartum <- relocate(Postpartum, quality_access, .after = support_type)
```

``` r
Postpartum <- Postpartum |>
  mutate(regions = case_when(
    first_state %in% c("Connecticut", "Maine", "Massachusetts", "New Hampshire", 
                       "New Jersey", "New York", "Pennsylvania", "Rhode Island",
                       "Vermont", "Delaware") ~ "Northeast",
    first_state %in% c("Illinois", "Indiana", "Iowa", "Kansas", "Michigan", "Minnesota",
                       "Missouri", "Nebraska", "North Dakota", "Ohio", "South Dakota",
                       "Wisconsin") ~ "Midwest",
    first_state %in% c("Alabama", "Arkansas", "Florida", "Georgia", "Kentucky",
                       "Louisiana", "Maryland", "Mississippi", "North Carolina", 
                       "South Carolina", "Tennessee", "Virginia", "West Virginia", 
                       "Washington, D.C.") ~ "Southeast",
    first_state %in% c("Texas", "Oklahoma", "Arizona", "New Mexico") ~ "Southwest",
    first_state %in% c("Alaska", "California", "Colorado", "Hawaii", "Idaho", "Montana",
                       "Nevada", "Utah", "Wyoming") ~ "West",
    first_state %in% c("Oregon", "Washington", "Idaho") ~ "Northwest",
    TRUE ~ NA_character_
  ))

unique(Postpartum$regions)
```

    ## [1] "Southeast" "West"      "Southwest" "Northeast" NA          "Midwest"  
    ## [7] "Northwest"

To make `regions` useful with a leaflet map, we would have assigned each
region coordinates (multipolygon).

``` r
Postpartum <- relocate(Postpartum, regions, .after = first_state)
```

``` r
#library(leaflet) ## For leaflet interactive maps
#library(sf) ## For spatial data
#library(RColorBrewer) ## For colour palettes
#library(htmltools) ## For html
#library(leafsync) ## For placing plots side by side
#library(kableExtra) ## Table  output (in slides)
#library(maps)
#library(mapdata)
#library(tigris)
#library(ggiraph) 
#install.packages('ggiraph')

# states <- states(cb = TRUE)

# leaflet(states) |>
 # addProviderTiles(providers$OpenStreetMap) |>
  #addProviderTiles(providers$georegion) |>
 # addPolygons()
  
#lines above make base map
  
 # addProviderTiles(provider = providers$OpenStreetMap) |>
 # setView(lng = ,
 #         lat = ,
  #        zoom = 5) |>
 # addPolygons(

#Postpartum |>
#ggplot(state_stats) +
 # geom_sf(aes(fill = "regions")) +
  #labs(title = "map",
   #    x = "Longitude",
    #   y = "Latitude",
     #  fill = "regions")

#Above is an outline of our idea for the leaflet
```

``` r
#map('state', fill = TRUE, col = palette())
```

### Plot 3 Clean up

## Cleaning `critical`

``` r
# Postpartum <- rename(Postpartum, "critical" = "critical_clean")
```

``` r
Postpartum <- relocate(Postpartum, critical, .after = critical_support)
```

``` r
Postpartum <- Postpartum |>
   separate_longer_delim(critical, delim = ",")
```

``` r
unique(Postpartum$critical)
```

    ##  [1] "Help with meals"                       
    ##  [2] "In-home help with care tasks"          
    ##  [3] "Lactation support"                     
    ##  [4] "Unpaid parental leave"                 
    ##  [5] "Pelvic floor PT"                       
    ##  [6] "Emotional support"                     
    ##  [7] "Family support"                        
    ##  [8] "New parent groups"                     
    ##  [9] "Hospital/office follow up appointments"
    ## [10] "Other"                                 
    ## [11] "Overnight help"                        
    ## [12] "In-home follow up appointments"        
    ## [13] "Paid parental leave"                   
    ## [14] "Massage or chiropractic"               
    ## [15] "Acupuncture"

``` r
Postpartum <- Postpartum |>
  mutate(critical = ifelse(critical == " In-home help with care tasks", "In-home help with care tasks", critical)) |>
  mutate(critical = ifelse(critical == " Lactation support", "Lactation support", critical)) |>
  mutate(critical = ifelse(critical ==  " Pelvic floor PT", "Pelvic floor PT", critical)) |>
  mutate(critical = ifelse(critical ==  " Emotional support", "Emotional support", critical)) |>
  mutate(critical = ifelse(critical ==  " Help with meals", "Help with meals", critical)) |>
  mutate(critical = ifelse(critical ==  " Family support", "Family support", critical)) |>
  mutate(critical = ifelse(critical ==  " Other", "Other", critical)) |>
  mutate(critical = ifelse(critical ==  " New parent groups", "New parent groups", critical)) |>
  mutate(critical = ifelse(critical ==  " In-home follow up appointments", "In-home follow up appointments", critical)) |>
  mutate(critical = ifelse(critical ==  " Paid parental leave", "Paid parental leave", critical)) |>
  mutate(critical = ifelse(critical ==  " Overnight help", "Overnight help", critical)) |>
  mutate(critical = ifelse(critical ==  " Massage or chiropractic", "Massage or chiropractic", critical)) |>
  mutate(critical = ifelse(critical ==  " Acupuncture", "Acupuncture", critical)) |>
  mutate(critical = ifelse(critical ==  " family support", "Family support", critical)) |>
  mutate(critical = ifelse(critical ==  " Hospital/office follow up appointments", "Hospital/office follow up appointments", critical)) |>
  mutate(critical = ifelse(critical ==  " New parent group", "New parent group", critical)) |>
  mutate(critical = ifelse(critical ==  "Pelvice floor PT", "Pelvic floor PT", critical)) |>
  mutate(critical = ifelse(critical ==  " Pelvic floor rehab", "Pelvic floor PT", critical)) |>
  mutate(critical = ifelse(critical ==  " Unpaid parental leave", "Unpaid parental leave", critical)) |>
  mutate(critical = ifelse(critical ==  " in-home help with care tasks", "In-home help with care tasks", critical)) |>
  mutate(critical = ifelse(critical ==  "Pelvic floor rehab", "Pelvic floor PT", critical)) |>
  mutate(critical = ifelse(critical ==  "Emotial support", "Emotional support", critical)) |>
  mutate(critical = ifelse(critical ==  "Help with meal", "Help with meals", critical)) |>
  mutate(critical = ifelse(critical ==  "Lactation", "Lactation support", critical)) |>
  mutate(critical = ifelse(critical ==  "Lactatiom support", "Lactation support", critical)) |>
  mutate(critical = ifelse(critical ==  "Emototional support", "Emotional support", critical)) |>
  mutate(critical = ifelse(critical ==  "In-home help with child care", "In-home help with care tasks", critical)) |>
  mutate(critical = ifelse(critical ==  "In-home help wih care tasks", "In-home help with care tasks", critical)) |>
  mutate(critical = ifelse(critical ==  "Help with care tasks", "In-home help with care tasks", critical)) |>
  mutate(critical = ifelse(critical ==  "New parent group", "New parent groups", critical)) |>
  mutate(critical = ifelse(critical ==  "Child care", "In-home help with care tasks", critical)) |>
  mutate(critical = ifelse(critical ==  "Lacation support", "Lactation support", critical)) |>
  mutate(critical = ifelse(critical ==  "Sleep support", "Overnight help", critical))
```

``` r
unique(Postpartum$critical)
```

    ##  [1] "Help with meals"                       
    ##  [2] "In-home help with care tasks"          
    ##  [3] "Lactation support"                     
    ##  [4] "Unpaid parental leave"                 
    ##  [5] "Pelvic floor PT"                       
    ##  [6] "Emotional support"                     
    ##  [7] "Family support"                        
    ##  [8] "New parent groups"                     
    ##  [9] "Hospital/office follow up appointments"
    ## [10] "Other"                                 
    ## [11] "Overnight help"                        
    ## [12] "In-home follow up appointments"        
    ## [13] "Paid parental leave"                   
    ## [14] "Massage or chiropractic"               
    ## [15] "Acupuncture"

### Plot 3: Ridge plot

``` r
Postpartum |>
  count(critical) |>
  arrange(desc(n))
```

    ##                                  critical   n
    ## 1            In-home help with care tasks 941
    ## 2                       Emotional support 788
    ## 3                       Lactation support 700
    ## 4                          Family support 400
    ## 5                         Help with meals 301
    ## 6                         Pelvic floor PT 220
    ## 7                       New parent groups 178
    ## 8          In-home follow up appointments 137
    ## 9                                   Other 136
    ## 10                         Overnight help  89
    ## 11                    Paid parental leave  78
    ## 12                Massage or chiropractic  70
    ## 13 Hospital/office follow up appointments  34
    ## 14                            Acupuncture  20
    ## 15                  Unpaid parental leave  10

``` r
Postpartum |>
  filter(critical != "NA") |>
  filter(critical != "Duplicate entry") |>
  filter(!critical %in% c("Family support", "Help with meals",  "Pelvic floor PT", "New parent groups", "In-home follow up appointments", "Other", "Overnight help", "Paid parental leave", "Massage or chiropractic", "Hospital/office follow up appointments", "Acupuncture", "Unpaid parental leave")) |>
  filter(regions != "NA") |>
  ggplot(aes(x = first_age, 
           y = critical,
           color = critical, 
           fill = critical)) +
  facet_wrap(~ regions) +
  geom_density_ridges(alpha = .6) +
  scale_color_viridis_d(alpha = .6) +
  scale_fill_viridis_d(alpha = .6) +
 # scale_x_continuous(breaks = seq(from = 18, to = 44, by = 2)) +
  theme_ridges() +
  theme(legend.position = "none") +
  theme(axis.title.x = element_blank(),
       axis.text.x = element_blank(),
       axis.ticks.x = element_blank()) +
  theme(axis.title.y = element_blank()) +
  # need to re-order care types and potentially break it up by quality or formal/informal
  labs (title = "Top 3 Most Critical Postpartum Care Types", subtitle = "by Survey Respondents Across the US")
```

    ## Picking joint bandwidth of 1.03

    ## Picking joint bandwidth of 1.06

    ## Picking joint bandwidth of 1.23
    ## Picking joint bandwidth of 1.23

    ## Picking joint bandwidth of 1.16

    ## Picking joint bandwidth of 0.784

![](memo_files/figure-gfm/draft-critical-ridge-plot-1.png)<!-- -->

``` r
ggsave("postpartum-ridges-wide.png", width = 10, height = 5)
```

    ## Picking joint bandwidth of 1.03

    ## Picking joint bandwidth of 1.06

    ## Picking joint bandwidth of 1.23
    ## Picking joint bandwidth of 1.23

    ## Picking joint bandwidth of 1.16

    ## Picking joint bandwidth of 0.784

``` r
Postpartum |>
  count(first_age) |>
  arrange(desc(n))
```

    ##    first_age   n
    ## 1         32 520
    ## 2         33 433
    ## 3         31 431
    ## 4         30 352
    ## 5         34 338
    ## 6         29 328
    ## 7         35 326
    ## 8         36 233
    ## 9         28 195
    ## 10        26 171
    ## 11        27 159
    ## 12        37 112
    ## 13        38 107
    ## 14        40 103
    ## 15        39  80
    ## 16        25  43
    ## 17        42  43
    ## 18        41  28
    ## 19        20  23
    ## 20        24  23
    ## 21        21  20
    ## 22        23  12
    ## 23        22  10
    ## 24        18   6
    ## 25        44   6

``` r
Postpartum |>
  filter(critical != "NA") |>
  filter(critical != "Duplicate entry") |>
  filter(!critical %in% c("Family support", "Help with meals",  "Pelvic floor PT", "New parent groups", "In-home follow up appointments", "Other", "Overnight help", "Paid parental leave", "Massage or chiropractic", "Hospital/office follow up appointments", "Acupuncture", "Unpaid parental leave")) |>
  filter(regions != "NA") |>
  ggplot(aes(x = first_age, 
           y = critical,
           color = critical, 
           fill = critical)) +
 # facet_wrap(~ regions) +
  geom_violin() +
  #geom_density_ridges(alpha = .6) +
  scale_color_viridis_d() +
  scale_fill_viridis_d() +
 # scale_x_continuous(breaks = seq(from = 18, to = 44, by = 2)) +
  theme_ridges() +
  theme(legend.position = "none") +
  theme(axis.title.x = element_blank()) +
  #     axis.text.x = element_blank(),
   #    axis.ticks.x = element_blank()) +
  theme(axis.title.y = element_blank()) +
  # need to re-order care types and potentially break it up by quality or formal/informal
  labs (title = "Top 3 Most Critical Postpartum Care Types", subtitle = "by Survey Respondents Across the US", caption = "Age During First Birth")
```

![](memo_files/figure-gfm/draft-alternative-violin-plot-1.png)<!-- -->

``` r
Postpartum |>
  filter(critical != "NA") |>
  filter(critical != "Duplicate entry") |>
  filter(!critical %in% c("Family support", "Help with meals",  "Pelvic floor PT", "New parent groups", "In-home follow up appointments", "Other", "Overnight help", "Paid parental leave", "Massage or chiropractic", "Hospital/office follow up appointments", "Acupuncture", "Unpaid parental leave")) |>
  filter(regions != "NA") |>
  ggplot(aes(x = first_age, y = critical, color = critical)) +
  geom_point(alpha = 0.2) +
  scale_color_viridis_d() +
  theme_ridges() +
  theme(legend.position = "none") +
  labs(x = "Review Scores Ratings", y = "Neighbourhoods")
```

![](memo_files/figure-gfm/draft-dot-plot-1.png)<!-- -->

### Plot 4: Heat map

We experimented with visualizing support types across geographical areas
using a heat map.

``` r
Postpartum |>
  filter(support_type != "NA") |>
  filter(regions != "NA") |>
  filter(support_type != "Family support") |>
  ggplot(aes(x = regions, y = support_type, fill = regions)) +
   geom_tile(alpha = .3) +
 scale_fill_viridis_d() +
  theme_minimal() +
   theme(axis.title.x=element_blank(),
        axis.text.x=element_blank(),
        axis.ticks.x=element_blank()) +
 # theme(axis.text.y=element_blank(),
       # axis.ticks.y=element_blank()) +
  labs(title = "Heatmap-Like Plot Example", y = "Care Type", fill = "US Region")
```

![](memo_files/figure-gfm/draft-heatmap-like-for-plot-critique-1.png)<!-- -->

``` r
ggsave("example-tile-heatmap-wide.png", width = 10, height = 5)
```

### Plot 6: Missing Data

``` r
Postpartum_missing_data <- Postpartum |>
  select(respondent, state, age, birth_location, informed_by, other_info_sources, support_type, provider, ins_covered_services, cost_factor, if_insurance, critical_support, ideal_support, comments, emails)


vis_miss(Postpartum_missing_data)
```

![](memo_files/figure-gfm/draft-missing-data-1.png)<!-- -->

``` r
ggsave(filename = "Missingdataplot.png", width = 8, height = 4)
```

#### Final Plot 1

``` r
#Colourblind-friendly colour choices per support types:
#Baseline: d95f02
#Physical: 66c2a5
#Community: d8b365
#Emotional: af8dc3

Postpartum |>
  filter(!is.na(support_type)) |>
  filter(support_type != "New parent groups - in person",
         support_type != "NA",
         support_type != "Overnight help",
         support_type != "Family support",
         support_type != "New parent groups",
         support_type != "In-home help with care tasks",
         support_type != "Other",
         support_type != "None of the above",
         support_type != "Help with meals") |>
  select(respondent, support_type) |>
  count(support_type) |>
  mutate(percentage = n / 784 *100) |>
ggplot(mapping = aes(x = fct_reorder(support_type, percentage), y = percentage, fill = support_type)) +
  geom_col() +
  scale_fill_manual(
    values = c(
      "Lactation support" = "#d95f02",
      "Massage or chiropractic" = "#66c2a5",
      "Acupuncture" = "#66c2a5",
      "Emotional support" = "#af8dc3",
      "Hospital/office follow up appointments" = "#d95f02",
      "In-home follow up appointments" = "#66c2a5",
      "Pelvic floor PT" = "#d95f02")
    ) +
  theme_bw() +
  theme_ridges() +
  theme(legend.position = 'none') + 
  coord_flip() +
  labs(title = "Formal types of Postpartum Care Accessed", 
       subtitle = "In the US Between 2013-2023 by Survey Respondents", 
        x = "", 
       y = "")
```

<img src="memo_files/figure-gfm/final-formal-bar-chart-1.png" alt="Bar Chart displaying percentage of survey respondents of a postpartum care experience survey who had access to seven main types of formal care. After giving birth, most people had access to Baseline support types, meaning 73.0% accessed Lactation support, 48.5% accessed Hospital/office follow up appointments, and 17.6% accessed Pelvic floor PT. For the category of Emotional support, 42% accessed this type of care. Lastly, for Physical support, 14.2% accessed Massage or chiropractic and under 15% had access to In-home follow up appointments and Acupuncture."  />

``` r
ggsave("final-formal-bar-chart.png", width = 10, height = 4)
```

``` r
Postpartum |>
  drop_na(support_type) |>
  filter(support_type != "Lactation support",
         support_type != "Hospital/office follow up appointments",
         support_type != "Emotional support",
         support_type != "Pelvic floor PT",
         support_type != "Massage or chiropractic",
         support_type != "In-home follow up appointments",
         support_type != "Acupuncture",
         support_type != "Other",
         support_type != "Overnight help",
         support_type != "Family support",
         support_type != "None of the above") |>
  select(respondent, support_type) |>
  count(support_type) |>
  mutate(percentage = n / 784 *100)|>
ggplot(mapping = aes(x = fct_reorder(support_type, percentage), y = percentage, fill = support_type))  +
  geom_col() +
  scale_fill_manual(
    values = c(
      "Help with meals" = "#d8b365",
      "In-home help with care tasks" = "#66c2a5",
      "New parent groups" = "#d8b365"
    )) +
  theme_bw() +
  theme_ridges() +
  theme(legend.position = 'none') + 
  coord_flip() +
  labs(title = "Informal types of Postpartum Care Accessed", 
       subtitle = "In the US Between 2013-2023 by Survey Respondents",
       x = "",
       y = "")
```

<img src="memo_files/figure-gfm/final-informal-bar-chart-1.png" alt="Bar Chart displaying percentage of survey respondents of a postpartum care experience survey who had access to three main types of informal care. After giving birth, 62.2% had access to In-home help with care tasks, which is categorized as Physical support. 61.0% had access to New parents groups, and 42.2% of respondents had access to Help with meals, which are both categorized as Community support."  />

``` r
ggsave("final-informal-bar-chart.png", width = 10, height = 4)
```

### Plot 2: Violin plot

``` r
fig.alt="Violin plot of the frequency of support types mentioned by survey respondents by age of first birth and answers to 'What support was most critical to you/your household in the year following birth?' where the top 3 answers were lactation support, in-home help with care tasks, and emotional support. The color scale is viridis. The majority of survey participants fall between ages 29 and 35. Within this age group, there is fairly consistent pattern in the frequency that these care types were mentioned as most critical to their postpartum experience. When these 'most critical' care types are compared with what care types survey respondents had access to, almost double as many respondents had access to lactation support compared to the other two most critical care types."

Postpartum |>
  filter(critical != "NA") |>
  filter(critical != "Duplicate entry") |>
  filter(!critical %in% c("Family support", "Help with meals",  "Pelvic floor PT", "New parent groups", "In-home follow up appointments", "Other", "Overnight help", "Paid parental leave", "Massage or chiropractic", "Hospital/office follow up appointments", "Acupuncture", "Unpaid parental leave")) |>
  ggplot(aes(x = first_age, 
           y = critical,
           color = critical, 
           fill = critical)) +
  geom_violin() +
  scale_color_viridis_d() +
  scale_fill_viridis_d() +
  theme_ridges() +
  theme(legend.position = "none") +
  theme(axis.title.x = element_blank()) +
  theme(axis.title.y = element_blank()) +
  labs (title = "Top 3 Most Critical Postpartum Care Types", subtitle = "by Survey Respondents Across the US", caption = "Age During First Birth")
```

![](memo_files/figure-gfm/final-violin-plot-1.png)<!-- -->

``` r
ggsave("postpartum-violin.png", width = 10, height = 5)
```

### Plot 3: Missing Data

This plot is not included in our hand-out yet is a useful indicator that
we could make a fairly reliable analysis and conclusions, since we had
very few missing values (about 95% complete data).

``` r
Postpartum_missing_data <- Postpartum |>
  select(respondent, state, age, birth_location, informed_by, other_info_sources, support_type, provider, ins_covered_services, cost_factor, if_insurance, critical_support, ideal_support, comments, emails)


vis_miss(Postpartum_missing_data)
```

![](memo_files/figure-gfm/missing-data-plot-1.png)<!-- -->

``` r
ggsave(filename = "Missingdataplot.png", width = 8, height = 4)
```
