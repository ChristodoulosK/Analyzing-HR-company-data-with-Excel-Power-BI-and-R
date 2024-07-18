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
