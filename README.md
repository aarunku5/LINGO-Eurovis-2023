# LINGO-Eurovis-2023

We investigate answer generation tasks, i.e., tasks which require word/sentence answers from the model to answer a given question.
---

## Embedding Space for Answer Generation

<img src="1.png" alt= "Embedding Space for Answer Generation" width="300" height="300">

We note that answer generation tasks are distributed sparsely around the sphere. There are also no tight local clusters based on the source datasets. This indicates higher linguistic diversity in the task definitions for this task type.

---

## Selecting Root Instruction

<img src="2.png" alt= "Root Instruction QASC" width="300" height="300">

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

---

## Correlation View

We show correlation views for link thresholds of 0.8, 0.7, and 0.6. The 10 tasks (T1-T10) can be found in the subfolder "Tasks". The following observations of the linguistic characteristics of the various task instructions can be used to provide context to how links are formed between clusters.


#### General Characteristics:
* None of the tasks have the same source dataset.
* All of the tasks fall within the answer generation category.
* None of the tasks use verbatim definitions.
* None of the tasks have common positive/negative examples.

#### Definitions:
            - T1-T7 all involve span-based selection to generate the answer. T8-T10 involve the generation of text-content based on the context of input, but which are not necessarily contained within the input.
            - T1, T2 exhibit high similarity in the definition, particularly in the case of 'Things to Avoid' as well as 'Emphasis and Caution'.
            - T3, T4 use similar language to T1, T2 and additionally specify that the shortest continuous text span should be selected to generate a relevant answer. Therefore they place further constraints on the expected output. 
            - T5's definition does not use similar language to T1-T4 in the definition, and in fact has the shortest length definition across all instructions.
            - T6, T7 also specify that span-based selection needs to be used for a larger volume of text input (i.e., a paragraph is used as the input instead of 1-2 sentences). T7 additionally provides contextual info on the types of reasoning to perform on the given input. 
            - T8-T10 involve the generation of text as output with reference to the input context. T8 requires event durations to be specified based on common sense reasoning with relation to the input. T9 explicitly asks for conversational, non-stereotypical answers to be provided as a response to the input. T10 is a story completion task, and requires the model to provide the ending for a 3-sentence story.

#### Positive Examples:


