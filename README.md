# LINGO-Eurovis-2023

We investigate answer generation tasks, i.e., tasks which require word/sentence answers from the model to answer a given question.

---

## Embedding Space for Answer Generation

<img src="Images/1.png" alt= "Embedding Space for Answer Generation" width="300" height="300">

We note that answer generation tasks are distributed sparsely around the sphere. There are also no tight local clusters based on the source datasets. This indicates higher linguistic diversity in the task definitions for this task type.

**Insight: Further investigation of answer generation tasks belonging in a given quadrant of the embedding sphere.**

---

## Selecting Root Instruction

<img src="Images/2.png" alt= "Root Instruction QASC" width="300" height="300">

We select the following task for the root instruction:

**Source:** [QASC](https://arxiv.org/pdf/1910.11473.pdf)

**Categories:** Question Answering -> Contextual Question Answering -> Extractive; Text Span Selection; Reasoning -> Commonsense Reasoning

**Definition:** _Write a correct answer to the given question based on its associated fact. Make sure that your answer is contained in the associated fact.Things to avoid: Don't be creative and introduce any new word that is not mentioned in the associated fact! Remember that, the associated fact has been rearranged to form the question. So, the correct answer words must lie within the associated fact.\nEmphasis & Caution: The correct answer can be a word, phrase, or even a sentence._

**Positive Examples:**

1.
            "input": "Fact: pesticides can harm animals. \nQuestion: What can harm animals?",
            "output": "pesticides.",
            "explanation": "The answer is present in the span of the fact."
2.
            "input": "Fact: rain can help form soil. \nQuestion: Rain can help form?",
            "output": "soil.",
            "explanation": "The answer is present in the span of the fact."
3.
            "input": "Fact: rain helps plants to survive. \nQuestion: rain helps plants to?",
            "output": "survive.",
            "explanation": "The answer is present in the span of the fact."
4.
            "input": "Fact: a radio converts electrical energy into sound. \nQuestion: a radio converts electrical energy into?",
            "output": "sound.",
            "explanation": "The answer is present in the span of the fact."
5.
            "input": "Fact: chopping down trees causes animals to move to another habitat. \nQuestion: what might cause animals to move to another habitat?",
            "output": "chopping down trees.",
            "explanation": "The answer is present in the span of the fact."
6.
            "input": "Fact: a protractor  can be used to measure the angles of a prism. \nQuestion: What would you use to measure the angles of a prism?",
            "output": "a protractor.",
            "explanation": "The answer is present in the span of the fact."
7.
            "input": "Fact: lightning can cause harm to animals. \nQuestion: What might cause harm to animals?",
            "output": "lightning.",
            "explanation": "The answer is present in the span of the fact."

**Negative Examples:**

1.
            "input": "Fact: pesticides can harm animals. \nQuestion: What can harm animals?",
            "output": "Plastic.",
            "explanation": "Even though the answer \"plastic\" is factually correct as plastic can harm animals, since it is not present in the given fact is is not a good answer. Note that, the correct answer words must lie within the associated fact."
2.
            "input": "Fact: rain can help form soil. \nQuestion: Rain can help form?",
            "output": "soil and trees.",
            "explanation": "The words \"and trees\" are not present in the associated fact. So, it's a bad answer."
3.
            "input": "Fact: rain helps plants to survive. \nQuestion: rain helps plants to?",
            "output": "survived.",
            "explanation": "Here, the answer does not fit with the question grammatically. The correct answer would have been \"survive\". Remember to copy your answer directly from the given fact, as questions have been formed after rearranging their associated facts."

**Insight: Selecting QASC as the root instruction, compare the distribution of tasks across domains, source dataset, and task type that fall wihtin k=10.**

---

## Correlation

<img src="Images/3.png" alt= "Correlations" width="1093" height="270">

We show correlation views for link thresholds of 0.8, 0.7, and 0.6. The 10 tasks (T1-T10) can be found in the folder "Tasks". The following observations of the linguistic characteristics of various task instructions can be used to provide context to how links are formed between clusters:

#### General Characteristics:
* None of the tasks have the same source dataset.
* All of the tasks fall within the answer generation category.
* None of the tasks use verbatim definitions.
* None of the tasks have common positive/negative examples.

#### Task Nature
* T1-T7 all involve span-based selection to generate the answer. T8-T10 involve the generation of text-content based on the context of input, but which are not necessarily contained within the input.
* T1, T2 exhibit high similarity in the definition, particularly in the case of 'Things to Avoid' as well as 'Emphasis and Caution'.
* T3, T4 use similar language to T1, T2 and additionally specify that the shortest continuous text span should be selected to generate a relevant answer. Therefore they place further constraints on the expected output. 
* T5's definition does not use similar language to T1-T4 in the definition, and in fact has the shortest length definition across all instructions.
* T6, T7 also specify that span-based selection needs to be used for a larger volume of text input (i.e., a paragraph is used as the input instead of 1-2 sentences). T7 additionally provides contextual info on the types of reasoning to perform on the given input. 
* T8-T10 involve the generation of text as output with reference to the input context; however, the text generated does not involve span-based selection, and is instead free response. T8 requires event durations to be specified based on common sense reasoning with relation to the input. T9 explicitly asks for conversational, non-stereotypical answers to be provided as a response to the input. T10 is a story completion task, and requires the model to provide the ending for a 3-sentence story.

**Insight: Tasks T1-T5 are the same task type. T6, T7 parse longer input lengths but are also the same task type. T8-T10 involve answer generation, but produce freeform output instead of span-based selection. Additionally, the similarity of T8, T9, T10 to T1 are 0.72, 0.63, 0.61 respectively. We  must further investigate word overlap and length of the task examples to gauge the impact of task type on level of diversity.**

---

## Instruction Decomposition

### Positive Examples, Word Overlap 

https://user-images.githubusercontent.com/54283572/226142736-1ca42c13-0be0-45f9-be24-989ccc153628.mov

### Negative Examples, Word Overlap 

https://user-images.githubusercontent.com/54283572/226142739-6cfa6aee-95d9-4b80-9d6b-da4feaece7e6.mov

* Comparing word overlap for positive and negative examples respectively (threshold 0.5 since there is greater linguistic variation between tasks, in order to observe chord formation), we find that T1 is linked to T2, T5, T6, T7. T2, T4, T8 are also linked. T9, T10 are not linked to any of the other tasks. 
* On observing the text associated with these tasks using the correlation view, we can conclude that the chord associations follow from the characteristics of the task examples we observe, listed below:
  * In general, the number of positive examples exceeds the number of negative examples for all the tasks; non of the tasks have more than 3 negative examples. Some tasks have a large number of positive examples (>2) -- T1, T4, T6, T7, T10.
  * Overall, the explanations for negative examples show higher linguistic diversity than for positive examples; they also show higher variation in length than the positive examples.
  * T1, T6, T7 have overlap between positive and negative examples; also, the explanations given are short-- they only talk about whether the output is from the input span or not and are formulaic in structure.
  * T3 has no overlap, and has richer explanation text which is unique to each example.
  * T2, T4 have overlap within positive examples and T5 has no overlap; additionally, the explanation text for all these tasks also follows a uniform pattern and mentions that the span is present within the question as the justification for correctness or incorrectness of model output.
  * T8 has overlap between positive and negative examples; the overlapping examples follow similar explanation patterns.
  * T9 has overlap between the positive and negative examples; however the explanations exhibit higher patterns of variation.
  * T10 has overlap within postive examples; however the explanations vary in language and structure.

**Insight: Word overlap is not very useful when comparing T1 to tasks, other than T2 and T5. The diversity of the examples between tasks is such that only those tasks with highly structured, templatized explanations (particularly for positive examples) show significant mutual relationships; we also have to set the threshold quite low to generate chords for tasks other than T1.**

### Positive Examples, Length 

https://user-images.githubusercontent.com/54283572/226142829-aa38de65-32c8-46e1-82cb-8b311a96c132.mov

### Negative Examples, Length

https://user-images.githubusercontent.com/54283572/226142836-72003a4c-f28d-4527-9ebc-3157426bc566.mov

* Comparing length for positive and negative examples respectively (threshold 0.7 since there is comparatively lesser variation in length between tasks):
  *  T1 is linked to T2, T5, T8. These have 1-2 sentence input, produce 1 word output, and have similar length explanations for positive examples. Though length of negative explanations is usually longer than positive ones, they do not vary much across these tasks.
  *  T6, T7 are linked only to each other. These tasks have long, paragraph length inputs, which dictates their low similarity to the other tasks, though the explanation length is comparable to some of the lesser input-volume tasks.
  * T3, T4 are mutually linked. They have slightly longer explanations compared to T1 and T5, and T3 and T4 additionally produce multi-word output. 
  *  T9, T10 are mutually linked. They both produce sentence length output, with an input of 2-3 sentences. Explanations lengths are comparable respectively for postive and negative examples between these tasks. 

**Insight: Length-based comparison seems to depend highly on the input/output volume as opposed to the explanation lengths comparing T1 against T6, T7, T9, T10. Explanations impact this relationship in the case of T3, T4, which outstrip T1 in explanation length; we set the threshold high in order to elicit this result.**


---

## Model Results

<img src="Images/5-r.png" alt= "Model Results" width="1200" height="675">

* Task instances in bins with higher similarity to the examples are more accurately solved. T1, T2, T6, T7 have similar performance distributions in this regard, with a higher proportion of samples in 0.65+ similarity bins.
* T5, T8 have more diversity in instances in comparison to the positive/negative examples (i.e., most instances range from 0.4-0.6 similarity) 
* All the tasks have some empty bins ranging in either 0.75+ or 0.25- similarity.
* Tasks with greater diversity in the explanation and output text, i.e., T5, T8-T10 have a more uniformly distributed performance.
* Tasks with structured, templatized explanations (T1-T4, T6-T7), as well as those with overlap between positive and negative examples (T1, T6, T7) all show higher average performance, with a higher number of instances in high similarity bins.


**Insight: The lowest performing tasks have no overlap between positive and negative examples, more diverse explanations, and a more uniform distribution in terms of similarity bins. This indicates that models find these tasks relatively more difficult to solve, i.e., these tasks may potentially have lower bias in comparison to remaining tasks.**
---

## Bias Metrics

Owen checks the unique vocabulary contributed by examples in each task instruction (5.1). He finds that T6 and T7 contribute the highest unique vocabulary, while T1–T4 contribute the lowest. T6 and T7 are also the hardest for the model to solve (as previously seen in (4)). Next, Owen examines the Jaccard Similarity of ad- verbs across full task instructions compared against their respective task instances (5.2). Here, T5–T7 show the lowest values, which indicates higher variation in the data. Owen can partially attribute this to non-repetition of phrases from the definition in the example explanations.

---

## Impact of Instruction Modification

The variants of T1 can be found in the subfolder "Variants" (V1-V8). The variants can be grouped into 3 modification stages:
* V1-V2: Definition modified
* V3-V5: Example explanations modified 
* V6-V8: Example text modified (i.e., new examples either replace or are added to the instruction)

We discuss motivation of the change made with reference to T2-T10 performance, as well as the implications of variants in terms of:
(i) effect on model performance
(ii) effect on bias-metrics
(iii) correlation with other tasks

Note: 
* Changes made for each stage of modification are carried over to the next stage. 
* We summarize the change in respective statistics across all variants in table form for the purpose of brevity. 
* Updated panels are shown only for V8 (the final variant). 

### V1

(6) Finally, Owen modifies the task instruction for T1, in order to (i) reduce the similarity bias between instruction examples and task instances, and (ii) reduce overlap between the instruction definition and instruction example explanations. He updates the definition and replaces a positive example, so that the instruction now contributes a higher proportion of vocabulary and adverbs (6.1). Owen notices that the beeswarm updates (6.2) for T1 to show decreased overall performance, with fewer samples in similarity bins > 0.9 as well as fewer samples crossing 80% accuracy. Additionally, T1 now ex- hibits lower Jaccard similarity (6.3) and higher unique vocabulary (6.4). Overall, Owen’s modifications achieve a decrease in accu- racy, Jaccard similarity for words, POS-tags, and n-grams, and an increase in unique vocabulary. Hence by iteratively changing the task instruction (for instance, replacing more examples) Owen can further reduce instruction bias for T1, and create a more difficult task instruction for the model to solve.

### V2
### V3
### V4
### V5
### V6
### V7
### V8



