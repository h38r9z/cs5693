java c Quantitative Methods for Policy Evaluation – Stata Assignment 
 
 This assignment guides you through the replication of a DID paper we’ve discussed in 
class, Card and Krueger (1994). A table containing their main results can be found at the 
end of the assignment (“Table 3”). Please note: 
1) You will need to download the data file «did.dta» from Blackboard. 
2) To ensure that all students can follow along with this assignment, almost all 
Stata commands required are provided to you in a step-by-step way. Some steps 
are accompanied by questions that are underlined. 
3) For submission, please submit your do-file (.do Stata file) containing all the 
commands you used for this assignment and a separate Word/Pdf files containing 
your written answers to the underlined questions. 
 
Open Stata, and make your own do-file 
• Start Stata through the start menu button 
• In de white command window type doedit to start de do-file editor. Place the Stata 
screen on the left and the do file editor on the right such that you can easily switch 
between the two. 
• Save the empty do-file as a new do-file under an applicable name such as 
ectrcs_did.do in a directory that you want to use for this course, for example H:\ectrcs 
• In the first two lines of the do-file type 
cls //this clears the screen 
clear all //this clears the memory 
cd "H:/ectrcs" //this is your path 
• The path you use can be customized and depends on whether you use a Mac 
cd "~/ectrcs" //this is your MAC path cd 
"c:/ectrcs" //this is your PC path 
 
Differences in Differences 
In this computer exercise we will analyze the relation between wages and employment 
using quasi experimental data. More in particular, we will estimate the impact of an 
increase in the minimum wage on low-skilled workers. Economic theory predicts that in 
a competitive market, demand will fall when prices increase. A minimum wage increase 
should thus lead to unemployment if the labor market is competitive. 
Until the 1980s empirical evidence of the impact of minimum wages on employment 
was provided by regression models of aggregated time series data. The dependent 
variable typically is country wide teenage employment,
1
 which is explained by a host of 
factors including minimum wages. Empirical results indicate that a 10% increase in the 
minimum wage leads to 1%-3% decrease in teenage employment. 
The main problems with the use of multiple regression models based on aggregated 
time series are (1) omitted variables; (2) measurement error; (3) simultaneous causality of 
 
1
 Given that it is mainly teenagers that earn wages close to the minimum wage, focusing on this group is more 
interesting than the total population that seeks employment.  
2 
employment (quantity) and wages (price). As a reaction on the aggregated time series 
approach Card and Krueger (1994), hereafter denoted as CK94, use quasi-experimental 
data. 
We will use the data of CK94 to replicate their main results (see below). Download the 
file did.dta. A description of all variables can be found in the file did.pdf. Available 
are data on employment in 410 fast food restaurants in New Jersey (NJ) and Pennsylvania 
(PA) before and after an increase in minimum wages in New Jersey on 1 April 1992. Load 
the data into Stata using the command use did.dta, clear 
 
1. The unit of observation is a particular restaurant. Total employment is defined as the 
number of full-time employees (inclusive management) plus 0.5 times the number of 
part-time employees. This is the dependent variable in the empirical analysis. 
Generate this variable for both periods (generate emptot1 = emppt*0.5 + 
empft + nmgrs and generate emptot2 = emppt2*0.5 + empft2 + nmgrs2). 
Period 1 corresponds to the pre-treatment period whereas period 2 is the posttreatment
period. Regress emptot1 on a constant term and the dummy variable state, 
which has the value 1 for a restaurant in the treatment group (state==1 if restaurant 
located in NJ) and zero for the control group (state==0 for PA restaurants). Do not 
forget to use heteroskedasticity-robust standard errors (i.e. type , robust). 
a What is the difference between the Treated and Control state outcome in period 
1? 
b Is this difference statistically significant? 
2. The estimated regression from question 1 is a simple regression model with a dummy 
variable regressor. The estimated coefficient of the constant term is equal to the sample 
average of the control group, hence in this case average employment in PA 
restaurants. The coefficient of the state dummy measures the difference in average 
employment between treatment group (NJ restaurants) and control group (PA 
restaurants) before the minimum wage increase (pretreatment). The estimation results 
are equal to those in row 1, columns (i) and (iii) of Table 3 in CK94. To reproduce row 
1,
2
 column (ii) generate statealt = 1 – state and type regress emptot1 
statealt, robust. 
a What does the coefficient of the constant and the variable statealt measure in this 
regression? 
b What is the relationship between the coefficients found in this question 
compared to the previous question? 
3. Proceed in the same way (but now use emptot2 as dependent variable) to generate the 
results in row 2, columns (i)-(iii) of Table 3. Basically what you get is the differences 
estimator: a comparison of post treatment (after the minimum wage increase) 
outcomes. The estimation results show a slightly negative employment effect of -0.14 
employees, although not significantly different from zero (standard error equals 1.07). 
If the zero conditional mean assumption holds we should get an unbiased estimator 
of the treatment effect. 
 
2
 Small differences with CK94 table 4 are due to rounding errors.  
