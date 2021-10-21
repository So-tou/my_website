---
categories:
- ""
- ""
date: "2017-10-31T21:28:43-05:00"
description: ""
draft: false
image: lbs.jpg
keywords: ""
slug: academicexperience
title: Academic experience
---

I am studying the Masters in Financial Analysis programme at London Business School. In this prestigious school, in the heart of London, I have the opportunity to deepen my knowledge in Finance in the most international school that exists. I am building a significant network that will help me to become an investment banker. I also have the huge opportunity to develop new skills, such as coding on R studio. I have the honour to learn to code during my favourite course, which is Data Analytics of course, taught by the best teacher ! 


After high school, I decide to study a dual bachelor degree in International Management and in Business Administrations. I studied two years at NEOMA Business School, in France and I studied in the University of International Business and Economics in Beijing, China. In the University of International Business and Economics, all the programme was taught in Mandarin. Thanks to this dual degree, I have a French diploma and Chinese diploma and I speak Mandarin fluently. 

##Group project##
Please find below one of my group's project that have been done for data analytics.

1 Youth Risk Behavior Surveillance
Every two years, the Centers for Disease Control and Prevention conduct the Youth Risk Behavior Surveillance System (YRBSS) survey, where it takes data from high schoolers (9th through 12th grade), to analyze health patterns. We work with a selected group of variables from a random sample of observations during one of the years the YRBSS was conducted.

1.1 Load the data
This data is part of the openintro textbook and we can load and inspect it. There are observations on 13 different variables, some categorical and some numerical.

data(yrbss) # loading the data
glimpse(yrbss) #having a glimpse on the data
## Rows: 13,583
## Columns: 13
## $ age                      <int> 14, 14, 15, 15, 15, 15, 15, 14, 15, 15, 15, 1~
## $ gender                   <chr> "female", "female", "female", "female", "fema~
## $ grade                    <chr> "9", "9", "9", "9", "9", "9", "9", "9", "9", ~
## $ hispanic                 <chr> "not", "not", "hispanic", "not", "not", "not"~
## $ race                     <chr> "Black or African American", "Black or Africa~
## $ height                   <dbl> NA, NA, 1.73, 1.60, 1.50, 1.57, 1.65, 1.88, 1~
## $ weight                   <dbl> NA, NA, 84.4, 55.8, 46.7, 67.1, 131.5, 71.2, ~
## $ helmet_12m               <chr> "never", "never", "never", "never", "did not ~
## $ text_while_driving_30d   <chr> "0", NA, "30", "0", "did not drive", "did not~
## $ physically_active_7d     <int> 4, 2, 7, 0, 2, 1, 4, 4, 5, 0, 0, 0, 4, 7, 7, ~
## $ hours_tv_per_school_day  <chr> "5+", "5+", "5+", "2", "3", "5+", "5+", "5+",~
## $ strength_training_7d     <int> 0, 0, 0, 0, 1, 0, 2, 0, 3, 0, 3, 0, 0, 7, 7, ~
## $ school_night_hours_sleep <chr> "8", "6", "<5", "6", "9", "8", "9", "6", "<5"~
Before you carry on with your analysis, it’s is always a good idea to check with skimr::skim() to get a feel for missing values, summary statistics of numerical variables, and a very rough histogram.

skimr::skim(yrbss)
Data summary
Name	yrbss
Number of rows	13583
Number of columns	13
_______________________	
Column type frequency:	
character	8
numeric	5
________________________	
Group variables	None
Variable type: character

skim_variable	n_missing	complete_rate	min	max	empty	n_unique	whitespace
gender	12	1.00	4	6	0	2	0
grade	79	0.99	1	5	0	5	0
hispanic	231	0.98	3	8	0	2	0
race	2805	0.79	5	41	0	5	0
helmet_12m	311	0.98	5	12	0	6	0
text_while_driving_30d	918	0.93	1	13	0	8	0
hours_tv_per_school_day	338	0.98	1	12	0	7	0
school_night_hours_sleep	1248	0.91	1	3	0	7	0
Variable type: numeric

skim_variable	n_missing	complete_rate	mean	sd	p0	p25	p50	p75	p100	hist
age	77	0.99	16.16	1.26	12.00	15.0	16.00	17.00	18.00	<U+2581><U+2582><U+2585><U+2585><U+2587>
height	1004	0.93	1.69	0.10	1.27	1.6	1.68	1.78	2.11	<U+2581><U+2585><U+2587><U+2583><U+2581>
weight	1004	0.93	67.91	16.90	29.94	56.2	64.41	76.20	180.99	<U+2586><U+2587><U+2582><U+2581><U+2581>
physically_active_7d	273	0.98	3.90	2.56	0.00	2.0	4.00	7.00	7.00	<U+2586><U+2582><U+2585><U+2583><U+2587>
strength_training_7d	1176	0.91	2.95	2.58	0.00	0.0	3.00	5.00	7.00	<U+2587><U+2582><U+2585><U+2582><U+2585>
1.2 Exploratory Data Analysis
You will first start with analyzing the weight of participants in kilograms. Using visualization and summary statistics, describe the distribution of weights. How many observations are we missing weights from?

# Weights show a right-skewed distribution with mean = 67.9 and median = 64.4, and sd = 16.9
# The number of observations with missing weight are 1004

yrbss <- yrbss %>% 
  filter(!is.na(weight))

yrbss %>% 
  summarise_each(funs(mean, median, sd, max, min), weight)
