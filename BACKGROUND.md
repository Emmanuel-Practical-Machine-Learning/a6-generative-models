# Background: Embeddings and Generative Models
Edited by Mark Sherman <shermanm@emmanuel.edu> from MIT 6.S198
The assignment is in [README.md](README.md)
# Visualizing datasets using the embedding projector

The experiments with Teachable Machine in assignment 1 relied on the attributes generated for images by MobileNet (1000 attributes for each image) to represent images as points in a high-dimensional space. We can say that the collection of images is "embedded" into a 1000-dimensional space. Similarly, we can embed other data sets into high-dimensional spaces. The first Teachable Machine assignment used k nearest neighbors to classify images. Beyond computing only nearest neighbors, we can use visualization tools to gain intuition about the structure of the points embedded in high-dimensional space. In this class, we'll be exploring such a tool: the Embedding Projector.

## How this Visualization Works

Projecting from the high-dimensional space down to three dimensions is done using Principal Component Analysis (PCA). PCA maps the data onto the dimensions along which the data has the greatest variation, the next greatest variation, and so on. In this case, we stop after the top three dimensions of variation. The result is that even though we can't visualize 784-dimensional space, we do get some sense of how the data is spread out.

Visit Victor Powell's PCA demo at <http://setosa.io/ev/principal-component-analysis/>. Toward the top of the screen, slide the 5 points around, and watch the resulting map onto the x-axis and the y-axis as the two principal components. You should see that when the points on the left spread out in some direction (any direction), the points on the right line up in a mostly horizontal direction. When the points on the left don't spread out in any preferred direction (e.g. they lie on a circle) then there's no preferred alignment on the right either. Similarly, for the embedding projector, the first three principal components map to x, y, and z. The extent to which the MNIST points align along these dimensions can provide some sense of how spread out they are in 784-space.

PCA is only one tool for visualizing high-dimensional data sets. Another one is t-SNE (pronounced "tee snee"), which stands for t-Distributed Stochastic Neighboring Entities. The idea of t-SNE is to choose 3 axes so that the distances between the projected points in 3 dimensions are close to the distances between the points the high-dimensional space. This is done by an iterative computation where the distance difference is reduced step by step. If you are interested, you can find information in "How to Use t-SNE Effectively" by Wattenberg, Viégas, and Johnson: <https://distill.pub/2016/misread-tsne/>.

## Problem 1
# Words as vectors
In the MNIST embedding, the placement of points comes from images and so is plausibly related to something geometric. But how about visualizing data sets of words? Is there any sense in which a set of words could have a geometry?

One intuitive approach to creating a geometry of words uses the insight that *two words should have similar attributes if they appear in the similar contexts* in some large text corpus. This is the idea behind the Word2Vec 10K data set in the Embedding Projector which shows an embedding of 10,000 words into 200-dimensional space.

Select the Word2Vec 10K data set in the embedding projector, and use PCA. 

Type in a word at the upper right. Below it, you'll see a list of the closest words in 200-dim space.

Adjust the slider to show 100 neighbors and look at some of the words that are shown in the cloud. Those words should be bolded in the 3D projector. 

You can click "Isolate 101 points" above the search bar to get rid of the other non-neighboring words. The picture below shows the result of entering the word "hello". You'll notice a lot of computer science terminology, which indicates that those terms frequently appear together in the text corpus that was used to create Word2Vec 10K data set.

![](https://lh4.googleusercontent.com/Lj6II2hv8op2zmkXw71pbXJQeu8l38843UwLmoRFt55ns3YFgP5szy6XVnbv6qVntOH2gvCqivG0vi9EX1QvJsUkhBYOT6tsLpjwW9BBf5UNixuoLCQeGyd_0klr2rulFKsMvQG5)

Try different keywords and look at the 100 nearest neighbors. Are these words plausibly related to the word you searched for? There's also a "Word2Vec All" data set of 70,000 words that you can experiment with.

You might think that the word embeddings are determined by some kind of dictionary search, but that's not the case at all. Rather, Word2Vec uses the frequency with which the 10,000 words appear together in a large corpus of Google News articles (billions of words altogether). This frequency is determined by choosing word sequences of a certain size (e.g., 6 words) seeing which pairs of words have high probability of appearing in the same sequence, then training a neural network to reflect that probability.

Word2Vec is created using a 3-layer network whose input is a 10,000 component vector, one for each vocabulary word. A given input word is represented by a vector with "1" in the component corresponding to that word, and "0" in all the other components. (This is called "1-hot encoding".) The output layer, also 10,000 components, one component for each word, will gives the probability that the input and output will be observed in the same word sequence. Between the input and output is a fully-connected hidden layer of width 500.

For example, suppose one of the sequences is "MIT researchers announced a physics breakthrough". Then, given the input word "announced", the sequence will contribute to higher probability for the output words "MIT", "researchers", "a", "breakthrough", "physics". This method of relating words is called the "Skip-Gram model".

The network is trained using each word sequence as a training example, and adjusting the weights of the hidden layer so that the probabilities in the output layer match the observed probabilities. Finally, given any word input to the network, it Word2Vec takes the 500 values in the hidden layer to be the 500-dimensional vector that represents the word.

# 1.3: Word geometry
## Problem 2
# Finding word analogies with vector algebra
## Problem 3
# Exploring fonts with the Embedding Projector