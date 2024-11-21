The Postpartum Experience
================
by Emmy Wahlström, Tikyra Downs, and Willow Gibson

## Summary

The postpartum experience survey was created by Willow Gibson for a research project assigned in a course titled "History of Midwifery and Women's Healthcare in the US" at College of the Atlantic in 2022. The survey was used to collect data about the postpartum experiences of those who gave birth between 2013 and 2023 in the U.S.A. There were 784 responses. The purpose of collecting this information was to observe the state of postpartum care in the US with the aim of recognizing gaps in care. This project provided an opportunity for deeper exploration into the data collected and identified the types of care and quality of care that respondents accessed.

## Methods

Data was collected through a Google form survey that was circulated via email and briefly on Facebook and Instagram. The survey accepted responses for 1 week in February 2022 and received 788 responses including international entries, which we later excluded. The data set is unpublished and is considered preliminary data only. The survey questions can be viewed [here]: https://forms.gle/AX37kKZ9CkroQZPm7.

## Analysis

We initially planned to create quality categories of "good/better/best" types of care and assign them numerical values in an ordinal scale. We wanted to look at that data on a leaflet map in order to display quality of care across the US. While we still believe this would be a valuable exploration, we felt we would need to study `stringr` in more depth before moving forward with this type of plot due to the data being almost exclusively text from open ended and multiple choice questions.

Instead, we created care categories that aligned with the following definitions based on our research:

`Baseline Support`: Care needed to prevent the worst long term health outcomes (i.e. organ prolapse, long term depression, etc.)
Survey entries that are in this category: Lactation support, pelvic floor PT, emotional support, hospital/office follow up appointments, unpaid parental leave

`Physical Support`: Care that increases the birthing parent and baby’s physical wellbeing beyond baseline support.
Survey entries that are in this category: In-home help with care tasks, in-home follow up appointments, bodywork like massage or chiropractic, acupuncture, or overnight help

`Community Support`: Care that builds and maintains community around the birthing parent/family for long term support.
Survey entries that are in this category: Help with meals (meal train or meal service), new parent groups (online and in-person), family & friend support, paid parental leave

When cleaning our data, we also chose to group some responses together to create more cohesive (and shorter) sets of words to use in our visualizations. These included:

`In-home help with care tasks`: As defined by the author KC Davis, LPC: 
“Care tasks describes any task, chore, or errand that is required to care for self and keep life going. Typically, these tasks are recurring, never-ending, and are required to be completed in order to "get on with living." 
In the context of the postpartum experience, this might include childcare, help with cleaning, cooking, or laundry, holding the baby while the parent sleeps or bathes, picking up groceries, or other helpful tasks around the home. Part of physical support.

`Lactation support`: Access to high quality pumping equipment and lactation services from IBCLC lactation consultants, La Leche League Groups, as well as Doulas, Midwives, Nurses, and family members. Part of baseline support.

`Emotional support`: This care type includes formal and informal mental health treatment like therapy, PPD/PPA medications, advice from loved ones or doulas, etc. Part of baseline support.

We used one violin chart, two bar charts, and two word clouds to visualize hundreds of entries and make sure every respondent's experience was accounted for in a clear and concise way.

The bar chart for formal types of postpartum care access shows that over 60% of respondents had accessed lactation support while only 42% of respondents had access to emotional support. The least accessed support types following emotional support were pelvic floor PT, chiropractic care, in home follow up appointments, and acupuncture in descending order.

In the informal types of postpartum care bar chart, 37.5% of respondents reported help with in-home care tasks, 33.4% had access to new parent support groups (both online and in person), and only 23% of respondents had help with meals from others. It was disheartening to learn that less than half of respondents had assistance with in-home tasks, implying that somebody that had recently given birth is not only doing a lot of the child rearing, but is alone in maintaining the home. 

In the violin plot for the top three most critical postpartum types, we observe that the majority of survey participants fall between ages 29 and 35. Within this age group, we can see a fairly consistent pattern in the frequency that these care types were mentioned as most critical to their postpartum experience. The three most critical postpartum types accessed were lactation support, in home help with care tasks, and emotional support. 

The Word clouds were helpful in visualizing qualitative data. We used the comments from the open-ended responses within the survey about support respondents felt were most critical to them, as well as comments about care that would have been ideal to have received. The results show words that were associated with the other graphs. Words such as "cleaning, family, house, task, groups" correlated with the multiple choice option "in home help with care tasks" for types of postpartum care access.


## Conclusions

In the process of cleaning our data we came to understand how important survey design is. It is a balancing act to elicit authentic responses from survey respondents while simultaneously ensuring the data collected is simplified enough to use for creating plots. We found that this survey may have offered too many opportunities for open ended responses. 

There were also a number of responses that mentioned what "would" have been most critical to them. We excluded these answers from our analysis of critical care, but think it's important to mention that there are families who endure the postpartum experience with zero outside support. It was necessary to standardize answers in order to create meaningful and readable plots.

Despite this, the analysis we carried out shows major differences of what postpartum care people in the US can access and our plots are useful in identifying what aspects of care differed most.  


## Handout

Our presentation can be found [here](memo/postpartum-experience-poster.pdf).

## Memo

A link to the code and how we created our graphics in our memo can be found [here](memo/memo.html).

## Data

Gibson, W 2022. Postpartum Experience Survey Data 2022, Unpublished raw data. November 22 2023. https://docs.google.com/spreadsheets/d/1lyA-MwFnF-uMN81ntOT_NAy-QoqueXNLb0CPFXzth3w/edit?usp=sharing

## References

“ACOG Committee Opinion No. 736.” Obstetrics & Gynecology, vol. 131, no. 5, May 2018, pp.
    e140–50, https://doi.org/10.1097/aog.0000000000002633.

Gibson, Willow. “Postpartum Care in the US.” 17 March 2022. History of Midwifery and Women's
    Health Care in the US, College of the Atlantic, student paper.

João Paulo Souza, et al. “A Global Analysis of the Determinants of Maternal Health and
    Transitions in Maternal Mortality.” The Lancet Global Health, vol. 12, no. 2, Elsevier
    BV, Dec. 2023, https://doi.org/10.1016/s2214-109x(23)00468-0.

Lopez-Gonzalez, Diorella M., and Anil K. Kopparapu. “Postpartum Care of the New Mother.”
   National Library of Medicine, StatPearls Publishing, 11 Dec. 2022,
   www.ncbi.nlm.nih.gov/books/NBK565875/.

Postpartum Experience Survey. “Postpartum Experience Survey.” Google Docs, 2023, forms.gle/9pfqZpnCXq5RstCB7. Accessed 17 Nov. 2024.

World Health Organization. “More than a Third of Women Experience Lasting Health Problems
    after Childbirth, New Research Shows.” Www.who.int, 7 Dec. 2023,
    www.who.int/news/item/07-12-2023-more-than-a-third-of-women-experience-lasting-health-problems-after-childbirth.



