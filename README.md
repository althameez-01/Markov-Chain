📘 Overview

This project demonstrates how statistical language models (Markov chains) can generate creative, human-like text by learning probabilistic word transitions from a corpus.
It combines syntactic awareness (POS tagging) from spaCy with statistical modeling from Markovify to produce more coherent sentences than a simple random generator.

🧩 Key Components
Library	Purpose
NLTK	Access to the Gutenberg corpus (classic English texts).
spaCy	Tokenization, sentence parsing, and part-of-speech tagging.
Markovify	Creation of a Markov chain model to generate new text sequences.
Regex (re)	Text cleaning and normalization.
⚙️ Installation

Run the following commands to install dependencies:

!pip install nltk
!pip install spacy
!pip install markovify
!python -m spacy download en_core_web_sm

🧠 Dataset

We use Shakespeare’s tragedies from NLTK’s Gutenberg corpus:

Hamlet

Macbeth

Julius Caesar

You can explore available texts:

from nltk.corpus import gutenberg
print(gutenberg.fileids())

🧹 Text Cleaning

Before modeling, we perform text preprocessing:

Remove line breaks, special characters, and numbers.

Normalize whitespace.

Strip metadata like chapter headings.

Example cleaning function:

def text_cleaner(text):
    text = re.sub(r'--', ' ', text)
    text = re.sub(r'\n', '', text)
    text = re.sub(r'(\b|\s+\-?|^\-?)(\d+|\d*\.\d+)\b', '', text)
    text = ' '.join(text.split())
    return text

🧩 Model 1 — Basic Markov Chain Generator

A simple Markov model learns sentence transitions from the Shakespeare corpus:

generator_1 = markovify.Text(shakespeare_sents, state_size=3)
for i in range(3):
    print(generator_1.make_sentence())

💬 Model 2 — POS-Aware Markov Chain (spaCy Integration)

We extend markovify.Text to include part-of-speech tags, improving grammatical consistency.

class POSifiedText(markovify.Text):
    def word_split(self, sentence):
        return ['::'.join((word.orth_, word.pos_)) for word in nlp(sentence)]
    def word_join(self, words):
        sentence = ' '.join(word.split('::')[0] for word in words)
        return sentence


Instantiate and generate:

generator_2 = POSifiedText(shakespeare_sents, state_size=3)
for i in range(5):
    print(generator_2.make_sentence())

🧠 Example Output
Be innocent of the knowledge, dearest Chuck, Till thou applaud the deed.
Enter Antony, Octavius, and the grim Alarme excite the mortified man.
Thy selfe do grace to them, we rest your Ermites King.

📈 How It Works

Tokenization: spaCy splits text into tokens (words, punctuation, etc.).

Markov Chain Construction: Markovify models word transitions.

Generation: Random but statistically likely sequences are generated.

POS Conditioning: The extended version maintains grammatical flow.

🧠 Applications

Creative text generation

Language modeling experiments

AI-based story/poetry generation

Educational demos in NLP and AI

🧑‍💻 Future Enhancements

Add a GUI or web interface using Streamlit or Gradio.

Combine multiple authors for stylistic blending.

Use deep learning models (GPT, LSTM) for comparison.

Generate dialogue with character-level conditioning.


