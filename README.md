# Analyzing-HR-company-data-with-Excel-Power-BI-and-R

Hello to all interested HR professionals. This will be the first showcase of the potential Microsoft platforms and R can offer into gaining meaningful HR insights. As this is the first case-specific scenario, it will be more general in nature, using a simple, static, made up company database that simulates the way HR departments typically store employee data for continuous workforce monitoring. It is also worth mentioning that the level of analyses corresponds to the ones found in entry-level HR positions in organisations and are in-line with the most common technical requirements for such positions. 

#introducing-the-company

We can see, right from the outset, that the company structures employee data in an Excel table and uses a uniquely identifiable serial code for each person, alongside their full names (randomly generated), job title, department and business unit, gender, ethnicity, age, gross annual salary, bonus %, country, city, hire and exit date. 

#Categorising-the-dataset

Nevertheless, we would need a more categorized structure of our data to better analyze them. The first column we would add is the one called "Career stage" and would need the following IF Function, with the arbitrary cut-off points set by the specific company: 

=IF(H2<=27,"Junior",IF(H2<=32,"Associate",IF(H2<=37,"Advanced","Senior")))

The same logic could be applied to the categorization of bonus packages:

=IF(K2=0, "No Bonus", IF(K2<=0.1, "Small Bonus", IF(K2<=0.2, "Medium Bonus", "Large Bonus")))

And then, we would have to add the "Final Salary" column, which would consist of a very simple formula: 

=Salary + (Salary*Bonus)

The abovementioned formula works due to the fact that names for the specific columns where already defined, using Formulas > Define Name, a good practice when working with datasets. 
Lastly, we would also need to see the tenure in years of people who have left,  by creating the last column called "Tenure until leave", with the following formula:

=IF(R2="","N/A",DATEDIF(Q2,R2,"y")&" years ")

**Starting of with the data analysis**

# the-hot-topic

There were rumors circulating in the company that women are paid less than men and that gender accounted for differences in salary. So naturally, the thing that caught our attention first was whether there were any gender pay disparities in the company. We started of by comparing initial/gross salaries per gender and department, using pivot tables. Initial pivot tables indicated that the average female was being paid higher than the average male in HR and Accounting, whiles males were being paid marginally higher in Finance and generally higher in the remaining departments. However, the average wage difference between male and female accross departments, races and countries was not that wide. This can be observed by a simple observation of the pivot table data, but we run a t-test between the two groups as well, using R.

Assumption checks: 
#Load necessary libraries

library(readxl)
library(ggplot2)
library(stats)

head(Book3)
female_salaries <- Book3$`Female Salaries`
male_salaries <- Book3$`Male Salaries`

shapiro_test_female <- shapiro.test(female_salaries)
shapiro_test_male <- shapiro.test(male_salaries)
print(shapiro_test_female)
print(shapiro_test_male)

ggplot(data.frame(female_salaries), aes(x = female_salaries)) +  geom_histogram(aes(y = ..density..), bins = 10, fill = "blue", alpha = 0.7) +  geom_density(color = "red") +  ggtitle("Histogram of Female Salaries") +  xlab("Female Salaries") +  ylab("Density")

qqnorm(female_salaries)qqline(female_salaries, col = "red")

ggplot(data.frame(male_salaries), aes(x = male_salaries)) +  geom_histogram(aes(y = ..density..), bins = 10, fill = "blue", alpha = 0.7) +  geom_density(color = "red") +  ggtitle("Histogram of Male Salaries") +  xlab("Male Salaries") +  ylab("Density")

qqnorm(male_salaries)
qqline(male_salaries, col = "red")

var_test <- var.test(female_salaries, male_salaries)
print(var_test)

#Perform the independent t-test

if(var_test$p.value > 0.05) {  t_test_result <- t.test(female_salaries, male_salaries, var.equal = TRUE)} else {  t_test_result <- t.test(female_salaries, male_salaries, var.equal = FALSE)}

print(t_test_result)

Shapiro-Wilk normality test data: female_salaries W = 0.93827, p-value = 0.6232
Shapiro-Wilk normality test data: male_salaries W = 0.93783, p-value = 0.6193
F test to compare two variances data: female_salaries and male_salaries F = 1.5908, num df = 6, denom df = 6, p-value = 0.587 alternative hypothesis: true ratio of variances is not equal to 1 95 percent confidence interval: 0.273347 9.258144 sample estimates: ratio of variances 1.590813 
Two Sample t-test data: female_salaries and male_salaries t = -0.22754, df = 12, p-value = 0.8238 alternative hypothesis: true difference in means is not equal to 0 95 percent confidence interval: -21519.56 17449.85 sample estimates: mean of x mean of y 131802.6 133837.5. 

As per the t-test results, wages between men and women under abovemtioned circusmtances are not statistically significant either.

#what-next

Curiosity got the best of us, so we decided to further investigate the demographics of the company. First things first, pivot tables were created which demonstrated that the average age of women and men is arguably the same, even in differing career stage levels. Also, females seem to earn more in advanced and associate position, while their male counterparts earn more on entry and senior-level positions.

# Race

Statistics about race unraveled many interesting interplays. Firstly, that the company is predomiantely Asian (40%), with 14% of Asians occupying senior positions, followed by latinos and caucasians. The most interesting part would concern the black workers. Even though they make up only 7% of the company, most of them, in both gender (2,2% for females and 2,6% for males) occupy senior positions. Especially for black females, they receive the biggest bonuses (11%) and seem to be, on average, the best paid race in the company, at an average salary of 141.860 euros annually accross departments, followed by 140,772 euros for Asian males. Caucasian females seem to be the best paid in accounting and latino females in marketing and sales. Latino males also seem to dominate on sales, caucasian males on accounting and finance, asian males in marketing, IT and engineering while the rest departments reward black employees the highest. Overall, this would prove a track record of solid professional progress for black women, as they seem to earn the most in most departments, despite accouting for just 4% of the workforce. 

# Countries

In any case, we are employees of a multinational organisation and we ought to decipher data on an international basis. We can see that our organisation conducts its' business on 3 countries: the United States of America, China and Brazil. First big thing to notice is that China offers more well-paid opportunities in Finance, Human Resources and IT. All salaries have been standarized by the way, so as to have comparable purchasing power metrics. Brazil and the US are tied, as both offer better salary opportunities in two departments each, Brazil in Engineering and Marketing and the US in Accounting and Sales, for both men and women, thereby establishing a clear country-to-salary trend. 