## # A tibble: 1 x 5
##    mean median    sd   max   min
##   <dbl>  <dbl> <dbl> <dbl> <dbl>
## 1  67.9   64.4  16.9  181.  29.9
There are 1004 observations missing.

Next, consider the possible relationship between a high schooler’s weight and their physical activity. Plotting the data is a useful first step because it helps us quickly visualize trends, identify strong associations, and develop research questions.

Let’s create a new variable in the dataframe yrbss, called physical_3plus , which will be yes if they are physically active for at least 3 days a week, and no otherwise. You may also want to calculate the number and % of those who are and are not active for more than 3 days. RUse the count() function and see if you get the same results as group_by()... summarise()

yrbss <- yrbss %>% 
  mutate(physical_3plus = ifelse(yrbss$physically_active_7d > 2, "yes", "no"))

yrbss_plot <- yrbss %>% 
  mutate(physical_3plus = ifelse(yrbss$physically_active_7d > 2, "yes", "no")) %>%
  na.exclude()
ggplot(yrbss_plot, aes(x=weight, y=physical_3plus)) + geom_boxplot() + theme_bw()


Can you provide a 95% confidence interval for the population proportion of high schools that are NOT active 3 or more days per week?

# Creating new column for those kids who are physically active 3 or more days a week

yrbss <- yrbss %>% 
  filter(!is.na(physically_active_7d)) %>% 
  mutate(physical_3plus = ifelse(physically_active_7d >= 3, "Yes", "No"))

# Comparing "count()" and "group_by() + summarize()" functions: checking they yield the same result (and percentages)

yrbss %>% 
  count(physical_3plus) %>% 
  mutate(physical_3plus_perc = n / sum(n))
## # A tibble: 2 x 3
##   physical_3plus     n physical_3plus_perc
##   <chr>          <int>               <dbl>
## 1 No              4022               0.325
## 2 Yes             8342               0.675
yrbss %>% 
  group_by(physical_3plus) %>% 
  summarise(number = n()) %>% 
  mutate(physical_3plus_perc = number / sum(number))
## # A tibble: 2 x 3
##   physical_3plus number physical_3plus_perc
##   <chr>           <int>               <dbl>
## 1 No               4022               0.325
## 2 Yes              8342               0.675
1.3 Boxplot
Make a boxplot of physical_3plus vs. weight. Is there a relationship between these two variables? What did you expect and why?

ggplot(data = yrbss, mapping = aes(x = weight, y = physical_3plus), size = 0.6) +
  geom_boxplot()


# The boxplot shows the medians are comparable. Also, the 25th and the 75th quartiles of both plots do not differ too much. The interpretation here could be based on how people perceive the physical exercise: either as a way to lose weight or gain muscular strength, i.e. weight. Based on the data, those excersing for 3 or more days per week seem to be fit as there are more people who exercise having a median weight, whereas people who don't exercise are in higher weight than people who do.
1.4 Confidence Interval
Boxplots show how the medians of the two distributions compare, but we can also compare the means of the distributions using either a confidence interval or a hypothesis test. Note that when we calculate the mean, SD, etc. weight in these groups using the mean function, we must ignore any missing values by setting the na.rm = TRUE.

yrbss %>%
  group_by(physical_3plus) %>% 
  #grouping by physical activity as we test whether the weight differ 
  #for those who is invilved in physical activity and otherwise
  filter(!is.na(physical_3plus)) %>% #filtering out missing data
  summarise(mean_weight = mean(weight, na.rm = TRUE), #calculating necessary statistics
            sd_weight = sd(weight, na.rm=TRUE), #calculating necessary statistics
            count = n(),
            se_weight = sd_weight/sqrt(count), #calculating necessary statistics
            t_critical = qt(0.975, count-1), #calculating necessary statistics for 95% CI
            margin_of_error = t_critical * se_weight, #calculating necessary statistics
            lower = mean_weight - t_critical * se_weight, #calculating CI
            upper = mean_weight + t_critical * se_weight #calculating CI
            )
## # A tibble: 2 x 9
##   physical_3plus mean_weight sd_weight count se_weight t_critical
##   <chr>                <dbl>     <dbl> <int>     <dbl>      <dbl>
## 1 No                    66.7      17.6  4022     0.278       1.96
## 2 Yes                   68.4      16.5  8342     0.180       1.96
## # ... with 3 more variables: margin_of_error <dbl>, lower <dbl>, upper <dbl>
There is an observed difference of about 1.77kg (68.44 - 66.67), and we notice that the two confidence intervals do not overlap. It seems that the difference is at least 95% statistically significant. Let us also conduct a hypothesis test to make sure that two different methods result in the same outcome.

1.5 Hypothesis test with formula
Write the null and alternative hypotheses for testing whether mean weights are different for those who exercise at least times a week and those who don’t.*

t.test(weight ~ physical_3plus, data = yrbss) #conducting t-test
## 
##  Welch Two Sample t-test
## 
## data:  weight by physical_3plus
## t = -5, df = 7479, p-value = 9e-08
## alternative hypothesis: true difference in means between group No and group Yes is not equal to 0
## 95 percent confidence interval:
##  -2.42 -1.12
## sample estimates:
##  mean in group No mean in group Yes 
##              66.7              68.4
The confidence interval does not include zero, hence the difference in means is statistically significant. Therefore, running for 3 or more days per week is beneficial for your weight

1.6 Hypothesis test with infer
Next, we will introduce a new function, hypothesize, that falls into the infer workflow. You will use this method for conducting hypothesis tests.

But first, we need to initialize the test, which we will save as obs_diff.

