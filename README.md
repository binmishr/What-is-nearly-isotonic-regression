# What-is-nearly-isotonic-regression

The details of the codeset and plots are included in the attached Microsoft Word Document (.docx) file in this repository. 
You need to view the file in "Read Mode" to see the contents properly after downloading the same.

How is nearly-isotonic regression a generalization of isotonic regression? The term (\beta_i - \beta_{i+1})_+ is positive if and only if \beta_i > \beta_{i+1}, that is, if there is a monotonicity violation. The larger the violation, the larger the penalty. Instead of insisting on no violations at all, nearly-isotonic regression trades off the size of the violation with the improvement one gets from goodness of fit to the data. Nearly-isotonic regression gives us a series of fits that range from interpolation of the data (when \lambda = 0) to the isotonic regression fit (when \lambda = \infty). (Actually, you will get the isotonic regression fit once \lambda is big enough such that any change in the penalty cannot be mitigated by the goodness of fit improvement.)

Why might you want to use nearly-isotonic regression? One possible reason is to check if the assumption monotonicity is reasonable for your data. To do so, run nearly-isotonic regression with cross-validation on \lambda and compute the CV error for each \lambda value. If the CV error achieved by the isotonic regression fit (i.e. largest \lambda value) is close to or statistically indistinguishable from the minimum, that gives assurance that monotonicity is a reasonable assumption for your data.

