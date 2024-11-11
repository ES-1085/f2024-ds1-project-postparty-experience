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