obs_diff <- yrbss %>%
  specify(weight ~ physical_3plus) %>% 
  calculate(stat = "diff in means", order = c("Yes", "No")) 

obs_diff
## Response: weight (numeric)
## Explanatory: physical_3plus (factor)
## # A tibble: 1 x 1
##    stat
##   <dbl>
## 1  1.77
Notice how you can use the functions specify and calculate again like you did for calculating confidence intervals. Here, though, the statistic you are searching for is the difference in means, with the order being yes - no != 0.

After you have initialized the test, you need to simulate the test on the null distribution, which we will save as null.

null_dist <- yrbss %>%
  specify(weight ~ physical_3plus) %>% 
  hypothesize(null = "independence") %>% 
  generate(reps = 1000, type = "permute") %>% 
  calculate(stat = "diff in means", order = c("Yes", "No"))
null_dist
## Response: weight (numeric)
## Explanatory: physical_3plus (factor)
## Null Hypothesis: independence
## # A tibble: 1,000 x 2
##    replicate    stat
##        <int>   <dbl>
##  1         1  0.413 
##  2         2 -0.607 
##  3         3 -0.588 
##  4         4 -0.377 
##  5         5  0.518 
##  6         6 -0.435 
##  7         7 -0.222 
##  8         8 -0.278 
##  9         9  0.0912
## 10        10 -0.433 
## # ... with 990 more rows
Here, hypothesize is used to set the null hypothesis as a test for independence, i.e., that there is no difference between the two population means. In one sample cases, the null argument can be set to point to test a hypothesis relative to a point estimate.

Also, note that the type argument within generate is set to permute, which is the argument when generating a null distribution for a hypothesis test.

We can visualize this null distribution with the following code:

ggplot(data = null_dist, aes(x = stat)) +
  geom_histogram()


Now that the test is initialized and the null distribution formed, we can visualise to see how many of these null permutations have a difference of at least obs_stat of 1.77?

We can also calculate the p-value for your hypothesis test using the function infer::get_p_value().

null_dist %>% visualize() +
  shade_p_value(obs_stat = obs_diff, direction = "two-sided")


null_dist %>%
  get_p_value(obs_stat = obs_diff, direction = "two_sided")
## # A tibble: 1 x 1
##   p_value
##     <dbl>
## 1       0
This the standard workflow for performing hypothesis tests.

2 IMDB ratings: Differences between directors
Recall the IMBD ratings data. We explore whether the mean IMDB rating for Steven Spielberg and Tim Burton are the same or not. We have already calculated the confidence intervals for the mean ratings of these two directors and as you can see they overlap.



First, I would like you to reproduce this graph. You may find geom_errorbar() and geom_rect() useful.

In addition, you will run a hypothesis test. You should use both the t.test command and the infer package to simulate from a null distribution, where you assume zero difference between the two.

Before anything, write down the null and alternative hypotheses, as well as the resulting test statistic and the associated t-stat or p-value. At the end of the day, what do you conclude?

We conclude that the IMDB rating for Steven Spielberg is higher than Tim Burton with a respective mean of 7.57 and 6.93.

You can load the data and examine its structure

movies <- read_csv(here::here("data", "movies.csv"))
glimpse(movies)
## Rows: 2,961
## Columns: 11
## $ title               <chr> "Avatar", "Titanic", "Jurassic World", "The Avenge~
## $ genre               <chr> "Action", "Drama", "Action", "Action", "Action", "~
## $ director            <chr> "James Cameron", "James Cameron", "Colin Trevorrow~
## $ year                <dbl> 2009, 1997, 2015, 2012, 2008, 1999, 1977, 2015, 20~
## $ duration            <dbl> 178, 194, 124, 173, 152, 136, 125, 141, 164, 93, 1~
## $ gross               <dbl> 7.61e+08, 6.59e+08, 6.52e+08, 6.23e+08, 5.33e+08, ~
## $ budget              <dbl> 2.37e+08, 2.00e+08, 1.50e+08, 2.20e+08, 1.85e+08, ~
## $ cast_facebook_likes <dbl> 4834, 45223, 8458, 87697, 57802, 37723, 13485, 920~
## $ votes               <dbl> 886204, 793059, 418214, 995415, 1676169, 534658, 9~
## $ reviews             <dbl> 3777, 2843, 1934, 2425, 5312, 3917, 1752, 1752, 35~
## $ rating              <dbl> 7.9, 7.7, 7.0, 8.1, 9.0, 6.5, 8.7, 7.5, 8.5, 7.2, ~
Your R code and analysis should go here. If you want to insert a blank chunk of R code you can just hit Ctrl/Cmd+Alt+I

table <- movies %>% 
  

  filter(!is.na(rating), director == 'Steven Spielberg' | director == 'Tim Burton') %>%
  group_by(director) %>% 
  
  summarise(mean_1= mean(rating), mean = round(mean_1, digits = 2), sd= sd(rating), t_critical=qt(.95, n() - 1), standard_error=sd(movies$rating)/sqrt(n()), lower_95_1 = mean - t_critical*(standard_error), upper_95_1 = mean + t_critical*(standard_error), lower_95= round(lower_95_1, digits = 2),upper_95= round(upper_95_1, digits = 2) )

