# CCIC 2021 Call For Participation

## Introduction

The Computer Composition Identification Challenge, initiated by the Conference on Sound and Music Technology (CSMT), is succeeding the CSMT Data Challenge, 2020. This new challenge is dedicated to finding improved music generation models and objective evaluation models for a number of specified music styles. 

There will be **two** roles competing against each other in this challenge: **Generator** versus **Judge**. Participants must choose one role and complete the corresponding tasks. 

**Generators** are supposed to develop a system[^1] that can automatically generate a playable (solo) violin phrase with the <u>beginning and ending bars</u>, <u>tempo</u>, <u>key signature</u>, <u>time signature</u>, and <u>the required style (by composer)</u> as the input specified by the challenge. 

**Judges** are required to analyze given <u>music phrases</u>, under the given <u>style context (by composer's name)</u>, and produce a *Human Composition Score* ($HCS$​​​​​​​) -- the probability of the input being composed by a human composer.

[^1]: A system can consist of more than one objective model or rule sets.

## Prizing

1. **The best generator** -- for the participant whose **generator** system obtains the highest score. 
2. **The best judge** -- for the participant whose **judge** system obtains the highest score. 

4. **Creativity award** -- for the participant with the most creative solution, determined by the committee.

## Schedule

| Time               | Event                                              | Description                                                  |
| ------------------ | -------------------------------------------------- | ------------------------------------------------------------ |
| End of August 2021 | Beginning of challenge                             | A list of violin composers will be released, where each violin composer has a specific music style. |
| 27th Sept          | Preliminary submission deadline for **generators** | Participants who have chosen to build a **generator** should submit their output. Valid submission indicates registration to the challenge. |
| 8th Oct            | Preliminary submission deadline for **judges**     | Participants who have chosen the **judge** role should submit their output. Valid submission indicates registration to the challenge. |
| 1st Nov            | Release of the beginnings and endings              | Beginnings and endings are released in pairs as a part of requirements of final submission. |
| 3rd Nov            | Final submission deadline for **generators**       | **Generator** systems should complete these phrases in 48 hours and make the final submission. |
| 4th Nov            | Release of the generated phrases                   | Generated phrases will be compiled, shuffled and made available to **judge** systems. |
| 6th Nov            | Final submission deadline for **judges**           | **Judges** systems should calculate the $HCS$s in 48 hours and make the final submission. |
| Mid-Nov            | Result announcement                                | Results and prizes will be announced during the conference   |

## Procedure

When the challenge begins, [a list of violin composers](../Violin Composer List/Violin Composer List of CCIC 2021.html) will be released, where each violin composer has a specific music style. 

### Task Requirements

For **generator** systems, 200 music phrases should be generated in the style of each composer in the list. The beginning and ending bars will not be specified until the final submission. This means the music phrases generated before the final submission can have arbitrary beginning and ends and should be able to generate with specified beginning and ending bars in the final stage.

For **judge** systems, participants are required to compute the probability of a given music phrase as the input being generated by computer systems, with the given composer’s name as the context. Throughout the development stage, only the composers’ own compositions (positive samples) are allowed to be used to train the **judge** system[^2].

[^2]: Notice that the determination of music generation system should not be considered as a classification problem between human composed phrases and computer-generated phrases.

The submission will be via **[Microsoft CMT](https://cmt3.research.microsoft.com/)**. The entry is not opened until 20th, Sep 2021.

### Preliminary Submission

The preliminary result contains two submission deadlines. The first deadline is for **generators** to submit 200 music phrases for each composer. The second deadline is set around one week later to **judges** for submitting the results ($HCS$​​​​), which are the feedback for the music phrases generated by **generators**. Notice that any valid submission record in this two preliminary submissions will be considered as completing the registration for this challenge but the submitted content will not affect the final score.

### Final Submission

The last few days before the final submission deadline will be the evaluation stage, when 200 pairs of beginnings and endings will be released for each composer's pieces in the list. 

In 48 hours, **generator** systems should complete these music phrases and submit them as their final submission. Then, these phrases will be compiled together and made available to **judge** systems. From this moment on, in the next 48 hours, **judge** systems are expected to calculate the $HCS$​​​​​​​​s for these phrases and make the final submission. For each composer, a certain number of extra phrases extracted from human-composed pieces will be mixed into the input. [^3]

[^3]: The numbers may vary and the details will be specified when the challenge starts.

The final submission requires participants submit their program output (phrases for **generators** and $HCS$s for **judges**), source code[^4] and a technical report.

[^4]:  If necessary, the zip file can have a password. The organization committee may ask a password disclosure for reimplementation of the submitted system under certain circumstances.

## Scoring

**Generator** scores are calculated by the average $HCS$​​​​ from all **judges**.

**Judge** scores are calculated by the sum of the following two scores:

1. $HCS$ for human composed phrases 
	
2. $1-HCS$​ for phrases generated by **generators**.


## Q&A

For any enquires, email csmt.ccic.2021@gmail.com
