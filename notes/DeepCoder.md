## DeepCoder: Learning to Write Programs
##### Matej Balog, Alexander L. Gaunt, Marc Brockschmidt, Sebastian Nowozin, Daniel Tarlow (2016)

**Short Summary**: One way to algorithmically write programs that are consistent with input-output examples is to guide traditional program search techniques with hints as to what the program may look like. The model in this paper learns to do so by embedding the inputs and outputs and predicting a set of subroutines that should appear in the program that is then used by the underlying search techniques to find a correct program. The result is significant speed ups over baseline search techniques and generalizability to longer, unseen programs.

### Summary
There are (probably among others) two big lines of work in the area of algorithmically writing programs: one is the track of having differentiable interpreters, such as recent work with NTM, Neural RAM, Neural GPU, NPI, Neural Programmers, etc. This line of work seeks to develop a neural controller that learns an implicit model of how to produce the correct actions to yield the desired outcome, yet none of these models write programs. The other large line of work is from the programming languages community, where clever algorithms are employed to search efficiently through the space of programs given constraints, yet these methods are often carefully constructed and are not as easily amenable to new domains or problems. This paper is motivated by the strong promise of combining aspects from both approaches: incorporate learned features in the search algorithms, and allow the hybrid system to solve problems with potentially large speedups.

The problem is then formally stated as follows. The task in this paper is given pairs of inputs and outputs, produce a program that is consistent with all examples. Such an input and output pair, and a correct program might be

```
([0, 1, 2, 3, 4], [6]) --> sum(filter(lambda x : x % 2 == 0, input))
```

written in Python syntax, which is *not* what the paper uses. The paper defines a DSL for writing programs where the constructs included strike a balance between being concise enough to search over efficiently without sacrificing the expressive power of the language. For example, the same program above written in the custom DSL is as follows:

```
a <-- [int]
b <-- FILTER (%2==0) a
c <-- SUM b
```

The search algorithms, which are DFS, sort-and-add, and Sketch, therefore search over programs by sampling correct expansions from the DSL. This guarentees syntatically correct programs (though this DSL is very simple). The novel learning problem that is introduced in this paper is where a simple neural model (feedforward networks) maps input and output pairs to a set of binary attributes, where each attribute corresponds to a subroutine, like FILTER or SUM, that is likely to appear in the program. There are clever ways of incorporating these hints into the search procedure, and as such, the search procedure will be more likely to converge to a correct program.

In order to consume the inputs and outputs, they must be embedded in some continuous space in order for backpropagation to update the embeddings.

The contributions of the paper are therefore threefold:

1. Defining the custom DSL to search over
2. Embedding integers and integer arrays in a continuous space
3. Predict attributes that aid search and show magnitudes of speedup over standard program synthesis techniques.

### Details
##### Encoder
The only inputs allowed are integer and integer arrays, which are generic enough to hypothetically represent any sort of information if encoded in the right way. The encoding procedure is as follows:

1. Pad arrays up to length L=20 with NULL symbols.
2. Represent the type as a one-hot encoding (array or singleton), and embed each integer in the range [-256, 256] and NULL character in E=20 dimensional space.
3. Concatenate all representations of inputs, outputs, and input and output types.
4. Feed through simple H=3 layer fully connected network with K=256 neurons in each layer, with a sigmoid activation.

##### Decoder
The decoder then consumes the output of the neural network and applies a single multiply of size K x C, where C=34 is the number of attributes, as well as the sigmoid activation. Each logit can than be represented as the likelihood of the function at that index appearing somewhere in the program; we can call this final output C_hat.

##### Training
When training, the model is trained on tuples of (input, output, attributes), where the correct attributes are provided. Note that more than one attribute can be present for a particular input-output pair, making this problem akin to a multilabel classification problem. The model is trained with the cross entropy loss function in the typical maximum likelihood fashion. The encoder and decoder are trained jointly.

