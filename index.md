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
The Education columns also had an inconsistent scale of options for different countries. I chose to rebox this into 4 options (<=16yr, <=18yr, degree, doctorate/postdoc/additional vocational qualification) for straightforward comparison.

- anything before mid secondary => 1
- anything after the last one to end of secondary/age 18 => 2
- anything after the last one to bachelor degree => 3
- anything after the last one like post grad, associate, masters => 4
- vocational courses, phds, doctorates and long term training things => 5
(<16 and <18 are British age bands, since some countries divide education at 14 I will be naming the first 2 bands early secondary and late secondary, and early secondary will cover everything from preschool to age 14 or 16, since I think it unlikely many people have dropped out of school so early so as to differentiate between them in this dataset)
Some detail regarding the idiosycracies of education systems will be lost in the process, but since there are over 30 countries direct mapping is almost impossible
Finally the Income columns do have clear bands for income, but each country's question has used their respective currency, so I had to create a new generally applicable set of bands and convert the currencies used to the same currency and convert the answers given to match the new bands
Salaries correct as of date of recording (unknown) and exchange rates correct as of 5/9/25
Education and Income then had to be merged into one column so the model(s) would accept it
I have kept the original columns and data for later use, but will only train/test/validate the model on a subset of columns not including them, as the new converted data will be less accurate than the old data.
I have decided to convert all of the bands to USD and reassign each band to a new universal band as below, taking the midpoint of each band:
1	<$5k
2	$5k–$10k
3	$10k–$20k
4	$20k–$30k
5	$30k–$50k
6	$50k–$75k
7	$75k–$100k
8	$100k–$200k
9	$200k–$500k
10	>$500k

This is somewhat inaccurate, but without the time to assess the weighting of people within each band according to their income (which I don't know) and the size of each band to precisely fraction each band into new bands, and the use of a logarithmic scale to make the new bands even in size, I think this was the best that I could do to make the data ready for machine learning.
I could have decided to onehot encode the Education columns, or assign each unique value a value from 1 to however many unique values there are, but that had a flaw in that both categories are ordinal, and it is not entirely clear which qualifications from one country are more or less valuable than others. Also some may be different qualifications, but equal in value.

After removing the aforementioned unwanted columns, merging and renaming columns as needed, and solving the data division problem explained in the last paragraph, it was then time to utilise the standard methods of data cleaning, to check for missing or invalid values, and deal with them as appropriate.

## TODO
- onehot encode region
- convert currency and rebox to reasonable boxes
- merge currency into one column
- lengthen explaination of machine learning choices
- complete introduction
- research alternative to k-nearest neighbours
- convert text to past tense
- upload data to second repo
- import data to python
- clean data again
- split into temporarily unusable income columns and rest
- split rest into train/test/whatever as needed
- create and run models
- create graphs
- interpret graphs
- add graphs and interpretation to report
- write conclusion, things to do better and conclusion

# Cleaning done
- rescaled transgender from 1-2 to 0-1 where 0 is no /
- rescaled employment length so 994 is 0 and 995 (meaning 100+ years is 100) /
- removing red columns /
- merging education /
- remapping values of orientation /
- reboxing education /
- merging education /
- converting education to numbers /
- reboxing income
- merging INCOME
- mapping region, merging and onehot encoding
- handling 0 is no /
- checking for null values

# create mapping
mapping = {
    "France": {1: "North France", 2: "South France"},
    "Spain": {1: "Madrid", 2: "Barcelona"},
    # ... repeat for all 30 country columns
}
# convert numbers to names
for col, colmap in mapping.items():
    df[col] = df[col].map(colmap)

# create new column called region with all new names
df_long = df.melt(value_vars=mapping.keys(), value_name="region")["region"]

# create 1 column for each region, one hot encoded in each row accordingly
df_onehot = pd.get_dummies(df_long, prefix="region", dtype=int)

# remove old columns (REGION_NAME)
df.drop(['names', 'of', 'old', 'columns'])

# attach new columns (will attach after last column, need to split and concat to insert in middle)
new_df = df.join(df_onehot)

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

lots of data lost to differences between education systems, unsure how to compare qualifications unique to a country with other unique qualifications
Could learn this, would have to learn the intricacies of 30+ countries education systems, not enough time to do that
currency data does not take into account regional purchasing power, original bands are uneven
standardising bands for easier analysis, but raw data is not ideal
Dataset is not originally mine or designed exactly for what I am using it for
I cannot access the questionnaire format that it was captured in to assess question idiosyncracies
Data was gathered by multiple people into the spreadsheet initially, and then standardised. Data may have been lost in the process, but I don't have control over that, and arguably that is more the concern of the company I asked the data from, as data lost that way is data they could perhaps use.


---
## License

The theme is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