table
## # A tibble: 2 x 10
##   director         mean_1  mean    sd t_critical standard_error lower_95_1 upper_95_1
##   <chr>             <dbl> <dbl> <dbl>      <dbl>          <dbl>      <dbl>      <dbl>
## 1 Steven Spielberg   7.57  7.57 0.695       1.72          0.219       7.19       7.95
## 2 Tim Burton         6.93  6.93 0.749       1.75          0.263       6.47       7.39
## # ... with 2 more variables: lower_95 <dbl>, upper_95 <dbl>
CI_lower <- table %>% 
  filter(director == 'Steven Spielberg') %>% 
  select(lower_95)

CI_higher <- table %>% 
  filter(director == 'Tim Burton') %>% 
  select(upper_95)

movies_plot <- table %>% 

  ggplot(aes(x = mean, y = reorder(director, mean), color = director, xmin = lower_95, xmax = upper_95)) +
  
  geom_errorbar(aes(xmin=lower_95, xmax=upper_95, color=director), width=0.1, size=1.5, scales = "free") +
  geom_point(aes(x = mean, y = director, color = director, size = 1.7))+

  geom_rect(aes(xmin = CI_lower$lower_95, xmax = CI_higher$upper_95, ymin= -Inf, ymax=Inf), alpha=0.2, colour = "transparent")+
  
  geom_text(aes(label=mean), color= "black", size=5, vjust=-0.8)+
  geom_text(aes(label=upper_95), color= "black", size=3, vjust=-2, hjust=-6)+
  geom_text(aes(label=lower_95), color= "black", size=3, vjust=-2, hjust=7)+
  theme_minimal()+
  labs(title= "Do Spielberg and Burton have the same mean IMDB ratings ?",
               subtitle = "95% confidence intervals overlap\n",
               x="Mean IMDB Rating",
               y="")+
  theme(plot.title = element_text(family = "Helvetica", face = "bold", size = (12)),
          axis.title = element_text(family = "Helvetica", size = (8)),
          axis.text = element_text(family = "Helvetica", size = (8)))

movies_plot + theme(legend.position="none")


3 Omega Group plc- Pay Discrimination
At the last board meeting of Omega Group Plc., the headquarters of a large multinational company, the issue was raised that women were being discriminated in the company, in the sense that the salaries were not the same for male and female executives. A quick analysis of a sample of 50 employees (of which 24 men and 26 women) revealed that the average salary for men was about 8,700 higher than for women. This seemed like a considerable difference, so it was decided that a further analysis of the company salaries was warranted.

You are asked to carry out the analysis. The objective is to find out whether there is indeed a significant difference between the salaries of men and women, and whether the difference is due to discrimination or whether it is based on another, possibly valid, determining factor.

3.1 Loading the data
omega <- read_csv(here::here("data", "omega.csv"))
glimpse(omega) # examine the data frame
## Rows: 50
## Columns: 3
## $ salary     <dbl> 81894, 69517, 68589, 74881, 65598, 76840, 78800, 70033, 635~
## $ gender     <chr> "male", "male", "male", "male", "male", "male", "male", "ma~
## $ experience <dbl> 16, 25, 15, 33, 16, 19, 32, 34, 1, 44, 7, 14, 33, 19, 24, 3~
The data frame omega contains the salaries for the sample of 50 executives in the company. We are going to run the following analyses: Confidence intervals, Hypothesis testing, Correlation analysis, Regression. Then we are going to look at whether there is a significant difference between the salaries of the male and female executives, and check whether they all lead to the same conclusion.

3.2 Relationship Salary - Gender ?
The data frame omega contains the salaries for the sample of 50 executives in the company. Can you conclude that there is a significant difference between the salaries of the male and female executives?

Note that you can perform different types of analyses, and check whether they all lead to the same conclusion

. Confidence intervals . Hypothesis testing . Correlation analysis . Regression

Calculate summary statistics on salary by gender. Also, create and print a dataframe where, for each gender, you show the mean, SD, sample size, the t-critical, the SE, the margin of error, and the low/high endpoints of a 95% condifence interval

# Summary statistics of salary by gender
salary_gender <- mosaic::favstats (salary ~ gender, data=omega)

salary_gender <- salary_gender %>%
                mutate(t_critical=qt(.975, n-1), standard_error=sd/sqrt(n), upper_95= t_critical +  mean*standard_error, lower_95 = t_critical - mean*standard_error) %>% 
                select(gender, mean, sd, n, t_critical, standard_error, lower_95, upper_95)

salary_gender
##   gender  mean   sd  n t_critical standard_error  lower_95 upper_95
## 1 female 64543 7567 26       2.06           1484 -9.58e+07 9.58e+07
## 2   male 73239 7463 24       2.07           1523 -1.12e+08 1.12e+08
omega %>% 
  ggplot(aes(x = gender, y = salary)) +
  geom_boxplot() +
  labs(x = "",
       title = "Mean salary of men is higher than women!",
       subtitle = "Boxplots of salary for male and female") +
  theme_minimal()


What can you conclude from your analysis? A couple of sentences would be enough

It goes without saying that there is a difference of 95%. We can observe that men salaries are closer to the data set mean, so men are more paid than women. It can be seen that executives women mean salary is dramatically lower than the executives men mean salary, with a difference of 8596.

You can also run a hypothesis testing, assuming as a null hypothesis that the mean difference in salaries is zero, or that, on average, men and women make the same amount of money. You should tun your hypothesis testing using t.test() and with the simulation method from the infer package.

