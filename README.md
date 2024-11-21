The Postpartum Experience
================
by Emmy Wahlström, Tikyra Downs, and Willow Gibson

## Summary

This project provided an opportunity for deeper exploration into a survey based dataset collected for another COA course. The dataset contains a range of information related to the postpartum experience of anyone who gave birth between 2013 and 2023. We were focused on identifying the quality of care that survey respondents had access to, and what care was accessible vs. what care was cited as most critical.

## Methods

The Postpartum Experience Survey was created by Willow Gibson for a research project assigned in a course titled "History of Midwifery and Women's Healthcare in the US" at College of the Atlantic in 2022. The data was collected through a Google form survey that was circulated via email and briefly on social media. The survey accepted responses for 1 week in February 2022 and received 788 responses. The purpose of collecting this information was to observe the state of postpartum care in the US across the ten year period of 2013 - 2023 with the goal of learning what gaps in care exist. The dataset is unpublished and is considered preliminary data only. The survey questions can be viewed [here] (add link).

## Analysis

In the process of cleaning our data we came to understand how important survey design is. It is a balancing act to elicit authentic responses from survey respondents while simultaneously ensuring the data collected is tidy enough to use for creating plots. We found that this survey may have offered too many opportunities for open ended responses. It was necessary to standardize answers in order to create meaningful and readable plots. 

We initially thought about assigning numerical values to the quality categories and looking at them through a lens of "good/better/best" types of care. Each category would have been additive moving through the scale, and we planned to look at that data on a leaflet map in order to display quality of care across the US. While we still believe this would be a valuable exploration, we felt we would need to study `stringr` in more depth before moving forward with this type of plot. Instead, we created care categories that aligned with the following definitions (based on our research):

Baseline Support: Care needed to prevent the worst long term health outcomes (i.e. organ prolapse, long term depression, etc.)
Survey entries that are in this category: Lactation support, pelvic floor PT, emotional support, hospital/office follow up appointments, unpaid parental leave

Physical Support: Care that increases the birthing parent and baby’s physical wellbeing beyond baseline support
Survey entries that are in this category: In-home help with care tasks, in-home follow up appointments, bodywork like massage or chiropractic, acupuncture, or overnight help

Community Support: Care that builds and maintains community around the birthing parent/family for long term support
Survey entries that are in this category: Help with meals (meal train or meal service), new parent groups (online and in-person), family & friend support, paid parental leave

We chose a violin plot, bar charts, and word clouds to represent the data from the survey. These were clear and concise ways to visualize hundreds of entries and make sure every respondent’s experience was accounted for. The Bar graphs and violin graphs were an efficient way to communicate quantitative data.  

The bar chart for formal types of postpartum care access shows that over 60% of respondents had accessed lactation support, while only 42% of respondents had access to emotional support. The least accessed support types following emotional support in descending order were Pelvic floor (pt), chiropractic care, in home follow up appointments, and acupuncture.

In the informal types of of postpartum care bar graph, 37.5% of respondents reported help with in home care tasks, 33.4% had access to new parent support groups (both online and in person), and only 23% of respondents had help with meals from others. It was disheartening to learn that less than half of respondents had assistance with help with in home tasks, implying that somebody that had recently given birth is not only doing a lot of child rearing, but is alone in maintaining the home. 

In the violin plot for the top three most critical postpartum types, we observe that the majority of survey participants fall between ages 29 and 35. Within this age group, we can see a fairly consistent pattern in the frequency that these care types were mentioned as most critical to their postpartum experience. The three most critical postpartum types accessed were lactation support, in home help with care tasks, and emotional support. The specific care mentioned in the survey was grouped into the following categories to create this plot:

Lactation support: Access to high quality pumping equipment and lactation services from IBCLC lactation consultants, La Leche League Groups, as well as Doulas, Midwives, Nurses, and family members. Part of baseline support.

Care tasks: As defined by the author KC Davis, LPC: 
“Care tasks describes any task, chore, or errand that is required to care for self and keep life going. Typically, these tasks are recurring, never-ending, and are required to be completed in order to "get on with living." 
In the context of the postpartum experience, this might include childcare of other children, help with cleaning, cooking, or laundry, holding the baby while the parent sleeps or bathes, picking up groceries, or other helpful tasks around the home.

Emotional support: This care type includes formal and informal mental health treatment like therapy, PPD/PPA medications, advice from loved ones or doulas, etc.


The Word clouds were helpful in visualizing qualitative data. We took the comments from the open ended responses within the survey about support respondents felt were most critical to them, as well as comments about care that would have been ideal to have receive. The results show words that were associated with the other graphs, words such as "cleaning, family, house, task, groups" correlating with the multiple choice option "in home help with care tasks".


## Conclusions

Many people noted that their partner's help was most critical to them. While we recognize how vital a supportive partnership is in the journey of parenting, particularly in the early postpartum period, this survey was focused on postpartum support beyond the parents of the child.

There were also a number of responses that mentioned what "would" have been most critical to them. We excluded these answers from our analysis of critical care, but think it's important to mention that there are families who endure the postpartum experience with zero outside support.

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

