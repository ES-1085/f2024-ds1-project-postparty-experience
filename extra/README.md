# extra

Any extra documents you might have go here. This might include Rmd files you're using to develop your project, any notes, or anything else. The contents of this folder will **not** be marked, it's just a convenient place to store documents and collaborate with teammates without cluttering the rest of your repo.


## Plot critique 1

```{r care-type-frequency-bar-chart }

#p1 <- 
Postpartum |>
  filter(support_type != "NA") |>
  filter(support_type != "Lactation support, New parent groups - in person") |>
  ggplot(mapping = aes(x = fct_infreq(support_type), fill = support_type)) +
   geom_bar() +
  #  aes(pattern = birth_location, fill = birth_location, pattern_fill = birth_location),
   # colour                   = 'black', 
  #  pattern_density          = 0.35, 
   # pattern_key_scale_factor = 1.3) +
  theme_bw() +
  scale_fill_viridis_d() + 
  theme(legend.position = 'none') + 
  coord_flip() +
  labs(title = "Types of Postpartum Care Accessed", subtitle = "In the US Between 2013-2023 by Survey Respondents", x = "Support Type", y = "Frequency", fill = "Birth Location") 


#ggsave("example-postpartum-wide.png", width = 10, height = 4)






Special legend (started)

p1 <- Postpartum |>
  filter(!is.na(support_type)) |>
  filter(support_type != "Lactation support, New parent groups - in person",
         support_type != "NA",
         support_type != "Overnight help",
         support_type != "Family support",
         support_type != "New parent groups",
         support_type != "In-home help with care tasks",
         support_type != "Help with meals") |>
  ggplot(mapping = aes(x = fct_rev(fct_infreq(support_type)), fill = support_type)) +
  geom_bar() +
  scale_fill_manual(
    values = c(
      "In-home follow up appointments" = "pink",
      "Massage or chiropractic" = "pink",
      "Acupuncture" = "pink",
      "default" = "dark blue"
    )
    ) +
  theme_bw() +
  theme(legend.position = 'none') + 
  coord_flip() +
  labs(title = "Formal types of Postpartum Care Accessed", 
       subtitle = "In the US Between 2013-2023 by Survey Respondents", 
       x = "Support Type", 
       y = "Number of Survey Respondents",
       ) 

  new_legend <- ggplot() +
  geom_rect(aes(xmin = 0, xmax = 1, ymin = 0.3, ymax = 0.5), fill = "pink") +
  geom_text(aes(x = 1, y = 0.7, label = "Higher quality services"), size = 5, hjust = 0) +
    theme_void() +
    theme(plot.background = element_rect(fill = "white"))
  
  grid.arrange(p1, new_legend, ncol = 2, widths = c(10, 1))
  
  
## Plot 2 Comments wordcloud

 #I asked chatgpt to randomly select a number between 1 and 789. I then just copied whatever comments were in those cells in the excel document to choose the quotes for our wordcloud. 215 391 420 644 170 159 538 154(BLANK) 466 535 351 439 231(BLANK) 266 174 572 and 577(BLANK) 306(BLANK) 662 180. I asked chat gpt to randomly generate 4 numbers between 1 and 786 to make up for the blank entries. These numbers were 328 567 149 and 734.

xiety and health anxiety with a newborn Fed is best Kaiser made me feel like a failure for supplementing with formula I had a relatively positive experience compared to many others I know the support is so vital after second developed post op infection and Ended up with wound vac for 6 plus weeks did have home health care nurse come just for that but no support other than that Struggled with breast feeding both Ended up pumping dried up at 4 months with first and stopped after 13 months with second Supply dropped a lot when I returned to work with second at 4 months postpartum and barely pumped enough to keep up with demand Had anxiety and depression after both Felt very alone as husband was working and working and in school with second baby Parents judgemental and not much help Friends disappeared after had first baby Received excellent care while pregnant countless Dr appointments and then once I left the hospital i felt completely abandoned Made me feel like the US health care system really only viewed be as an incubator for baby and once baby arrived they didnt care about me anymore  Only support I received was through seeking it out myself it should all be standard of care It was hard because everyone kept telling us it takes a village to raise a child but when our son was actually here, no one family friends was very interested in doing the actual work alongside us despite us asking for help A village is great but what happens when theres no village to show up for you Postpartum care also felt far too brief after going through such a huge transitional experience As the parents we overall felt very lonely and unsupported across different domains I prepared more for postpartum for my second delivery and although I paid more out of pocket it did make things easier during the fourth trimester The birth center gave me access to mental health support counseling which meant I was already regularly talking with someone and could get help when I realized I had postpartum anxiety This was crucial as seeking out a therapist and getting an appointment would have been an insurmountable hurdle for me I think there needs to be more support for partners as well because my husband really didnt know how to help me or adjust to his new role in a way that was sustainable That ultimately lead to more stress for me as he burned out and also had to go back to work The US needs better postpartum care This was my second child there were services but I had trouble accessing them because my older kid wasnt allowed")

corpus <- Corpus(VectorSource(comments_text))
corpus_clean <- corpus %>%
   tm_map(content_transformer
```{r wordcloud-1}
install.packages("wordcloud2")
install.packages("dplyr")
install.packages("png")
install.packages("tm", dependencies = TRUE, repos = "http://cran.rstudio.com/")