# hypothesis testing using t.test() 
t.test(salary ~ gender, data = omega)
## 
##  Welch Two Sample t-test
## 
## data:  salary by gender
## t = -4, df = 48, p-value = 2e-04
## alternative hypothesis: true difference in means between group female and group male is not equal to 0
## 95 percent confidence interval:
##  -12973  -4420
## sample estimates:
## mean in group female   mean in group male 
##                64543                73239
# hypothesis testing using infer package
set.seed(1234)
salary_gender2 <- omega %>% 
  specify(salary ~ gender) %>% 
  hypothesize(null="independence") %>% 
  generate(reps=1000, type = "permute") %>% 
  calculate(stat="diff in means", order = c("female", "male"))

salary_gender2 %>% visualise()


salary_gender2 %>% 
  get_p_value(obs_stat= 73200 - 64500, direction="both")
## # A tibble: 1 x 1
##   p_value
##     <dbl>
## 1       0
What can you conclude from your analysis? A couple of sentences would be enough

There is an obvious difference in salary means because the p-value is very close to 0 and t is equal to -4.

3.3 Relationship Experience - Gender?
At the board meeting, someone raised the issue that there was indeed a substantial difference between male and female salaries, but that this was attributable to other reasons such as differences in experience. A questionnaire send out to the 50 executives in the sample reveals that the average experience of the men is approximately 21 years, whereas the women only have about 7 years experience on average (see table below).

# Summary Statistics of salary by gender
favstats (experience ~ gender, data=omega)
##   gender min    Q1 median   Q3 max  mean    sd  n missing
## 1 female   0  0.25    3.0 14.0  29  7.38  8.51 26       0
## 2   male   1 15.75   19.5 31.2  44 21.12 10.92 24       0
Based on this evidence, can you conclude that there is a significant difference between the experience of the male and female executives? Perform similar analyses as in the previous section. Does your conclusion validate or endanger your conclusion about the difference in male and female salaries?

# hypothesis testing using t.test() 
t.test(experience ~ gender, data = omega)
## 
##  Welch Two Sample t-test
## 
## data:  experience by gender
## t = -5, df = 43, p-value = 1e-05
## alternative hypothesis: true difference in means between group female and group male is not equal to 0
## 95 percent confidence interval:
##  -19.35  -8.13
## sample estimates:
## mean in group female   mean in group male 
##                 7.38                21.12
# hypothesis testing using infer package
set.seed(1234)
salary_gender3 <- omega %>% 
  specify(experience ~ gender) %>% 
  hypothesize(null="independence") %>% 
  generate(reps=1000, type = "permute") %>% 
  calculate(stat="diff in means", order = c("female", "male"))

salary_gender3 %>% visualise()


salary_gender3 %>% 
  get_p_value(obs_stat= 73200 - 64500, direction="both")
## # A tibble: 1 x 1
##   p_value
##     <dbl>
## 1       0
set.seed(1234)

null_world2 <- omega %>% 
  specify(experience ~ gender) %>%
  hypothesize(null = "independence") %>%
  generate(reps = 1000, type = "permute") %>% 
  calculate(stat = "diff in means", order = c("male", "female"))

null_dist %>% visualize()


Based on this evidence there is a significant difference between the experience of male and female executives and this conclusion validates the conclusion about the difference in men and women salaries.

3.4 Relationship Salary - Experience ?
Someone at the meeting argues that clearly, a more thorough analysis of the relationship between salary and experience is required before any conclusion can be drawn about whether there is any gender-based salary discrimination in the company.

Analyse the relationship between salary and experience. Draw a scatterplot to visually inspect the data

ggplot(omega, aes(x = experience, y = salary, color = gender)) +
  geom_point() +
  geom_smooth() +
  theme_minimal() +
  labs(title = "Higher salary for men than women",
       subtitle = "Relationship between salary and experience based on gender")


3.5 Check correlations between the data
You can use GGally:ggpairs() to create a scatterplot and correlation matrix. Essentially, we change the order our variables will appear in and have the dependent variable (Y), salary, as last in our list. We then pipe the dataframe to ggpairs() with aes arguments to colour by gender and make ths plots somewhat transparent (alpha  = 0.3).

omega %>% 
  select(gender, experience, salary) %>% #order variables they will appear in ggpairs()
  ggpairs(aes(colour=gender, alpha = 0.3))+
  theme_bw()


Look at the salary vs experience scatterplot. What can you infer from this plot? Explain in a couple of sentences

We can infer that women are unequally paid. For instance, we can see that with the same amount of experience, women executives are less paid than men executives.

4 Challenge 1: Yield Curve inversion
Every so often, we hear warnings from commentators on the “inverted yield curve” and its predictive power with respect to recessions. An explainer what a inverted yield curve is can be found here. If you’d rather listen to something, here is a great podcast from NPR on yield curve indicators

In addition, many articles and commentators think that, e.g., Yield curve inversion is viewed as a harbinger of recession. One can always doubt whether inversions are truly a harbinger of recessions, and use the attached parable on yield curve inversions.



In our case we will look at US data and use the FRED database to download historical yield curve rates, and plot the yield curves since 1999 to see when the yield curves flatten. If you want to know more, a very nice article that explains the yield curve is and its inversion can be found here. At the end of this challenge you should produce this chart



First, we will load the yield curve data file that contains data on the yield curve since 1960-01-01

yield_curve <- read_csv(here::here("data", "yield_curve.csv"))

