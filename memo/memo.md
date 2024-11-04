Project memo
================
Postparty Experience - Tikyra, Emmy, and Willow

This document should contain a detailed account of the data clean up for
your data and the design choices you are making for your plots. For
instance you will want to document choices you’ve made that were
intentional for your graphic, e.g. color you’ve chosen for the plot.
Think of this document as a code script someone can follow to reproduce
the data cleaning steps and graphics in your handout.

``` r
library(tidyverse)
library(broom)
library(readxl)
```

``` r
# use this part in the proposal


Postpartum_by_child <- read.csv("Postpartum_by_child.csv")
```

## Data Clean Up Steps for Overall Data

### Step 1: \_\_\_\_\_\_\_\_\_

``` r
Postpartum_support_type <- Postpartum_by_child |>
  mutate(support_type = str_replace(support_type, pattern = "In-home help with tasks like laundry, cooking, cleaning, and child care", "In-home help with care tasks")) 
```

``` r
Postpartum_support_longer <- Postpartum_support_type |>
  separate_longer_delim(support_type, delim = ",")
```

``` r
  unique(Postpartum_support_longer$support_type)
```

    ##  [1] "Lactation support"                                                                                                                                   
    ##  [2] " Emotional support"                                                                                                                                  
    ##  [3] " In-home help with care tasks"                                                                                                                       
    ##  [4] " Hospital or office based wellness services and postpartum follow up appointments"                                                                   
    ##  [5] " In-home wellness services and postpartum follow up appointments"                                                                                    
    ##  [6] " Community meal train or other meal service"                                                                                                         
    ##  [7] " New parent groups - in person"                                                                                                                      
    ##  [8] " Massage or chiropractic"                                                                                                                            
    ##  [9] " Acupuncture"                                                                                                                                        
    ## [10] "Emotional support"                                                                                                                                   
    ## [11] " New parent groups - online"                                                                                                                         
    ## [12] " Pelvic floor rehabilitation"                                                                                                                        
    ## [13] " Grief support"                                                                                                                                      
    ## [14] " Doula. Also"                                                                                                                                        
    ## [15] " I probably had access to pelvic floor therapy but didn’t know about it in order to access it."                                                      
    ## [16] " I didn’t attend any of the new parent groups"                                                                                                       
    ## [17] " but the second hospital I delivered at has a robust new parent program"                                                                             
    ## [18] "None of the above"                                                                                                                                   
    ## [19] "In-home help with care tasks"                                                                                                                        
    ## [20] "Community meal train or other meal service"                                                                                                          
    ## [21] "New parent groups - online"                                                                                                                          
    ## [22] "Hospital or office based wellness services and postpartum follow up appointments"                                                                    
    ## [23] " Mental health therapy"                                                                                                                              
    ## [24] " Family"                                                                                                                                             
    ## [25] " Therapist but not specializing in postpartum concerns"                                                                                              
    ## [26] " Nighttime support night nurse"                                                                                                                      
    ## [27] " 1x nurse check-in over phone (through hospital)"                                                                                                    
    ## [28] " Lactation support was VERY hard to access"                                                                                                          
    ## [29] "Pelvic floor rehabilitation"                                                                                                                         
    ## [30] " 6 week follow up pp with doctor"                                                                                                                    
    ## [31] " Therapy for Anxiety"                                                                                                                                
    ## [32] " I had access to lactation/new parent group but didn’t have the knowledge to go to them"                                                             
    ## [33] " I probably could have had access to some of the other things on this list but didn't look into it"                                                  
    ## [34] ""                                                                                                                                                    
    ## [35] " Ped PT and dentist for tie release + torticollis (for LO but her care is critical to my well being + BF journey)"                                   
    ## [36] " None of the above"                                                                                                                                  
    ## [37] " Had to seek out lactation help on my own"                                                                                                           
    ## [38] " the process wasn’t easy or clear. After my 6-week checkup"                                                                                          
    ## [39] " most of my concerns were ignored by my ob."                                                                                                         
    ## [40] " Except for my second  was born March 2020. I had no follow up"                                                                                      
    ## [41] "Gave birth in 2020. Everything I thought I’d have access to was shut down"                                                                           
    ## [42] " One postpartum appointment with my OBGYN"                                                                                                           
    ## [43] " Lactation support was virtual only due to pandemic"                                                                                                 
    ## [44] " Stay at home spouse"                                                                                                                                
    ## [45] " Postpartum doula and a night doula"                                                                                                                 
    ## [46] " Lactation support pre-covid. Post covid nothing in hospital"                                                                                        
    ## [47] " Baby was in NICU so we also had access to a social worker and classes with fellow nicu parents on development/feeding/etc"                          
    ## [48] " Ovia Parenting App coaches"                                                                                                                         
    ## [49] " Some of these were offered but not helpful. Saw a behaviorist bc my dad died right before birth but they just said I “seemed fine” and were no help"
    ## [50] " Besides my required postpartum checkup and family care"                                                                                             
    ## [51] " I didn't really receive support. I know my pediatrician has lactation services but I didn't need."                                                  
    ## [52] " Home visiting (Maine Families and public health nursing)"                                                                                           
    ## [53] " Postpartum doula"                                                                                                                                   
    ## [54] " Therapy (for PPD with second kid)"                                                                                                                  
    ## [55] " In—home baby education"                                                                                                                             
    ## [56] " Virtual postpartum follow up visits due to COVID"                                                                                                   
    ## [57] " 1 PP appointment at 6 weeks"                                                                                                                        
    ## [58] "Yes there was a postpartum follow up but didnt seem like she did nuch other than repeat what she was reading off a pamphlet."                        
    ## [59] " As a note"                                                                                                                                          
    ## [60] " any supports I had access to I had to find myself."                                                                                                 
    ## [61] " My partner was incredibly helpful. He handled all of the emotional support and meal and house prep and cleaning. However"                           
    ## [62] " we didn’t have anyone else and I often wished we had someone to hold the both of us while we bonded with our baby."                                 
    ## [63] " follow-up appointment with midwife; overnight doula (1 night per week)"                                                                             
    ## [64] " Mommy and me yoga and classes"

