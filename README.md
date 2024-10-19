
# **CardioGood Fitness Treadmill Customer Profiling**

**Description**:  
This project was tasked with creating a customer profile for each CardioGood Fitness treadmill product line. The goal was to investigate customer characteristics, such as age, gender, marital status, fitness level, and income, to identify differences between product lines. The dataset contained data on customers who purchased a treadmill at a retail store during the previous three months.

**Key Skills Demonstrated**:
- Data cleaning and preprocessing
- Descriptive analytics using summary statistics
- Visualizing data through violin plots, bar charts, and scatter plots
- Conducting correlation analysis and linear regression models using the `fixest` package
- Working with categorical data and creating customer profiles

**Project Workflow**:

1. **Data Import and Cleaning**  
   Imported the `cardio_fitness.csv` dataset and filtered out missing or irrelevant values.

   ```r
   df <- read.csv(file = 'cardio_fitness.csv')

   df %>%
      filter(!is.na(Age) & !is.na(Education) & !is.na(Fitness) & 
             !is.na(Usage) & !is.na(Income) & !is.na(Miles))
   ```

2. **Summary Statistics**  
   Generated descriptive statistics for key variables (Age, Education, Fitness, Usage, Income, and Miles).

   ```r
   view_summary <- datasummary(df$Age + df$Education + df$Usage + df$Fitness + df$Income + df$Miles ~ 
                   Mean + Median + SD + Min + Max + P25 + P75 + N, data = df) 

   view_summary
   ```

3. **Data Visualization**  
   Visualized the relationships between treadmill usage and key variables such as Gender, Marital Status, and Age Group using `ggplot2`.

   Example: Violin plots for treadmill usage by gender and marital status.

   ```r
   gender_v <- ggplot(df, aes(y = Usage, x = Gender)) +
      geom_violin(linewidth = 1, width = 0.5, color = 'blue', fill = 'Orange', trim = TRUE, alpha = 0.3) +
      geom_boxplot(color = 'black', fill = 'lightblue', size = 0.5, width = 0.1, alpha = 0.5, outlier.shape = NA) +
      labs(x = 'Gender', y = 'Usage') + theme_bw()

   marital_box <- ggplot(df, aes(y = Usage, x = MaritalStatus)) +
      geom_violin(linewidth = 1, width = 0.5, color = 'blue', fill = 'lightyellow', trim = TRUE, alpha = 0.3) +
      geom_boxplot(color = 'red', fill = 'pink', size = 0.5, width = 0.1, alpha = 0.5, outlier.shape = NA) +
      labs(x = 'MaritalStatus', y = 'Usage') + theme_bw()

   grid.arrange(gender_v, marital_box, ncol = 2)
   ```

4. **Correlation Analysis**  
   Computed correlations between numeric variables, followed by a visualization of the correlation matrix.

   ```r
   corrplot(cor(df[, unlist(lapply(df, is.numeric))]), type = "upper", tl.col = "black")
   ```

5. **Regression Analysis**  
   Fitted regression models to understand the relationship between treadmill usage and customer characteristics (Income, Education, etc.).

   ```r
   Usage_Income <- feols(Usage ~ Income, data = df, vcov = 'hetero')
   Usage_Income_Education <- feols(Usage ~ Income + Education, data = df, vcov = 'hetero')

   etable(Usage_Income, Usage_Income_Education)
   ```

