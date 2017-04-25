# Jonathon Shlens

In 2014, Jon Shlens of Google Brain wrote a few articles on a few fundamental topics, and I've found them to nice refreshers of fundamental topics; they're less than 5 pages long, so the summaries are very short.

## A Light Discussion and Derivation of Entropy
Jon Shlens goes through a really derivation of entropy as the uncertainty in a set of discrete outcomes using three basic principles.

1. The more outcomes, the higher the uncertainty.
2. The relative likelihood of each outcome determines the uncertainty.
3. The weighted uncertainty of independent events must sum.

Using these three rules, Jon showed how Shannon derived the formula for entropy: -\sum_i p_i \log p_i.

## Notes on Kullback-Leibler Divergence and Likelihood Theory
Jon describes KL divergence as a foundational object in information theory and in likelihood theory. It can be interpreted in a number of ways:

1. (information theory) KL divergence quantifies, in bits, how far a probability distribution P is to a model distribution Q. In other words, how many more extra bits are needed to encode samples from P using a code that is optimized for Q.
2. KL divergence can be thought of as the number of yes-no questions needed to identify a sample drawn from a known distribution P.
3. It is the amount of information lost when Q is used to approximate P.
3. (likelihood theory) KL divergence is the negative log of the average multinomial log-likelihood.

Appendix A walks through the derivation of the formula of KL divegence, D_KL (p || q) as the sum of the negative entropy of P and the cross-entropy of P and Q:

D_KL (P || Q) = H(P, Q) - H(P).

Jon also relates KL divergence to mutual information, describing it as a measure of statistically independent the distributions P and Q are.

While not mentioned in the review, a nice connection with Bayesian statistics is that the KL divergence can be used to measure the information gain when moving from a prior distribution to a posterior distribution: P(X) --> P(X | I), where I is new information (see Wikipedia's KL Divergence article).