glimpse(yield_curve)
## Rows: 6,884
## Columns: 5
## $ date      <date> 1960-01-01, 1960-02-01, 1960-03-01, 1960-04-01, 1960-05-01,~
## $ series_id <chr> "TB3MS", "TB3MS", "TB3MS", "TB3MS", "TB3MS", "TB3MS", "TB3MS~
## $ value     <dbl> 4.35, 3.96, 3.31, 3.23, 3.29, 2.46, 2.30, 2.30, 2.48, 2.30, ~
## $ maturity  <chr> "3m", "3m", "3m", "3m", "3m", "3m", "3m", "3m", "3m", "3m", ~
## $ duration  <chr> "3-Month Treasury Bill", "3-Month Treasury Bill", "3-Month T~
Our dataframe yield_curve has five columns (variables):

date: already a date object
series_id: the FRED database ticker symbol
value: the actual yield on that date
maturity: a short hand for the maturity of the bond
duration: the duration, written out in all its glory!
4.1 Plotting the yield curve
This may seem long but it should be easy to produce the following three plots

4.1.1 Yields on US rates by duration since 1960


ggplot(data = yield_curve, mapping = aes(x = date, y = value, colour = factor(duration))) + 
  geom_line() +
  theme(
    plot.background = element_rect(fill = "white"),
    panel.background = element_rect(fill = "white", colour = "black"),
    strip.background = element_rect(fill = "grey90", colour = "black"),
    panel.grid.major = element_line(colour = "grey90", size = 0.2),
    panel.grid.minor = element_line(colour = "grey90", size = 0.2),
    legend.position = "none"
  ) +
  labs(title = "Yields on U.S. Treasury rates since 1960", 
       x = NULL, 
       y ="%",
       caption = "Source: St. Louis Federal Reserve Economic Database (FRED)") +
  facet_wrap(~ duration, ncol = 2, nrow = 5)


4.1.2 Monthly yields on US rates by duration since 1999 on a year-by-year basis


yield_curve2 <- yield_curve %>% 
  mutate(month = month(date, label = FALSE, abbr = TRUE),
         year = year(date)) %>% 
  group_by(year, duration) %>% 
  filter(year >= "1999")

yield_curve2$maturity <- factor(yield_curve2$maturity, levels = c("3m","6m","1y","2y","3y","5y","7y","10y","20y","30y"))

ggplot(data = yield_curve2, mapping = aes(x = maturity, y = value, group = month, colour = factor(year))) + 
  geom_line() +
  theme(
    plot.background = element_rect(fill = "white"),
    panel.background = element_rect(fill = "white", colour = "black"),
    strip.background = element_rect(fill = "grey90", colour = "black"),
    panel.grid.major = element_line(colour = "grey90", size = 0.2),
    panel.grid.minor = element_line(colour = "grey90", size = 0.2),
    legend.position = "none"
  ) +
  labs(title = "US Yield Curve",
       x = "Maturity",
       y ="Yield (%)",
       caption = "Source: St. Louis Federal Reserve Economic Database (FRED)") +
  facet_wrap(~ year, ncol = 4, nrow = 6)


4.1.3 3-month and 10-year yields since 1999


yield_curve3 <- yield_curve2 %>% 
  filter(maturity == "3m" | maturity == "10y")

ggplot(data = yield_curve3, mapping = aes(x = date, y = value, group = maturity, colour = duration)) +
  geom_line(aes(x = date), size = 0.4) +
  theme(
    plot.background = element_rect(fill = "white"),
    panel.background = element_rect(fill = "white", colour = "black"),
    strip.background = element_rect(fill = "grey90", colour = "black"),
    panel.grid.major = element_line(colour = "grey90", size = 0.2),
    panel.grid.minor = element_line(colour = "grey90", size = 0.2)
  ) +
  labs(title = "Yields on 3-month and 10-year US Treasury rates since 1999",
       x = NULL,
       y ="%",
       caption = "Source: St. Louis Federal Reserve Economic Database (FRED)")


According to Wikipedia’s list of recession in the United States, since 1999 there have been two recession in the US: between Mar 2001–Nov 2001 and between Dec 2007–June 2009. Does the yield curve seem to flatten before these recessions? Can a yield curve flattening really mean a recession is coming in the US? Since 1999, when did short-term (3 months) yield more than longer term (10 years) debt?

Besides calculating the spread (10year - 3months), there are a few things we need to do to produce our final plot

Setup data for US recessions
Superimpose recessions as the grey areas in our plot
Plot the spread between 30 years and 3 months as a blue/red ribbon, based on whether the spread is positive (blue) or negative(red)
For the first, the code below creates a dataframe with all US recessions since 1946
# get US recession dates after 1946 from Wikipedia 
# https://en.wikipedia.org/wiki/List_of_recessions_in_the_United_States

recessions <- tibble(
  from = c("1948-11-01", "1953-07-01", "1957-08-01", "1960-04-01", "1969-12-01", "1973-11-01", "1980-01-01","1981-07-01", "1990-07-01", "2001-03-01", "2007-12-01","2020-02-01"),  
  to = c("1949-10-01", "1954-05-01", "1958-04-01", "1961-02-01", "1970-11-01", "1975-03-01", "1980-07-01", "1982-11-01", "1991-03-01", "2001-11-01", "2009-06-01", "2020-04-30") 
  )  %>% 
  mutate(From = ymd(from), 
         To=ymd(to),
         duration_days = To-From)