##### Search
The list of predicted attributes are then consumed by the search algorithms, where three baselines are considered; DFS, Sort-and-Add, and Sketch. When using DFS, and when expanding to a child node, the nodes considered are ordered by their likelihoods dictated by C_hat. When using Sort-and-Add, the top-k set of attributes are chosen and DFS is used to search with programs over these functions only. If a correct program has not been found, the next m attributes are added and the search repeats. Sketch is a system that can also consume hints in the same fashion as the Sort-and-Add procedure.

##### Results
The results are very positive -- there are up to 3 orders of magnitudes of speedup reported of finding a set of correct programs in the test set. They also show that the system demonstrates generalizability, where they test on programs of lengths 1 through 4 and test it on programs of length 1 through 5, which is shown to be a success. Therefore, the system generalizes to both unseen programs for a certain length, unseen input and output examples, as well as unseen programs for lengths that were never trained on (zero-shot generalization).

### Points of Interest
* The embedding and overall neural model is very simple; they find that more sophisticated neural architectures like the GRU unit are more difficult to train and only yield at best similar results.
* They use a very efficient implementation of DFS where each node caches the value of the program thus far, allowing fast testing of new programs without re-executing the entire program. As a result, they're able to process *3,000,000* programs per second.
* They use a curriculum in training the embedding where they grow the dimensionality by 1 whenever the loss dips below a limit. When projecting the final embeddings in 2D space, the results are quite nice, showing that the embeddings are in fact learning something meaningful.
* Sort-and-add is aided more drastically than DFS by C_hat. The authors hypothesize that this is because the objective with which C_hat is trained aligns better with Sort-and-add. Namely, DFS would be better aided if C_hat predicted functions that appeared earlier in the program synthesis phase, as early mistakes are more costly to DFS.

### Limitations
* The programs trained on are only of length 4, and at inference time, programs can be up to length 5.
* They restrict the integers from -256 to 256.
* They enforce that each array, at training time, is effectively length 20 (with padding).
* C_hat is only computed once per input-output pair, and is not recomputed as the search produces intermediate programs. This is unfortunate because the program synthesis is done in one-shot, and there is no sense of *partial correctness* in a program. Granted, this could be done with more supervision, but this is a tradeoff as well.
* Oddly enough, all methods experimented on, even the basic enumerative searches all solve 100% of the test set of programs at roughly the same time. The gains showcased in the paper are that the augmented searches solve 20% of the test set faster than the baseline searchers. This could be my misinterpretation of the graphs, but this strikes me as odd.

### Overall Thoughts
The direction of this paper is promising -- I do believe that models that actually synthesize programs are more interesting, practical, and relevant than models that essentially learn an implicit policy over what actions to take given certain inputs and outputs. I also am a big fan of combining techniques from the programming languages and the machine learning communities, which have operated largely disjointly until recently.

There are some glaring limitations, but this is not to discredit the work at all; indeed, any new line of work must start at some baseline, and this sets the stage for future improvements in synthesizing larger and more nontrivial programs. The incorporation of natural language into synthesizing programs is an important idea, and needs further exploration as well. The authors also use very simple encoder and decoder models, and note that while the RNNs they tested with were harder to train and did not yield reasonable results, more sophisticated neural architectures should not be discounted, which I completely agree with (in other words, this is my current research haha)

Lastly, I want to add something that I love thinking about when working in this field. An interesting idea that comes to mind is can a model *learn the DSL for programming in an unsupervised way*? While the question may seem trivial and irrelevant, upon more deliberation, this question essentially comes down to learning the semantics and syntax of a new language. For example, a wonderous dream of people in this area is for a model or agent to learn Python by taking a Python tutorial or class, where both the syntax and semantics of the langauge are taught. Moreover, the agent will not only add primitives and low-level constructs as it learns, but will also slowly build a repository of more abstract routines, like simple algorithms or methodologies! With hierarchical learning like this, we can achieve scalability in the types of programs the agent can synthesize, and we may be able to synthesize solutions to problems that are not immediately available (solving difficult programming problems, writing new software, etc.). The possibilities of such an agent are endless and ideas like this are what make the field of program induction and program synthesis so exciting. There is a lot of work to be done before this idea comes to fruition, but it will happen eventually.

