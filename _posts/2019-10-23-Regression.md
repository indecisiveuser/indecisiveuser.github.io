---
layout: post
title: Regression Project
---
The aim of the Regression Project was to analyze a real-life dataset. Some comstraints were that the data set must include at least 1 categorical explanatory variable and that the prediction variable must be numerical. I chose to analyze recipes and reviews from Food.com to try to predict the popularity of the recipe based on the average rating, tags added to the recipe, ingredients etc. Once the variables were transformed so that their histograms resembled normal distributions, a RidgeCV regression was performed. Because of the great variability within each explanatory variable, only about 11% of the variance in the number of reviews was accounted for (R^2 ≈ 0.11).<br>

You can find my code located [here](https://github.com/indecisiveuser/Regression_Project)

	

