---
#
# By default, content added below the "---" mark will appear in the home page
# between the top bar and the list of recent posts.
# To change the home page layout, edit the _layouts/home.html file.
# https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: page
---
# Categorising and Predicting Most Valuable Business Traveller Category(s)

---
## Executive Summary
My business problem that I am solving is to find out which of the people represented in my data set fit into distinct categories, and what are the distinguishing features of those categories. I also want to use machine learning to predict how valuable a customer would be based on the value of one or more columns to be predicted based on the other columns values, and potentially therefore which columns/features contribute most to value.

> ⁠How did you solve it

> What did you find

---
## Introduction
> which provides background and context for your problem, why you
chose this research question and why it is a strong candidate for a data-driven
solution

The background for this problem is 

I chose this research question because I believed that it was a question that could be answered using my dataset, but was detailed enough to be worthwhile applying machine learning to the problem. I am also personally invested in finding out the results of the analysis as the dataset is too dense to be analysed by hand
There are very useful points for figuring out who is most likely to do lots of travelling, be prone to spending, how rich they potentially are, such as country of residence, age, remote working, frequency of business travel, level of education, job title and what they would be willing to spend on to upgrade travel.
I am hoping that this would hypothetically allow someone to target the best people for upselling services or items.
I believe this is a strong candidate for a data-driven solution because the overall subjective problem has been broken down into many smaller non-subjective problems with categorical answers.
Also
---
## Methods
> including a description of your data sources (at least two), a
summary of any data cleaning you performed, and a description and justification of
your selected method for analysis (use any models or methods you have learned
about)
For this problem I have selected one main dataset of information on business travellers in the form of a questionnaire on the subject audience (i.e. the travellers) and one secondary dataset in the form of a table that elaborates on the questions asked in the questionnaire, the datatypes of columns, and the value bounds of the columns, which I have manually created to aid in the programmatic data cleaning process.
The main dataset was provided to me in the form of two Google Sheets files, one for the answers, and one for the questions and question key.
The secondary dataset is another Google Sheets file, and if possible, I will include this file in an appendix at the bottom of the report.
Using this file I have categorised the availible columns in the main dataset into four categories:
- Green - These columns contain data that I want to keep in the dataset as is
- Blue - These columns also contain data I want to keep in the dataset, but they will need some modification and cleaning, either programmatically or via Excel before analysis can take place.
- Yellow - These columns contain data that may be relevant to the problem statement, but I am not certan they are necessary, such as
    - The type of travel policy at the company the person works for
    - if they are LGBTQ or transgender
    - The size of their employer
    - How long their employer has been in business
    These may have some impact on the problem statement, but the company-related items can be covered by other columns, and although I think LGBTQ status is useful for diversity's sake, I suspect it won't be relevant enough
- Red - These columns contain data that I either believe is not relevant to the problem statement, or contain data that if used might come with moral baggage. It also includes data that is neither numerical, binary, or categorical, such as large open-ended text fields
    - GenderO has been removed, it is used to describe the person's gender if they chose 'Other'
    - IndustryO has been removed, it is used to describe the person's field of work if they chose 'Other'
    - Whether or not the person's employer has a formal travel policy has been removed, as it can be inferred from other columns
    - Columns about if the person has taken the COVID vaccine or has special needs have been removed, as the vaccine is irrelevant, and I don't want to categorise travellers based on their disabilities or conditions
    - Race has been removed for two reasons, 1) not every country is covered, so the data is incomplete, and 2) categorising people based on race is morally questionable
    - The question about risk of hacking has been removed, as it refers to the person's trust in their employer's infrastructure, not in business travel
    - Relationship has been removed, as we already have Marital Status covering it
    - The question about who in the person's company has the most influence on the business travel programme has been removed, it is too removed from the problem statement
    - Air travel safety in the last year has been ignored, 1) because we cannot influence it, and 2) because it will quickly be rendered out of date
    - All questions about artificial intelligence (AI) have been removed. AI is a trending topic, but these AI questions relate to changes made by employers integrating AI and so are not relevent, as I am not that company(s)

Various forms of data cleaning need to be performed. Most columns are not named after the question that they relate to.
In binary columns, 0 means no and 1 means yes, but in some categorical columns, 0 is also used as a no value.
If I do use the Sexual Orientation column, it currently has values between 2 and 5
The Region, Education and Income columns are segregated by country, have inconsistent numbers of potential values, there are 3 Educations for Belgium(in different languages) and 2 for Canada.
Education also has an inconsistent scale of options for different countries. I have chosen to rebox this into 4 options (<=16yr, <=18yr, degree, doctorate/postdoc)
Income is using bands based on the currency of each country, which makes it impossible to directly compare
Finally for this section, all the columns related to the United States are mislabelled.

To elaborate on the issue with Region, Education and Income, each topic contains 30 or so questions, one for each country in the questionnaire, and each question uses one column, with a number value to represent the answer given to that question. The problem with analysing this with Pandas is that it is impossible to analyse it as is, because '1' in Australia means "Western Australia', but in Belgium it means 'Antwerp' so they needed to be merged into one scale with one option for every possible regional option to avoid confusion.
Also, the original layout meant that most columns like this were empty, as anyone living in Australia had to fill in the Australia column, but leave the other 30 blank. This heavily increased the number of null values in the dataset.
The Education columns also had an inconsistent scale of options for different countries. I chose to rebox this into 4 options (<=16yr, <=18yr, degree, doctorate/postdoc) for straightforward comparison.
Finally the Income columns do have clear bands for income, but each country's question has used their respective currency, so I had to create a new generally applicable set of bands and convert the currencies used to the same currency and convert the answers given to match the new bands

After removing the aforementioned unwanted columns, merging and renaming columns as needed, and solving the data division problem explained in the last paragraph, it was then time to utilise the standard methods of data cleaning, to check for missing or invalid values, and deal with them as appropriate.

## TODO
- rebox education
- convert currency and rebox to reasonable boxes
- lengthen explaination of machine learning choices
- complete introduction
- research alternative to k-nearest neighbours
- convert text to past tense

I plan to use two different types of machine learning on the dataset:
1) a k-means clustering to group the participants in the questionnaire to be able to figure out if there are key groups, what common factors do they have, and how large the groups are
2) a k-nearest neighbours analysis to be able to predict if a given new person would be willing to spend their own personal money on perks based on the information in the dataset

I believe k-means clustering is suitable for this task because it is already well suited for categorising customers according to the information provided on the course. I also feel it will create useful graphics for non-technical people

I think k-nearest neighbours can allow me to build a model to predict the proclivities of people based on existing data, but I may want to swap to another supervised predictive analysis method if I can find one that is more suited to the job, and allows me to see the important of factors in the logic.
Support Vector Machines do allow the viewing of importance of factors, but it is not simple and I am unsure the model is much better than k-nearest neighbours.
---
## Results 
> including descriptive statistics and graphs with axis labels and
captions, your results, including graphs with axis labels and figure captions, and a
discussion and interpretation of your results

---
## Conclusion
> which summarises the work you have done and your recommendations
for next steps

---
## License

The theme is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