library(wordcloud2)
library(dplyr)
library(png)
library(tm) 

comments_text <- paste("We had a night nurse aka night nanny for 2 months She came 5 nights a week and taught us everything we needed to know in addition to allowing me to sleep 12 hours while she worked I had a level 4 tear during delivery and was surprised at how minimal the follow up and care instructions felt More support is needed in many areas My child was going to his pediatrician a few times a week or a few times a month until he was 3 months I was seen once and that was after a c section where they mainly just looked at my scar Definitely would have liked more support in postpartum After my first child was born I was a wreck and everyone told me it was normal In hindsight I wish someone would have said something to me that would have encouraged me to go get help I had intrusive thoughts and the hardest time sleeping not just because of the baby My milk took forever to come in and I never felt confident about breastfeeding Anytime I said anything about what I was feeling I was told I was normal I wish I had known that there were postpartum support available As a brand new mom of twins I didnt know what questions to ask or what was even available Next time we will be more prepared Having to go back to work 6 weeks later when my stitches still hadnt healed and I was barely sleeping was a nightmare Being postpartum in 2020 was absolutely awful The fear of you or your child getting sick because of having people over or taking the baby out was so so isolating Insurance made things really complicated I wasnt allowed to leave the birth center at the hospital until we had insurance for our daughter which was totally not on my radar and never mentioned in any of my education or prep support before birth And it was hard to find out what was covered and what costs would be if paid out of pocket And when my postpartum anxiety was bad my therapists recommended me to the midwives who then just recommended I see a therapist. So friends became extra helpful then This all broke apart 6 months in because covid hit so the small semblance of a community of postpartum support we had pieced together all fell apart And as I prepare for a second  child I dont have paid leave I shifted from full time W2 work with health insurance from to a 1099 consulting practice with high insurance costs we pay out to of pocket Id love what Germany and other countries have And we have privilege and access and so many people dont I dont know how marriages survive without such community care and I know many babies and children suffer as a result Also it turned out that birthing a baby also triggered a mental breakdown in our local in laws who we thought would be part of of our child care and support Couples therapy and individual therapy and mentors helped us come to understand how the event of our family triggered her narcissism and borderline personality disorder and that was traumatic experience that took my spouse and I 3 years to come to understand Postpartum in the US is horrible I had an unpaid leave and went back to work 9 weeks post c-section My baby got sick in daycare and I almost lost my job due to having to take time off I was lost sad exhausted and felt completely alone and like I was the only person experiencing what I was I felt like a complete failure to this child I am a LCSW and privileged to have access to supports and still felt this way it breaks my heart to think of those less educated privileged and the lack of support they experience")

corpus <- Corpus(VectorSource(comments_text))
corpus_clean <- corpus %>%
   tm_map(content_transformer(tolower)) %>% 
  tm_map(removePunctuation) %>%
  tm_map(removeNumbers) %>% 
  tm_map(removeWords, stopwords("en")) %>%
  tm_map(stripWhitespace) %>% 
  tm_map(removeWords, c("first", "second", "don't", "dont", "kid", "son", "for", "get", "one", "just", "make", "many", "come", "keep", "kept", "didn't", "going", "made", "anymore", "felt", "child", "something", "wasnt", "wasn't", "will", "borderline", "narcissism", "way", "children", "like", "hadn't", "hadnt", "germany", "told", "totally"))

tdm <- TermDocumentMatrix(corpus_clean)

m <- as.matrix(tdm)

word_freqs <- sort(rowSums(m), decreasing = TRUE)

word_freq_table <- data.frame(word = names(word_freqs), freq = word_freqs)

wordcloud2(word_freq_table, size = 0.5, color = "random-light", backgroundColor = "black")


```

## Second comments wordcloud

```{r wordcloud-2}
install.packages("wordcloud2")
install.packages("dplyr")
install.packages("tm", dependencies = TRUE, repos = "http://cran.rstudio.com/")


library(wordcloud2)
library(dplyr)
library(tm) 

comments_text <- paste("I would have loved more support in any form My PCP was the only medical provider who asked how I was doing emotionally my first year post-partum I think continued lactation support would have also made breastfeeding a lot easier I could have felt more supported in my decisions around feeding my infant How much COVID isolated me couldnt leave the house as a new mom and no one acknowledged how much that would increase likelihood of depression an(tolower)) %>% 
  tm_map(removePunctuation) %>%
  tm_map(removeNumbers) %>% 
  tm_map(removeWords, stopwords("en")) %>%
  tm_map(stripWhitespace) %>% 
  tm_map(removeWords, c("first", "second", "don't", "dont", "postpartum", "baby", "care", "support", "kid", "son", "for", "get", "one", "just", "make", "many", "come", "keep", "kept", "didn't", "gave", "going", "made", "anymore"))

tdm <- TermDocumentMatrix(corpus_clean)

m <- as.matrix(tdm)

word_freqs <- sort(rowSums(m), decreasing = TRUE)

word_freq_table <- data.frame(word = names(word_freqs), freq = word_freqs)

wordcloud2(word_freq_table, size = 0.5, color = "random-light", backgroundColor = "black")


```