3 
a Do you think that the estimated coefficient from this regression is a causal 
effect? Explain why or why not 
4. The differences-in-differences estimator will control for pretreatment characteristics 
by looking only at changes in employment (instead of the level of employment). 
Generate now employment changes (generate emptotd = emptot2 – emptot1) 
and regress emptotd on a constant and the state dummy. Again this is a simple 
regression and the coefficient of the constant term measures the average change in 
employment in PA restaurants. This coefficient equals –2.28 and is代 写data、Python/C++
代做程序编程语言 significant, i.e. 
employment has on average decreased in PA restaurants. The coefficient of the 
dummy regressor state measures the difference in average employment changes 
between NJ and PA restaurants. This coefficient is called the differences-in-differences 
(DID) estimator and equals 2.75 (row 4, column (iii)). Also we can deduct from these 
estimation results that the average employment change in NJ restaurants equals –
2.28+2.75=0.47 (row 4, column(ii)). Based on intuition and economic theory we would 
expect that an increase in the minimum wage leads to lower employment holding 
constant other relevant factors. Because we analyze employment changes over almost 
a year these other relevant factors are probably not constant. 
a What is the main assumption required to interpret the results from this 
question causally? 
5. The DID estimate in part 4. can also be obtained by regressing employment on a 
treatment indicator while including both time and restaurant fixed effects. In order to 
do this we have to “reshape” the data. First we keep only the restaurants in the data 
for which we have data on employment both before and after the change in the 
minimum wage, a so called balanced panel, by typing keep if emptot1!=.  
emptot2!=. We keep only the relevant variables in the data by typing keep 
restaurant_id state emptot1 emptot2. Next we create a dataset in long form 
by typing reshape long emptot, i( restaurant_id) j(time). 
a Explain what happened to the number of observations after the commands of 
this question 
b We dropped a number of observations to keep a balanced panel. Explain how 
the main results of the DID analysis would change if we proceeded with a 
unbalanced panel in this exercise. 
6. We are now going to obtain the DID estimator of the effect of the minimum wage on 
employment. We first let Stata know which variable indicates the entity (restaurant) 
by typing xtset restaurant_id. Next, we create a variable indicating that a 
restaurant has been treated by the minimum wage increase by typing gen 
treated=0 and next replace treated=1 if state==1  time==2. Obtain 
the DID estimator by typing xi: xtreg emptot treated i.time, fe robust. 
a Compare the results from the xtreg command to the results in part 4. 
7. It is also possible to obtain the DID estimator by regressing employment on the 
treatment indicator, a state dummy and a time dummy. Type xi: regress emptot 
treated i.state i.time, robust.  
4 
a Compare the estimated coefficient and the standard error on the variable 
treated with the estimates obtained in part 6. Why is the standard error in part 
6. smaller? 
8. We go back to the data set in “wide form” by typing use did.dta, clear. We 
have to create the employment variables again by typing 
generate emptot1 = emppt*0.5 + empft + nmgrs 
generate emptot2 = emppt2*0.5+empft2+nmgrs2 
generate emptotd = emptot2-emptot1 
9. So far the control group has been PA restaurants. CK94 also provide DID estimates for 
an alternative control group, i.e. NJ restaurants with a relatively high starting wage in 
the first period (before the minimum wage increase). They divide the restaurants in 
three groups, i.e. low (starting wage equals $4.25), middle (wage between $4.26 and 
$4.99) and high (wage higher than $4.99). Generate with the following Stata commands 
four binary variables representing the low, middle, and high categories as well as a 
category for which no starting wage is observed: 
generate low=0 
generate mid=0 
generate high=0 
generate dna=0 
replace low=1 if wage_st==4.25 
replace mid=1 if wage_st>4.25  wage_st=5.00 
replace dna=1 if wage_st==. 
10. Consider now the NJ restaurants only. There are in total 331 NJ observations 
(state==1) of which 101 restaurants have a starting wage equal to $4.25, 140 between 
$4.26 en $ 4.99 and 73 equal or higher than $5.00. Also 17 restaurants do not have 
reported a starting wage at all (dna==0), hence we leave them out of the analysis. 
Using the remaining 331-17=314 observations regress emptot on low, mid and high. 
The precise Stata command is regress emptot1 low mid high if state==1 
 dna==0, noconstant robust. 
a Explain why we added the option “noconstant” to the estimation command 
in this question. 
b What will Stata do if we do not use this option? 
11. From the Stata output table you see that 305 observations have been used in 
estimation. If you have specified all commands properly your estimates should equal 
those reported in row 1, columns (iv)-(vi) of Table 3 (slight differences are due to 
rounding errors). 
a What is the function of the “ dna==0” part of the regression command? 
b What command do you use to replicate the coefficients from columns (vii)-(viii) 
of Table 3? 
12. Perform the same regressions as in part 10., but now with emptot2 and emptotd as 
dependent variables. You will get the estimation results of rows 2 and 4, columns 
(iv)(vi). It is seen that employment increased in restaurants with a low initial starting 
wage. However, the opposite is true for the control group, i.e. restaurants with an 
initial high starting wage. CK94 judge these results as additional evidence for the  
5 
earlier surprising result that an increase in the minimum wage causes more 
employment. 
a Explain the intuition behind this analysis. What comparisons are being used to 
generate the results? What is the main assumption in this setting? 
13. To address the validity of the last regression analysis, type tabulate wage_st 
wage_st2 if state==1  dna==0  high==1 to show the joint frequency 
distribution for the control group of starting wages before and after the minimum 
wage increase. 
a What do you notice from the table generated? Explain whether you think that 
the table supports the validity of the regression from Question 10. 
 
Finally, we invite you to download the paper of Card  Krueger (1994) and have a look 
yourself at their other empirical results. As said before their main contribution to the 
minimum wage literature is to use quasi-experimental firm level data instead of 
observational aggregated time series data. Their empirical analysis does not go beyond 
the use of linear regression. Hence, you will be able to reproduce all their estimation 
output with your current knowledge. 
 
Card  Krueger (1994), Table 3 
 
         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