recessions
## # A tibble: 12 x 5
##    from       to         From       To         duration_days
##    <chr>      <chr>      <date>     <date>     <drtn>       
##  1 1948-11-01 1949-10-01 1948-11-01 1949-10-01 334 days     
##  2 1953-07-01 1954-05-01 1953-07-01 1954-05-01 304 days     
##  3 1957-08-01 1958-04-01 1957-08-01 1958-04-01 243 days     
##  4 1960-04-01 1961-02-01 1960-04-01 1961-02-01 306 days     
##  5 1969-12-01 1970-11-01 1969-12-01 1970-11-01 335 days     
##  6 1973-11-01 1975-03-01 1973-11-01 1975-03-01 485 days     
##  7 1980-01-01 1980-07-01 1980-01-01 1980-07-01 182 days     
##  8 1981-07-01 1982-11-01 1981-07-01 1982-11-01 488 days     
##  9 1990-07-01 1991-03-01 1990-07-01 1991-03-01 243 days     
## 10 2001-03-01 2001-11-01 2001-03-01 2001-11-01 245 days     
## 11 2007-12-01 2009-06-01 2007-12-01 2009-06-01 548 days     
## 12 2020-02-01 2020-04-30 2020-02-01 2020-04-30  89 days
yield_curve4 <- yield_curve %>%
  filter(maturity == "3m")

yield_curve5 <- yield_curve %>% 
  filter(maturity == "10y")

colnames(yield_curve5) <- c("date", "series_id", "value1", "maturity1", "duration", "month", "year")

yield_curve6 <- cbind(yield_curve4, yield_curve5) %>% 
  select(1, 3, 4, 8, 8) %>% 
  mutate(yield_delta = value1 - value,
         positive = ifelse(value1 > value, yield_delta, 0),
         negative = ifelse(value1 < value, yield_delta, 0))