``` r
Postpartum_support_type_clean <- Postpartum_support_longer |>
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
  mutate(support_type = ifelse(support_type == " I had access to lactation/new parent group but didn’t have the knowledge to go to them", "Lactation support, New parent groups - in person", support_type)) |>
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
  mutate(support_type = ifelse(support_type ==" In-home wellness services and postpartum follow up appointments", "In-home wellness services and postpartum follow up appointments", support_type))
```

``` r
unique(Postpartum_support_type_clean$support_type)
```

    ##  [1] "Lactation support"                                                               
    ##  [2] "Emotional support"                                                               
    ##  [3] " In-home help with care tasks"                                                   
    ##  [4] "Hospital or office based wellness services and postpartum follow up appointments"
    ##  [5] "In-home wellness services and postpartum follow up appointments"                 
    ##  [6] "Community meal train or other meal service"                                      
    ##  [7] "New parent groups - in person"                                                   
    ##  [8] "Massage or chiropractic"                                                         
    ##  [9] "Acupuncture"                                                                     
    ## [10] "New parent groups - online"                                                      
    ## [11] "Pelvic floor rehabilitation"                                                     
    ## [12] "Grief support"                                                                   
    ## [13] "NA"                                                                              
    ## [14] "None of the above"                                                               
    ## [15] "In-home help with care tasks"                                                    
    ## [16] "Family support"                                                                  
    ## [17] "Overnight help"                                                                  
    ## [18] "Lactation support, New parent groups - in person"                                
    ## [19] "Other"

``` r
#baseline <- c("Lactation support", "Pelvic floor rehabilitation",  "Emotional support",  "Hospital or office based wellness services and postpartum follow up appointments")

#better <- c("")

#best <- c("")

#baseline_pattern <- "lact|pelv|"

#Postpartum_support_type_clean <- Postpartum_support_type_clean |>
  #mutate(baseline = case_when(str_detect(support_type, pattern = regex(baseline_pattern, ignore_case = T)) ~ 1,
                             # TRUE ~ 0)) 

#Postpartum_support_type_clean <- Postpartum_support_type_clean |>
 # mutate(baseline = case_when(support_type %in% baseline ~ 1,
                            #  TRUE ~ 0)) |>
 # mutate(service_category = case_when(support_type %in% baseline ~ "baseline",
                            #  TRUE ~ 0)) |>
  
 # relocate(baseline, .after = support_type)

#Postpartum_support_type_clean |>
 # group_by(respondent, birth_age) |>
  #mutate(support_type_all = str_flatten(support_type, collapse = ", ")) |>
  #summarize(baseline_score = sum(baseline, na.rm = TRUE))
  #distinct(respondent, birth_age, .keep_all = TRUE) # we don't need this line, example only

#Postpartum_support_type_clean |>
 # group_by(respondent, birth_age) |>
  #summarize(baseline_score = sum(baseline, na.rm = TRUE))|>
  #ungroup() |>
  #distinct(baseline_score)
```

### Step 2: \_\_\_\_\_\_\_\_

## Plots

### ggsave example for saving plots

``` r
p1 <- starwars |>
  filter(mass < 1000, 
         species %in% c("Human", "Cerean", "Pau'an", "Droid", "Gungan")) |>
  ggplot() +
  geom_point(aes(x = mass, 
                 y = height, 
                 color = species)) +
  labs(x = "Weight (kg)", 
       y = "Height (m)",
       color = "Species",
       title = "Weight and Height of Select Starwars Species",
       caption = paste("This data comes from the starwars api: https://swapi.py43.com"))


ggsave("example-starwars.png", width = 4, height = 4)

ggsave("example-starwars-wide.png", width = 6, height = 4)
```

### Plot 1: \_\_\_\_\_\_\_\_\_

#### Data cleanup steps specific to plot 1

These data cleaning sections are optional and depend on if you have some
data cleaning steps specific to a particular plot

#### Final Plot 1

### Plot 2: \_\_\_\_\_\_\_\_\_

### Plot 3: \_\_\_\_\_\_\_\_\_\_\_

Add more plot sections as needed. Each project should have at least 3
plots, but talk to me if you have fewer than 3.

### Plot 4: \_\_\_\_\_\_\_\_\_\_\_
