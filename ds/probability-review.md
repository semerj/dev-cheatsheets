---
title: Probability Review
author: John Semerdjian
header-includes:
    - \usepackage{fancyhdr}
    - \usepackage{amsmath}
    - \pagestyle{fancy}
---

# Concepts

## Basics

The probability *P(A)* of event *A* is defined as the fraction of the outcomes in which *A* occurs, where *N* is the total number of outcomes of the experiment and *N(A)* is the number of outcomes leading to the occurrence of event *A*:

$$P(A) = {N(A) \over N}$$

### Sampling Table

The sampling table gives the number of possible samples of size *k* out of a population of size *n*, under various assumptions about how the sample is collected.

|                     | Order matters         | Not matter             |
| ------------------- |:---------------------:|:----------------------:|
| With replacement    | $n^k$                 | $\dbinom{n + k - 1}{k} = \dfrac{(n + k - 1)!}{k!(n - 1)!}$ |
| Without replacement | $\dfrac{n!}{(n - k)!}$| $\dbinom{n}{k} = \dfrac{n!}{k!(n-k)!}$ |

### Identities

$$
\begin{aligned}
0! &= 1! = 1 \\
\dbinom{n}{0} &= 1 \\
\dbinom{n}{1} &= n \\
\dbinom{k + 1}{k} &= \dbinom{k + 1}{1} \\
\dbinom{n}{k} &= \dbinom{n}{n - k} \\
n \dbinom{n - 1}{k - 1} &= k\dbinom{n}{k} 
\end{aligned}
$$

## Combination of Events

$$
\begin{aligned}
P(A^c) &= 1 - P(A) \\
P(A - B) &= P(A) - P(A \cap B) \\
\end{aligned}
$$

$$\text{If } A \subset B, \ \text{ then } P(A) \le P(B)$$

### Unions via Inclusion-Exclusion

$$
\begin{aligned}
P(A \cup B) &= P(A) + P(B) - P(A \cap B) \\
P(A \cup B \cup C) &= P(A) + P(B) + P(C) \\
&- P(A \cap B) - P(A \cap C) - P(B \cap C) \\&+ P(A \cap B \cap C) \\
\end{aligned}
$$

## Independence

$$
\begin{aligned}
P(A \cap B) &= P(A) P(B) \\
P(A^c \cap B^c) &= 1 - P(A \cup B) \\
\end{aligned}
$$

Note: different from disjointness: if A occurs, B cannot occur.

## Conditional Probability

$$P(A \cap B) = P(A \mid B) P(B) = P(B \mid A) P(A)$$

### Bayes Rule

