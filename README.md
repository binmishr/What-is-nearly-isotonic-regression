# What-is-nearly-isotonic-regression

The details of the codeset and plots are included in the attached Microsoft Word Document (.docx) file in this repository. 
You need to view the file in "Read Mode" to see the contents properly after downloading the same.

How is nearly-isotonic regression a generalization of isotonic regression? The term (\beta_i - \beta_{i+1})_+ is positive if and only if \beta_i > \beta_{i+1}, that is, if there is a monotonicity violation. The larger the violation, the larger the penalty. Instead of insisting on no violations at all, nearly-isotonic regression trades off the size of the violation with the improvement one gets from goodness of fit to the data. Nearly-isotonic regression gives us a series of fits that range from interpolation of the data (when \lambda = 0) to the isotonic regression fit (when \lambda = \infty). (Actually, you will get the isotonic regression fit once \lambda is big enough such that any change in the penalty cannot be mitigated by the goodness of fit improvement.)

Why might you want to use nearly-isotonic regression? One possible reason is to check if the assumption monotonicity is reasonable for your data. To do so, run nearly-isotonic regression with cross-validation on \lambda and compute the CV error for each \lambda value. If the CV error achieved by the isotonic regression fit (i.e. largest \lambda value) is close to or statistically indistinguishable from the minimum, that gives assurance that monotonicity is a reasonable assumption for your data.

Full Code Here
===============

library(neariso)
library(tidyverse)
library(gganimate)

# generate data
set.seed(51)
n <- 30
x <- 1:n
y <- rnorm(n) + 0.2 * x
plot(x, y)

# perform nearly isotonic fit
fit <- neariso(y)
dim(fit$beta)

# make data frames for animation 
df <- data.frame(iter = rep(1:ncol(fit$beta), each = n),
                 x = rep(1:n, times = ncol(fit$beta)),
                 beta = c(fit$beta))
truth_df <- data.frame(x = x, y = y)
lambda <- round(fit$lambda, 2)

# animated plot: transitions don't seem great
p <- ggplot(df, aes(x = x, y = beta)) +
    geom_path(col = "blue") +
    geom_point(data = truth_df, aes(x = x, y = y), shape = 4) +
    labs(title = "Nearly isotonic regression fits",
         subtitle = paste("Lambda = ", "{lambda[as.integer(closest_state)]}")) +
    transition_states(iter, transition_length = 1, state_length = 2) +
    theme_bw() + 
    theme(plot.title = element_text(size = rel(1.5), face = "bold"))
animate(p, fps = 5)
#anim_save("neariso_animation2.gif")

# alternative method for making plot: you will have to uncomment the 
# lines below for it to work
library(magick)
for (idx in 1:ncol(fit$beta)) {
    ggplot(filter(df, iter == idx), aes(x = x, y = beta)) +
        geom_path(col = "blue") +
        geom_point(data = truth_df, aes(x = x, y = y), shape = 4) +
        labs(title = "Nearly isotonic regression fits",
             subtitle = paste("Lambda = ", lambda[idx])) +
        theme_bw() + 
        theme(plot.title = element_text(size = rel(1.5), face = "bold"))
    #ggsave(paste0("frame_", idx, ".pdf"))
}

# create the animation
files <- sapply(1:ncol(fit$beta), function(idx) paste0("frame_", idx, ".pdf"))
# image_read(files, density = 300) %>% image_animate(fps = 2) %>%
#     image_write(path = "neariso_animation.gif")