ggplot(data = yield_curve6, mapping = aes(x = date, y = yield_delta)) +
  geom_line(aes(x=date), size = 0.3) +
  geom_line(aes(y = 0), fill = "black", size = 0.3) +
  geom_ribbon(aes(ymin = 0, ymax = positive), fill = "cornflowerblue", alpha = 0.5) +
  geom_ribbon(aes(ymin = negative, ymax = 0), fill = "brown1", alpha = 0.5) +
  geom_rug(data = subset(yield_curve6, yield_curve6$yield_delta > 0), color = "cornflowerblue", sides = "b", alpha = 0.5) +
  geom_rug(data = subset(yield_curve6, yield_curve6$yield_delta < 0), color = "brown1", sides = "b", alpha = 0.5) +
  annotate("rect", xmin = as.Date("1960-04-01"), xmax = as.Date("1961-02-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("1969-12-01"), xmax = as.Date("1970-11-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("1973-11-01"), xmax = as.Date("1975-03-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("1980-01-01"), xmax = as.Date("1980-07-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("1981-07-01"), xmax = as.Date("1982-11-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("1990-07-01"), xmax = as.Date("1991-03-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("2001-03-01"), xmax = as.Date("2001-11-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("2007-12-01"), xmax = as.Date("2009-06-01"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) +
  annotate("rect", xmin = as.Date("2020-02-01"), xmax = as.Date("2020-04-30"), ymin = -Inf, ymax = Inf, fill = "grey50", alpha = 0.4) + 
  labs(title = "Yield Curve Inversion: 10-year minus 3-month U.S. Treasury rates", 
       subtitle = "Difference in % points, monthly averages. Shaded areas correspond to recessions",
       x = NULL, 
       y ="Difference (10 year - 3 month) yield in %",
       caption = "Source: St. Louis Federal Reserve Economic Database (FRED)") +
  theme(
    plot.background = element_rect(fill = "white"),
    panel.background = element_rect(fill = "white"),
    panel.grid.major = element_line(colour = "grey90", size = 0.2),
    panel.grid.minor = element_line(colour = "grey90", size = 0.2),
    axis.ticks = element_blank(),
    plot.subtitle = element_text(size = 10, color = "black")
  ) + 
  scale_x_date(breaks = "2 years", date_labels = "%Y")


To add the grey shaded areas corresponding to recessions, we use geom_rect()
To color the ribbons blue/red we must see whether the spread is positive or negative and then use geom_ribbon(). You should be familiar with this from last week’s homework on the excess weekly/monthly rentals of Santander Bikes in London.
5 Challenge 2: GDP components over time and among countries
At the risk of oversimplifying things, the main components of gross domestic product, GDP are personal consumption (C), business investment (I), government spending (G) and net exports (exports - imports). You can read more about GDP and the different approaches in calculating at the Wikipedia GDP page.

The GDP data we will look at is from the United Nations’ National Accounts Main Aggregates Database, which contains estimates of total GDP and its components for all countries from 1970 to today. We will look at how GDP and its components have changed over time, and compare different countries and how much each component contributes to that country’s GDP. The file we will work with is GDP and its breakdown at constant 2010 prices in US Dollars and it has already been saved in the Data directory. Have a look at the Excel file to see how it is structured and organised

UN_GDP_data  <-  read_excel(here::here("data", "Download-GDPconstant-USD-countries.xls"), # Excel filename
                sheet="Download-GDPconstant-USD-countr", # Sheet name
                skip=2) # Number of rows to skip
The first thing you need to do is to tidy the data, as it is in wide format and you must make it into long, tidy format. Please express all figures in billions (divide values by 1e9, or 109), and you want to rename the indicators into something shorter.

tidy_GDP_data <- UN_GDP_data %>% 
  # change the dataset to long format
  pivot_longer(cols = 4:51, #columns 4 to 51
               names_to = "Year",
               values_to = "Value") %>% 
  
  # express all figures in billions and rename the indicators into something shorter
  mutate(Value = Value / 1e9,
         Year = as.integer(Year),
         IndicatorName_short = gsub("\\(.*","",IndicatorName),
         IndicatorName_short = case_when(
           IndicatorName_short == "Gross Domestic Product " ~ "GDP",
           IndicatorName_short == "Imports of goods and services" ~ "Imports",
           IndicatorName_short == "Exports of goods and services" ~ "Exports",
           IndicatorName_short == "Household consumption expenditure " ~ "Household expenditure",
           IndicatorName_short == "General government final consumption expenditure" ~ "Government expenditure",
           TRUE ~ IndicatorName_short
         ))


glimpse(tidy_GDP_data)
## Rows: 176,880
## Columns: 6
## $ CountryID           <dbl> 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4,~
## $ Country             <chr> "Afghanistan", "Afghanistan", "Afghanistan", "Afgh~
## $ IndicatorName       <chr> "Final consumption expenditure", "Final consumptio~
## $ Year                <int> 1970, 1971, 1972, 1973, 1974, 1975, 1976, 1977, 19~
## $ Value               <dbl> 5.56, 5.33, 5.20, 5.75, 6.15, 6.32, 6.37, 6.90, 7.~
## $ IndicatorName_short <chr> "Final consumption expenditure", "Final consumptio~
# Let us compare GDP components for these 3 countries
country_list <- c("United States","India", "Germany")
First, can you produce this plot?



tidy_GDP_data %>%
  # filter country and components first
  filter(Country %in% country_list & 
           IndicatorName_short %in% c('Gross capital formation','Exports','Government expenditure','Household expenditure','Imports')) %>% 
  # then start plotting
  ggplot(aes(x = Year, y = Value, colour = IndicatorName_short)) +
  # use line chart
  geom_line(size = 1.05) +
  # faceted by country
  facet_wrap(~Country) +
  # use bw theme
  theme_bw() +
  # add titles and other interpretaion info
    labs(
    title = "GDP components over time",
    subtitle = "In constant 2010 USD",
    x = "",
    y = "Billion US$"
  ) +
  # add the legend title
  guides(colour = guide_legend(title="Components of GDP"))  +
  # set the color for each component
  scale_colour_manual(
    values = c("Gross capital formation" = "#F8766D",
    "Exports" = "#A3A500",
    "Government expenditure" = "#00BF7D",
    "Household expenditure" = "#00B0F6",
    "Imports" = "#E76BF3"
  )) +
  # set the aspect ration fixed and the format of title and other elements
  theme(
    aspect.ratio = 3, 
    strip.text = element_text(size = 7),
    plot.subtitle = element_text(size = 8),
    axis.text.x = element_text(size = 6),
    axis.text.y = element_text(size = 6),
    plot.title = element_text(size = 12, face = 'bold')) +
  NULL


Secondly, recall that GDP is the sum of Household Expenditure (Consumption C), Gross Capital Formation (business investment I), Government Expenditure (G) and Net Exports (exports - imports). Even though there is an indicator Gross Domestic Product (GDP) in your dataframe, I would like you to calculate it given its components discussed above.

library(kableExtra)

tidy_GDP_data_2 <- tidy_GDP_data %>% 
  # delete the unnecessary column
  select(-IndicatorName) %>% 
  # Change the data from Long to Wide Format for simple calculations
  pivot_wider(names_from = IndicatorName_short, values_from = Value) %>% 
  # Then we calculate the Net Exports, GDP, and Percentage Change in the GDP
  mutate(`Net Exports` = Exports - Imports,
         `Calculated GDP` = `Household expenditure` + `Gross capital formation` + `Government expenditure` + `Net Exports`,
         `Percentage Change` = (`Calculated GDP` - `GDP`)/`GDP`)

# Styled Summary Statistics
mosaic::favstats(~`Percentage Change`, data=tidy_GDP_data_2) %>% 
  kbl(caption = "overall summary statistics for the % difference between calculated and included GDP") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
overall summary statistics for the % difference between calculated and included GDP
min	Q1	median	Q3	max	mean	sd	n	missing
-0.549	-0.021	0	0.022	1.16	0.004	0.087	9574	986
What is the % difference between what you calculated as GDP and the GDP figure included in the dataframe?

There is around 0% difference on average between the calculated GDP and GDP figure in the dataframe with a small standard deviation, which indicates they are roughly the same value.



What is this last chart telling you? Can you explain in a couple of paragraphs the different dynamic among these three countries?

Household Expenditure has accounted for the majority of the GDP for all three countries, and the components of their GDP have generally been the same. However, the trend is different.

For the proportion of Household Expenditures, India has decreased, Germany stays relatively constant and the U.S. has increased slightly. The reason could be U.S. people have more and more disposable income to spend as years go by and spend more money on daily shopping.

At the same time, the proportion of Gross Capital Formation experienced a surge in 2008, and it is probably due to the financial crisis. As the capital market broke down in the western countries, some foreign capital went into the Indian market and accounted for a large portion of the national GDP. As we can see, this portion has been going down as the economy recovers.

For Germany, the Net Exports has been taking a larger portion of GDP but is still below 10%. It represents that Germany government is probably developing their export strategy.

For the U.S., Gross Capital formation has taken a larger portion than Government Expenditure since 1995. The U.S. government always has a debt issue and has been purposely control their government expenditure.  

If you want to, please change country_list <- c("United States","India", "Germany") to include your own country and compare it with any two other countries you like

If we replace Germany with China, we can see there have been some changes. The GDP of China has been rapidly increasing since 2010 with capital formation in the leading position. The household expenditure has been exceeding capital formation until 2002. It is probably due to Chinese governments attention to domestic financial markets.


