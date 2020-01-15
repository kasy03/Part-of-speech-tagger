## Training:
In the train(self, data) method, data consist of a list of tuples, wherein the tuple consist of the sentence and the corresponding tags. We use this data to learn the following

• transition probability P(Si | Si-1) where Si is the tag for the current word and Si-1 is the tag of the previous word in the sentence. For the first word of the sentence, the transition is stored as start which accounts for the number of times the tag occurred as the starting tag of a sentence.

• emission probability P(Wi | Si) is the probability of the word given a tag.

• Prior probability P(Si), the probability of a tag. These probabilities are stored in the class Solver object.

## Challenges:
The words that occur in the testing data but are not present in the training set, the probability of the emission of such words is set to a small value of 1e-10 and they are assigned ‘noun’ tag. This decision was made because even with the skeleton code assigning noun to all the words, we were getting decent accuracy. Also, nouns like person name, places cannot all possibly occur in the training set. The transition probability of Si-1 to Si not seen in the training data is set to 1/(total transitions to Si).

## Approach:
Naïve Bayes:
Naive bayes is the simplest approach, we consider each word independent of the other and this approach being as naive as it is still gives a decent accuracy. simplified(self, sentence): The formula P(Si | Wi) = P(Wi | Si) * P(Si) / P(Wi) where we ignore the P(Wi) as it is a constant factor. Therefore, P(Si | Wi) = P(Wi | Si) * P(Si) The one with the highest probability is considered as tag for the word in the sentence.

Viterbi:
hmm_viterbi(self, sentence): Viterbi is a dynamming program approach where we find the maximum a posteriori (MAP) probability to discover the optimal path. vt(t+1) = max(P(Si | Wi) * vt(t) * P(Si | Si-1)) where P(Si | Wi) is emission and P(Si | Si-1) is the transnition probability. The transition and emission probabilities calculated while training is used here. The words that do not exist are assigned a 'noun' tag and a small probability.

Gibbs Sampling:
complex_mcmc(self, sentence): Gibbs sampling is one of the techniques used under Markov chain Monte Carlo. We start from a random sample or some initial sample, in this case our initial sample consist of 'noun' tag for all the words in the sentence. We start with a burn-in period of 1000 iterations, consider the last sample as input to the next loop. All the samples from the burn-in period are discarded. We calculate the maximum of P(Si | Wi) * P(Si | Si-1), for all the words of the sentence, except for the last word of the sentence where we consider the last tag to have two parents, the previous tag as well as the first tag of the sentence which is given by P(Si | Wi) * P(Si | Si-1) * P(Si | S0).

Accuracy:
The accuracy obtained over the testing set of 2000 sentences is as below. Highest accuracy is obtained through viterbi, as we consider the transition as well as the previous probability.
[alt text](https://github.com/kasy03/Part-of-speech-tagger/blob/master/Accuracy.pngAccuracy.PNG)

