# Classifierplots

Generates a visualization of binary classifier performance as a grid of diagonstic plots with just one function call. Includes ROC curves, prediction density, accuracy, precision, recall and calibration plots, all using ggplot2 for easy modification.
Debug your binary classifiers faster and easier!

### Installation

    install.packages("devtools")
    devtools::install_github("ambiata/classifierplots")

### Usage

The main function to use when running R interactively is:

    classifierplots(test.y, pred.prob)

Where you pass in ground truth values test.y and predictions in [0,1] as pred.prob.

If you want to save the results to disk as folder of seperate plots as well as a single ALL.pdf grid, use

    classifierplots_folder(test.y, pred.prob, folder)
    
![Example](/man/figures/example.png?raw=true "Example")

### Reproducible Example 

    # load
    library(classifierplots)
    
    # data
    set.seed(100)
    N <- 500
    x <- runif(N, 0, 1)
    y <- round(x + rnorm(N, 0, 0.1))
    df <- data.frame(x, y)
    
    # train / test 
    i <- sample(seq_len(nrow(df)), size = floor(0.8 * nrow(df)))
    train <- df[i, ]
    test <- df[-i, ]
    
    # model
    model <- glm(y ~ x, family = binomial(logit), data = train)        
    test$pred <- predict(model, test, type = "response")
    
    # plot
    classifierplots(test.y = test$y, pred.prob = test$pred)