$$
\begin{aligned}
P(A \mid B) &= \dfrac{P(B \mid A) \, P(A)}{P(B)} = \dfrac{P(B \mid A) \, P(A)}{P(B \mid A)P(A) + P(B \mid A^c) P(A^c)} \\
P(A \mid B, E) &= \underbrace{\dfrac{P(B \mid A, E)P(A \mid E)}{P(B \mid E)}}_{\text{Bayes' rule with extra conditioning}} \\
\underbrace{\dfrac{P(A \mid B)}{P(A^c \mid B)}}_{\text{posterior odds}} &= \underbrace{\dfrac{P(B \mid A)}{P(B \mid A^c)}}_{\text{prior odds}} \underbrace{\dfrac{P(A)}{P(A^c)}}_{\text{likelihood ratio}} \\
P(A_1, ..., A_n) &= P(A_1)P(A_2 \mid A_1)P(A_3 \mid A_1,A_2),...,P(A_n \mid A_1, ..., A_{n-1})
\end{aligned}
$$

### Law of total probability

aka Breaking a problem into simpler pieces.

Let $A_1,...,A_n$ be a partition of the sample space S (i.e., the $A_i$ are disjoint events and their union is S), with $P(A_i) > 0$ for all i. Then:

$$
\begin{aligned}
P(B) &= \sum^n_{i=1} P(B \mid A_i)P(A_i) \\
&= {P(B \mid A)P(A) + P(B \mid A^c) P(A^c)} \\
\end{aligned}
$$

Relates conditional probability to unconditional probability. It is essential for fulfilling the promise that conditional probability can be used to decompose complicated probability problems into simpler pieces, and it is often used in tandem with Bayes' rule.

### Law of total probability with extra conditioning

$$P(B \mid E) = \sum^n_{i=1} P(B \mid A_i, E)P(A_i \mid E)$$

### Conditional Independence

Events A, B are conditionally independent given C if 

$$P(A \cap B \cap C) = P(A \mid C)P(B \mid C)$$

Does conditional independence given C imply unconditional independence? NO!

* Ex: chess player of unknown strength. If you win first 5 games, you know you're a better player from the earlier games. Independence tells you that playing games with this person gives you no information about their strength to predict the outcome of later games. Thus, it may be that games outcomes are conditionally independent (given strength of opponent) but NOT independent unconditionally.

Does independence imply conditional independence given C? NO!

* Ex: A: fire alarm goes off is caused by F: fire or C: making popcorn. Suppose that F and C are independent, but $P(F \mid A, C^c)$ = 1. If the alarm goes off, then there are only 2 explanations, F or C. They are independent but NOT conditionally independent given that alarm goes off. As soon as alarm goes off, they become dependent given A.

### Prosecutor's Fallacy

$$P(\text{innocence} \mid \text{evidence}) \neq P(\text{evidence} \mid \text{innocence})$$

## Random Variables

### Bernoulli

$$
\begin{aligned}
\text{P.M.F} &: \begin{cases} 1-p & \text{for }k=0 \\ p & \text{for }k=1    \end{cases} \\
\text{mean} &: p\\
\text{variance} &: p(1-p) \\
\end{aligned}
$$

### Binomial

$$X \sim \text{Bin}(n, p)$$

X is the # of successes in *n* independent Bernoulli *p* trails, where *p* is the probability of success. The Binomial Formula is $P(\text{exactly k successes in n attempts})$.

$$
\begin{aligned}
\text{PMF, Binomial Formula} &: P(X = k) = \dbinom{n}{k} p^k (1-p)^{n-k} \\
\text{mean} &: np \\
\text{variance} &: np(1-p) \\
\end{aligned}
$$

If X and Y are independent, ($X \sim \text{Bin}(n, p) \ Y \sim \text{Bin}(m, p)$), then 

$$X + Y \sim \text{Bin}(n + m, p)$$

### Geometric

The number of failures until the first success with success probability *p* in *k* trials:

$$P(X = k) = (1-p)^k p, \text{ for k } = 1,2,3$$


# Questions: Basic

## 1. Subway Cars

*A subway train of n cars is boarded by k passengers at random. What is the probability of the passengers all ending up in different cars?*

There are $n^k$ ($n$ = cars, $k$ = people) distinct arrangements of passengers in cars.

$$P(A) = {n(n-1) ... (n-k+1) \over n^k}$$

## 2. Quality Control

*A batch of 100 items is checked by an inspector, who examines 10 items selected at random. If none of the 10 items is defective, they accept the whole batch. Otherwise, they batch is subjected to further inspection. What is the probability that a batch containing 10 defective items will be accepted?*

The number of ways of selecting 10 items out of a batch of 100 equals the number of combinations of 100 things taken 10 at a time:

$$N = \dbinom{100}{10} = \dfrac{100!}{{10! \ 90!}}$$

By hypothesis, these combinations are equiprobable (items selected at random). Let *A* be the event that "the batch of items is accepted by the inspector." Then *A* occurs whenever all 10 items belong to the set of 90 items of acceptable quality. Hence the number of combinations favorable to *A* is:

$$N(A) = \dbinom{90}{10} = \dfrac{90!}{{10! \ 80!}}$$

The probability of event *A*, i.e., of the batch being accepted, equals:

$$P(A) = \dfrac{N(A)}{N} = \dfrac{\dbinom{90}{10}}{\dbinom{100}{10}} = \dfrac{80!}{90!}\dfrac{90!}{100!} = \bigg(1 - \dfrac{1}{10} \bigg)^{10} \approx \dfrac{1}{e}$$

## 3. Both Aces

*What is the probability that 2 playing cards picked at random from a full deck are both aces?*

There are $\dbinom{52}{2} = 1326$ ways of selecting a pair of cards from the deck. Of these 1326 pairs, there are $\dbinom{4}{2} = 6$ consisting of 2 of aces.

$$\dfrac{\dbinom{4}{2}}{\dbinom{52}{2}} = \dfrac{6}{1326}$$

## 4. Permutation

*How many ways are there to permute the letters in the word STATISTICS?*

* Approach 1: We could choose where to put the S’s, then where to put the T’s (from the remaining positions), then where to put the I’s, then where to put the A (and then the C is determined). 
* Approach 2: Alternatively, we can start with 10! and then adjust for overcounting, dividing by 3! 3! 2! to account for the fact that the S’s can be permuted among themselves in any way, and likewise for the T’s and I’s.

$$\dbinom{10}{3}\dbinom{7}{3}\dbinom{4}{2}\dbinom{2}{1} = \dfrac{10!}{3! \ 3! \ 2!} = 50400$$

## 5. Split into Teams

*How many ways can you split 12 people into 3 teams of 4?*

Suppose we line up the people in a line and choose the first 4 to belong to group 1, the next 4 to group 2 and the last 4 to group 3. There are 12! ways of lining of people. However, the order of people in each group does not matter. Thus, for each line up, we have 4! 4! 4! ways of arranging people in each group. Therefore, the number of ways to arrange people in 3 equal groups is $\dfrac{12!}{4!4!4!}$. But labeling of the groups also does not matter. We can label the group 1 to be group 2 and group 2 to be group 3 and so on. We have 3! ways to label the groups: $\dfrac{12!}{4!4!4!3!}$.

Alternatively:

* There are $\dbinom{12}{4}$ ways to choose 4 students for group ONE,
* Now, there are $\dbinom{8}{4}$ ways to choose 4 students for group TWO,
* That leaves us with only 4 students, of $\dbinom{4}{4} = 1$ way to choose the students for group THREE: $\dbinom{12}{4}\dbinom{8}{4}\dbinom{4}{4} = \dfrac{12!}{4!4!4!}$

Now we need to divide that total by 3!, since the ordering of the groups doesn't matter, therefore $\dfrac{12!}{4!4!4!3!}$.

## 6. Lyft vs Uber

*If I call 2 Ubers and 3 Lyfts, what is the probability that all the Lyfts arrive first?*

$$\bigg( \dfrac{3}{5} \bigg) \bigg( \dfrac{2}{4} \bigg) \bigg( \dfrac{1}{3} \bigg) = \dfrac{1}{10}$$

## 7. Birthday Problem

*There are $k$ people in a room. Assume each person’s birthday is equally likely to be any of the 365 days of the year (we exclude February 29), and that people’s birthdays are independent. What is the probability that two or more people in the group have the same birthday?*

There are $365^k$ ways to assign birthdays to the people in the room (i.e. 365 days of the year being sampled $k$ times, with replacement).

Let’s count the complement: the number of ways to assign birthdays to *k* people such that NO two people share a birthday. This amounts to sampling the 365 days of the year *without* replacement, so the number of possibilities is $365 \cdot 364 \cdot 363 ... (365 - k + 1)$ for $k \le 365$. Therefore the probability of no birthday matches in a group of k people is:

$$P(\text{no birthday match}) = \dfrac{365 \cdot 364 \cdot 363 ... (365 - k + 1)}{365^k}$$

and the probability of at least one birthday match is:

$$P(\text{at least 1 birthday match}) = 1 - P(\text{no birthday match})$$

The first value of *k* for which the probability of a match exceeds 0.5 is $k = 23$. Of course, for $k = 366$ we are guaranteed to have a match.

Thinking about it in another way, note that with 23 people there are $\dbinom{23}{2} = 253$ *pairs* of people, any of which could be a birthday match.

## 8. Prius

*The probability of seeing at least one prius in a given hour is 64%. what’s the probability of seeing at least one prius in 2 hours? 30 mins?*

$$
\begin{aligned}
P(\text{not seeing any prius in 1 hour}) &= 1-0.64 = 0.36 \\
P(\text{not seeing any prius in 2 hours}) &= (1-0.64)^2 = 0.13 \\
P(\text{seeing at least one prius in 2 hours}) &= 1 - (1-0.64)^2 = 0.87 \\
P(\text{seeing at least one prius in 1/2 hours}) &= 1 - (1-0.64)^{1/2} = 0.40 \\
\end{aligned}
$$

## 9. Slot Machine

*1/2 of all slot machines in a casino payout 20% of the time, and the remaining half payout 30% of the time. What is the best strategy of maximizing your outcome?*

The probability of winning given that we've chosen a 30\% machine is:

$$P(\text{30\% machine} \mid \text{win on first pull}) = \dfrac{0.5 \times 0.3}{0.5 \times 0.3 + 0.5 + 0.2} = 0.6$$

Assume you play the same machine but lose this time. What is the new probability that this is a 30\% machine?

$$
\begin{aligned}
P(\text{30\% machine}) &= 0.6 \\
P(\text{20\% machine}) &= 0.4 \\
P(\text{lose} \mid \text{30\% machine}) &= 1-0.3 = 0.7 \\
P(\text{lose} \mid \text{20\% machine}) &= 1-0.2 = 0.8 \\
P( \text{30\% machine} \mid \text{lose on second pull}) &= \dfrac{0.6 \times 0.7}{0.6 \times 0.7 + 0.4 + 0.8} = 0.567
\end{aligned}
$$

Our best strategy would be to go from machine to machine, playing each one until we won. After winning on that machine, we can continue playing to see if it actually does pay out 30\% of the time.

## 10. Full House

*What is the probability of a full house (three of a kind and a pair) in a game of poker?*

Order doesn't matter:

$$\dfrac{13 \dbinom{4}{3} 12 \dbinom{4}{2}}{\dbinom{52}{5}}$$

## 11. Indistinguishable Particles

*How many ways are there to put k indistinguishable particles into n distinguishable boxes?*

$$\dbinom{n + k - 1}{k}$$

## 12. Bootstrap

*If we bootstrap n samples with replacement, what proportion of the original samples will we see?*

$$
\begin{aligned}
P(\text{not sampling the same item}) &= 1 - \dfrac{1}{n} \\
P(\text{not sampling the same item n times}) &= \bigg( 1 - \dfrac{1}{n} \bigg)^n \\
P(\text{sampling the same item n times}) &= 1 - \bigg( 1 - \dfrac{1}{n} \bigg)^n \approx 0.63
\end{aligned}
$$

# Questions: Combination of Events

## 13. Matching Problem

*There are n cards labeled 1,2,..., n. Let $A_j$ be the event "$j^{th}$ card matches". Find $P(\cup_{j=1}^N A_j)$.*

$$
\begin{aligned}
P(A_1 \cup A_2 \cup \ ... \ \cup A_n) &= {(n-k)! \over n!} \\
P(A_j) &= \dfrac{1}{n} \text{ since all positions are equally likely for card } A_j \\
P(A_1 \cup A_2) &= \dfrac{(n - 2)!}{n!} = \dfrac{1}{n(n-1)!} \\
\end{aligned}
$$
 
Apply inclusion/exclusion principle and you get 
 
$$
\begin{aligned}
P(A_1 \cup A_2 \cup \ ... \ \cup A_n) &= n \cdot \dfrac{1}{n} - \dfrac{n(n-1) }{2!} \dfrac{1}{n(n-1)} + \dfrac{n(n-1)(n-2)}{3!} \dfrac{1}{n(n-1)(n-2)} \\
&= 1 - \dfrac{1}{2!} + \dfrac{1}{3!} - \dfrac{1}{4!} + ... + (-1)^{n+1} \dfrac{1}{n!} \\
&= 1 - \dfrac{1}{e}
\end{aligned}
$$

# Questions: Independence

## 14. Newton-Pepys

*Probability of rolling AT LEAST one six in 6 rolls, two sixes in 12 rolls, or three sixes in 18 rolls. Which is most likely?*

$$
\begin{aligned}
P(A) &= 1 - \bigg( \dfrac{5}{6} \bigg)^6 \\
P(B) &= 1 - \bigg( \dfrac{5}{6} \bigg)^{12} - \underbrace{12 \bigg( \dfrac{1}{6} \bigg) \bigg( \dfrac{5}{6} \bigg)^{11}}_{\text{P(rolling exactly one 6)}} \\
P(C) &= 1 - \underbrace{\sum_{k=0}^2 \dbinom{18}{k} \dbinom{1}{6} \dbinom{5}{6}^{18 - k}}_{\text{P(exactly zero 6's), P(exactly one 6), or P(exactly two 6's)}} \\
\end{aligned}
$$

# Questions: Conditional Probability

## 15. Red Hearts

*A standard deck of cards is shuffed. Two cards are drawn randomly, one at a time without replacement. Let A = first card is a heart, and B = second card is red. Find $P(A \mid B)$ and $P(B \mid A)$.*

$$
\begin{aligned}
P(A \cap B) &= \bigg(\dfrac{13}{52}\bigg) \bigg(\dfrac{25}{51}\bigg) = \dfrac{25}{204} \\
P(B) &= \bigg(\dfrac{26}{52}\bigg) = \dfrac{1}{2} \\
P(A) &= \bigg(\dfrac{13}{52}\bigg) = \dfrac{1}{4} \\
P(A \mid B) &= \dfrac{P(A \cap B)}{P(B)} = \dfrac{25/204}{1/2} = \dfrac{25}{102} \\ 
P(B \mid A) &= \dfrac{P(A \cap B)}{P(A)} = \dfrac{25/204}{1/4} = \dfrac{25}{51} \\   
\end{aligned}
$$

## 16. At least one Ace

*Get random 2-card hand from standard deck. Find $P(\text{both Aces} \mid \text{have at least one Ace})$, $P(\text{both aces} \mid \text{have Ace of Spades})$*

$$
\begin{aligned}
P(\text{at least one ace}) &= 1 - \dfrac{\dbinom{48}{2}}{\dbinom{52}{2}} \\
P(\text{both aces } \cap \text{ at least one ace}) &= P(\text{both aces}) = \frac{\dbinom{4}{2}}{\dbinom{52}{2}} \\
P(\text{both aces} \mid \text{have at least one ace}) &= \dfrac{P(\text{both aces } \cap \text{ at least one ace})}{P(\text{at least one ace})} = \dfrac{1}{33} \\
P(\text{both aces} \mid \text{have ace of spades}) &= \dfrac{3}{51} = \dfrac{1}{17}
\end{aligned}
$$

## 17. At least one girl

*A family has two children, and it is known that at least one is a girl. What is the probability that both are girls, given this information? What if it is known that the elder child is a girl?*

$$
\begin{aligned}
P(\text{both girls} \mid \text{at least one girl}) &= \dfrac{P(\text{both girls, at least one girl})}{P(\text{at least one girl})} = \dfrac{\bigg(\dfrac{1}{2}\bigg)^2}{1-\bigg(\dfrac{1}{2}\bigg)^2} = \dfrac{1/4}{3/4} = \dfrac{1}{3} \\
P(\text{both girls} \mid \text{elder is a girl}) &= \dfrac{P(\text{both girls, elder is a girl})}{P(\text{elder is a girl})} = \dfrac{\bigg(\dfrac{1}{2}\bigg)^2}{\dfrac{1}{2}} = \dfrac{1}{2}
\end{aligned}
$$

## 18. A girl born in winter

*A family has two children. Find the probability that both children are girls, given that at least one of the two is a girl who was born in winter. Assume that the four seasons are equally likely and that gender is independent of season.*

$$P(\text{both girls} \mid \text{at least one winter girl}) = \dfrac{P(\text{both girls, at least one winter girl})}{P(\text{at least one winter girl})}$$

Denominator:

$$
\begin{aligned}
P(\text{at least one winter girl}) &= 1 - \bigg( \underbrace{1- \frac{1}{8}}_{\substack{P(\text{child is NOT a} \\ \text{winter-born girl})}} \bigg)^2
\end{aligned}
$$

Numerator:

Use the fact that "both girls, at least one winter girl" is the same event as "both girls, at least one winter child". Then use the assumption that gender and season are independent:

$$
\begin{aligned}
P(\text{both girls, at least one winter girl}) &= P(\text{both girls, at least one winter child}) \\
&= \underbrace{P(\text{both girls})}_{1/4} \underbrace{P(\text{at least one winter child})}_{P(1 - P(\text{both are non-winter}))} \\
&= \dfrac{1}{4}\bigg(1 - \bigg(\dfrac{3}{4}\bigg)^2\bigg) \\
\\
P(\text{both girls} \mid \text{at least one winter girl}) &= \dfrac{\dfrac{1}{4}\bigg(1 - \bigg(\dfrac{3}{4}\bigg)^2\bigg)}{1 - \bigg (\dfrac{7}{8} \bigg)^2} = \dfrac{7}{15}
\end{aligned}
$$

## 19. Breast Cancer

*1% of women have breast cancer (and therefore 99% do not). 80% of mammograms detect breast cancer when it is there (and therefore 20% miss it). 10% of mammograms detect breast cancer when it’s not there, a false positive (and therefore 90% correctly return a negative result). What is the probability that a positive test indicates an actual cancer?*

$$\begin{aligned}
P(\text{cancer} \mid \text{+test}) &= \dfrac{P(\text{+test} \mid \text{cancer})P(\text{cancer})}{P(\text{+test} \mid \text{cancer})P(\text{cancer}) + P(\text{+test} \mid \text{cancer}^c)P(\text{cancer}^c)} \\
&= \dfrac{0.8 \times 0.01 }{ (0.8 \times 0.01) + (0.10 \times 0.99) } \\
&= .075
\end{aligned}$$

## 20. Unfair Coin

*You are given a bag of 100 coins, with 99 fair ones flipping heads and tails with 0.5 probability each, and one unfair coin which flips heads with probability 1. You pick a coin randomly and flip it 10 times, getting heads every single time. What is the probability that you picked the unfair coin?*


$$
\begin{aligned}
P(A) &= 0.01 \\
P(A^c) &= 0.99 \\
P(B \mid A) &= 1 \\
P(B \mid A^c) &= \bigg( \dfrac{1}{2} \bigg)^{10}
\end{aligned}
$$

$$
\begin{aligned}
P(A \mid B) &= \dfrac{P(A)P(B \mid A)}{P(A)P(B \mid A) + P(A^c)P(B \mid A^c)} \\
&= \dfrac{1 \times 0.01}{ 1 \times 0.01 + 0.99 \times \bigg( \dfrac{1}{2} \bigg)^{10}} \\
&= 0.91
\end{aligned}
$$

## 21. Unfair Coin 2

*You have two coins in front of you. One of the coins is an unfair coin and will land heads 3/4 of the time. You pick up one of the coins and flip 3 heads in a row. What’s the chance that you’re holding the unfair coin?*

$$
\begin{aligned}
P(\text{picked up unfair coin}) = P(\text{picked up fair coin}) &= \dfrac{1}{2} \\
P(\text{flipped 3 heads in a row} \mid \text{coin is unfair}) &= \bigg( \dfrac{3}{4} \bigg)^{3} \\
P(\text{flipped 3 heads in a row} \mid \text{coin is fair}) &= \bigg( \dfrac{1}{2} \bigg)^{3} \\
P(\text{coin is unfair} \mid \text{flipped 3 heads in a row}) &= \frac{\bigg( \dfrac{3}{4} \bigg)^{3} \times \dfrac{1}{2}}{ \bigg( \bigg( \dfrac{3}{4} \bigg)^{3} \times \dfrac{1}{2} \bigg) + \bigg( \bigg( \dfrac{1}{2} \bigg)^{3} \times \dfrac{1}{2} \bigg)} \\
&= 0.77
\end{aligned}
$$

## 22. Interview

* *50% of all people who receive a first interview receive a second interview*
* *95% of your friends that got a second interview felt they had a good first interview*
* *75% of your friends that DID NOT get a second interview felt they had a good first interview*

*If you feel that you had a good first interview, what is the probability you will receive a second interview?*

* Pass: being invited to a second interview
* Fail: not being so invited
* Good: feel good about first interview
* Bad: don't feel good about first interview

$$
\begin{aligned}
P(\text{pass}) &= 0.5 \\
P(\text{pass}^c) &= 0.5 \\ 
P(\text{good} \mid \text{pass}) &= 0.95 \\
P(\text{good} \mid \text{pass}^c) &= 0.75 \\
P(\text{pass} \mid \text{good}) &= ? \\
\end{aligned}
$$

$$
\begin{aligned}
P(\text{pass} \mid \text{good}) &= \dfrac{P(\text{good} \mid \text{pass}) P(\text{pass})}{P(\text{good} \mid \text{pass}) P(\text{pass}) + P(\text{good} \mid \text{pass}^c) P(\text{pass}^c)} \\
&= \dfrac{0.95 \times 0.5}{0.95 \times 0.5 + 0.75 \times 0.5} \\
&= 0.559
\end{aligned}
$$

## 23. Monty Hall

*A contestant chooses one of three closed doors, two of which have a goat behind them and one of which has a car. One door is then opened and always has a goat behind it. The contestant is offered the option of switching to the other unopened door. Should the contestant switch doors?*

Suppose the contestant employs the switching strategy. If the car is behind door 1, then switching will fail, so $P(\text{get car} \mid C_1) = 0$. If the car is behind door 2 or 3, then because Monty always reveals a goat, the remaining unopened door must contain the car, so switching will succeed. Thus,

$$
\begin{aligned}
P(\text{get car}) &= P(\text{get car} \mid C_1) \cdot \dfrac{1}{3} + P(\text{get car} \mid C_2)\cdot \dfrac{1}{3} + P(\text{get car} \mid C_3) \cdot \dfrac{1}{3} \\
&= 0 \cdot \dfrac{1}{3} + 1 \cdot \dfrac{1}{3} + 1 \cdot \dfrac{1}{3} \\
&= \dfrac{2}{3}
\end{aligned}
$$

Switching strategy succeeds $\dfrac{2}{3}$ of the time vs. $\dfrac{1}{3}$ if they don't switch.

## 24. Simpson's Paradox

* $A$: event of a successful surgery
* $B$: were treated by Dr. Nick
* $B^c:$ treated by Dr. Hibbert
* $C$: heart survery
* $C^c$: band-aid removal

$$\text{Dr. Hibbert (H) and Dr. Nick (N)}$$

|         | H-heart | H-bandaid | | N-heart | N-bandaid |
|---------|:-------:|:---------:|-|:-------:|:---------:|
| success | 70      | 10        | | 2       | 81        |
| failure | 20      | 0         | | 8       | 9         |

$$
\begin{aligned}
P(A \mid B, C) &< P(A \mid B^c, C) \\
P(A \mid B, C^c) &< P(A \mid B^c, C^c) \\
P(A \mid B) &> P(A \mid B^c) \\
\end{aligned}
$$

$C$ is a "confounder" in this problem. If we fail to condition on the type of operation, we can get a very missleading answer, i.e., Dr. Nick is better than Dr. Hibbert.

## 25. Gambler's Ruin (Random Walk)

*Two gamblers, A and B, with a sequence of rounds where they bet a dollar each time. p = $P(\text{A wins a certain round})$, q = 1 - p. If one of the two gamblers goes bankrupt, then game is over. What is the probability that A wins the entire game, assuming A starts with $i$ dollars, B starts with $(N-i)$ dollars.*

Strategy: condition on first step.

Let $P_i = P(\text{A wins game} \mid \text{A starts at i})$

From the Law of Total Probability, we get this difference equation:

$$P_i = P P_{i+1} + q p_{i-1}, \ 1 \le i \le n - 1$$

Guess $P_i = x^i$ and solve

$$x^i = p x^{i+1} + q x^{i-1}$$

# Questions: Random Variables

## 26. Marbles

*Have b black, w white marbles. Pick simple random sample of size n. Find distribution of {# of white marbles} = X. (Sampling without replacement here, so trials are not independent and distribution is not binomial.)*

$$P.M.F. = P(X = k) = \dfrac{\dbinom{w}{k}\dbinom{b}{n - k}}{\dbinom{w + b}{n}}, \ 0 \le k \le w, \ 0 \le n - k \le b$$

This is called the hypergeometric distribution

## 27. Free Throws

$$
\begin{aligned}
P(\text{scores}) &= 0.7 \\
P(\text{miss}) &= 0.3 \\
P(\text{exactly 2 scores in 6 attempts}) &= \dbinom{6}{2}(0.7)^2(0.3)^4 \\
\end{aligned}
$$

## 28. Voter Survey

*A survey of 300 people was conducted to determine whether they would vote for Candidate A. 164 said they would for for Candidate A, while 136 said they would not. What is the 95% confidence interval for the proportion of people would vote for Candidate A?*

$$
\begin{aligned}
p &= \dfrac{164}{300} = 0.546 \\
SE &= \sqrt{\dfrac{p(1-p)}{n}} = \sqrt{\dfrac{0.546 (1-0.546)}{300}} = 0.028 \\
\end{aligned}
$$

We are confident that there is a 95% chance that a random sample mean is within 0.055 ($= 1.96 \times 0.028$) of our sample mean, 0.546, i.e. $54.6\% \pm 5.5$.