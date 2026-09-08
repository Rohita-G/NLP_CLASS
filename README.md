# CS5760 – Natural Language Processing  
## Homework 1  
### University of Central Missouri  
### Department of Computer Science & Cybersecurity  
### Fall 2026  

---

## Student Information

- **Student Name:** Rohita Gangishetty
- **Student ID:** 700750412

---

## Repository Overview

This repository contains my code and solution(used md block) for **Homework 1** in CS5760 (NLP).  
The main deliverable is a Google Colab notebook (`.ipynb`) that implements all required tasks:

- Regular expressions for various text patterns (Q1)  
- Manual and coded Byte Pair Encoding (BPE) on a toy corpus and on a short paragraph (Q2)  
- Application of Bayes’ rule to text classification (Q3)  
- Add-1 (Laplace) smoothing calculations for a sentiment example (Q4)  
- Tokenization in my language, comparison with an NLP tool, multiword expressions, and reflection (Q5)  

The notebook file is:  
- `NLP_HW_1.ipynb`  

All code is commented to explain the logic and steps.

---

## How to Run

1. Open the notebook in Google Colab:  
   - Either upload `NLP_HW_1.ipynb` to your Google Drive and open it with Colab, or  
   - Use “Open in Colab” if you have linked this GitHub repo to Colab.  

2. Make sure the runtime is set to **Python 3**.  

3. Run the cells in order from top to bottom using **Runtime → Run all** or by pressing `Shift+Enter` in each cell.  

4. All outputs (regex matches, BPE merges, token lists, etc.) will appear directly below each code cell.

No additional data files are required; all text used in the assignment is defined inside the notebook.

---

## Assignment Solutions

### Q1 – Regex

This section implements regular expressions in Python using the `re` module to match:

1. **U.S. ZIP codes**  
   - Patterns: `12345`, `12345-6789`, `12345 6789`  
   - Uses word boundaries to avoid matching inside longer strings.  

2. **Words that do not start with a capital letter**  
   - Allows internal apostrophes and hyphens (e.g., `don't`, `state-of-the-art`).  
   - Uses negation in the character class for the first letter.  

3. **Rich number format**  
   - Optional sign (`+`/`-`)  
   - Optional thousands separators (commas)  
   - Optional decimal part  
   - Optional scientific notation (e.g., `1.23e-4`)  

4. **Spelling variants of “email”**  
   - Matches `email`, `e-mail`, `e mail`  
   - Allows space, hyphen, or en-dash between `e` and `mail`  
   - Case-insensitive matching.  

5. **Interjection “go” with repeated “o” and optional punctuation**  
   - Matches `go`, `goo`, `gooo`, …  
   - Allows optional trailing `!`, `.`, `,`, or `?` as a word.  

6. **Lines ending with a question mark and optional closing quotes/brackets**  
   - Uses anchors to match end-of-line.  
   - Allows characters like `"`, `'`, `)`, `]`, `’` after the question mark.  

Each sub-question includes:
- The regex pattern  
- A short test string  
- `re.findall()` output demonstrating the matches  

---

### Q2 – Byte Pair Encoding (BPE)

#### Q2.1 – Manual BPE on Toy Corpus

Toy corpus:  
`low low low low low lowest lowest newer newer newer newer newer newer wider wider wider new new`

Steps:
- Added end-of-word marker `_` to each word.  
- Listed the initial vocabulary (characters + `_`).  
- Computed bigram counts over the tokenized words.  
- Performed the first three merges by hand:
  - Identified the most frequent pair  
  - Merged it into a new token  
  - Showed the updated corpus snippet and updated vocabulary after each step  

All intermediate counts and merges are documented in the notebook.

#### Q2.2 – Mini-BPE Learner (Code)

Implemented a small BPE learner in Python:

- Functions:
  - `get_pair_counts(words)` – counts adjacent token pairs  
  - `merge_pair(words, pair)` – merges the most frequent pair across all words  

- Process:
  - Initialized vocabulary from the toy corpus with `_`  
  - Ran BPE for several merge steps  
  - Printed:
    - The top pair at each step  
    - The new token created  
    - The evolving vocabulary size  

- Segmentation:
  - Showed subword sequences for:  
    - `new`  
    - `newer`  
    - `lowest`  
    - `widest`  
    - An invented word (e.g., `newestest`)  

- Reflection:
  - Explained how subword tokens help with the **out-of-vocabulary (OOV)** problem  
  - Gave an example where a subword aligns with a meaningful morpheme (e.g., `er_` as a suffix in English)  

#### Q2.3 – BPE on My Language / English

- Chose a short paragraph (4–6 sentences) in English.  
- Preprocessed the text:
  - Lowercased  
  - Split into words  
  - Added end-of-word marker `_`  

- Trained BPE:
  - Learned at least **30 merges**  
  - Tracked:
    - Initial vocabulary size  
    - Five most frequent merges (pair → new token, count)  
    - Five longest resulting subword tokens  

- Segmented **5 different words** from the paragraph:
  - Included:
    - One rare word  
    - One derived/inflected form (e.g., `learning`, `projects`)  

- Reflection (5–8 sentences):
  - Discussed what kinds of subwords were learned (prefixes, suffixes, stems, whole words)  
  - Listed two concrete **pros** of subword tokenization for this language  
  - Listed two concrete **cons** (e.g., unclear meaning of some subwords, longer sequences)  

---

### Q3 – Bayes’ Rule for Text Classification

Explained the terms from the slide:

- \( P(c) \): Prior probability of class \( c \) before seeing the document.  
- \( P(d \mid c) \): Likelihood – probability of observing document \( d \) given class \( c \).  
- \( P(c \mid d) \): Posterior probability – probability that document \( d \) belongs to class \( c \) after observing \( d \).  

Explained why the denominator \( P(d) \) can be ignored when comparing classes:
- \( P(d) \) is the same for all classes for a fixed document  
- Therefore, it does not affect which class has the highest posterior  
- Classification can be based on comparing \( P(d \mid c) P(c) \) only  

All explanations are written in my own words in the notebook.

---

### Q4 – Add-1 (Laplace) Smoothing

Based on the worked sentiment example from the slides:

- Given:
  - \( P(-) = 3/5 \), \( P(+) = 2/5 \)  
  - Vocabulary size \( V = 20 \)  
  - Total negative tokens = 14  

Computed:

1. **Denominator for likelihood with add-1 smoothing (negative class):**  
   \[
   \text{denom} = \text{total tokens} + V = 14 + 20 = 34
   \]

2. **\( P(\text{predictable} \mid -) \)** when “predictable” occurs 2 times in negative documents:  
   \[
   P(\text{predictable} \mid -) = \frac{2 + 1}{14 + 20} = \frac{3}{34} \approx 0.0882
   \]

3. **\( P(\text{fun} \mid -) \)** when “fun” never appeared in negative documents:  
   \[
   P(\text{fun} \mid -) = \frac{0 + 1}{14 + 20} = \frac{1}{34} \approx 0.0294
   \]

All steps and formulas are shown in the notebook.

---

### Q5 – Tokenization in My Language

#### 5.1 – Naïve vs. Manual Tokenization

- Took a short paragraph (3–4 sentences) in my language (Telugu).  
- Performed **naïve space-based tokenization** using `text.split()`.  
- Manually corrected tokens by:
  - Separating punctuation (e.g., `.`) from words  
  - Splitting some suffixes/clitics where appropriate (e.g., `కాలేజీకి` → `కాలేజీ + కి`)  

- Submitted both versions and highlighted differences in the notebook.

#### 5.2 – Comparison with an NLP Tool

- Used an NLP library that supports Telugu (e.g., `indic-nlp-library`).  
- Ran the same paragraph through the tool’s tokenizer.  
- Compared:
  - Tool tokens vs. naïve tokens  
  - Tool tokens vs. manually corrected tokens  

- Discussed:
  - Which tokens differ (e.g., treatment of compound words, suffixes, punctuation)  
  - Why they differ (word-level vs. morphological segmentation choices)  

#### 5.3 – Multiword Expressions (MWEs)

- Identified at least **3 multiword expressions** in Telugu, such as:
  - Place names  
  - Idiomatic phrases  
  - Common fixed expressions  

- Explained why each should be treated as a single token:
  - Their meaning is not fully compositional  
  - They frequently occur together as a unit  

#### 5.4 – Reflection

In 5–6 sentences, discussed:

- The hardest part of tokenization in Telugu (e.g., handling suffixes, clitics, and agglutinative structure).  
- How this compares to English tokenization (mostly space-separated, simpler morphology).  
- Whether punctuation, morphology, and MWEs make tokenization more difficult in my language.  

---

## Files in This Repository

- `NLP_HW_1.ipynb` – Main Colab notebook with all solutions and outputs  
- `README.md` – This file, explaining the work and providing student info  

---

## Notes for Instructor
 
- The notebook was developed and tested in Google Colab.  
 
