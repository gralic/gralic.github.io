---
title       : LetterFreq
subtitle    : Guess the language of text from Mean Squared Errors of Latin letter frequencies.
author      : Alex Rhatushnyak, for the Developing Data Products class on Coursera.org
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---

## How it works

1. You've got some text in an unknown language

2. You run the app by opening this page: https://arha.shinyapps.io/LetterFreq

3. You paste your text into the <b>Text input</b> field (the recommended volume is 1000...7000 symbols)

4. The most likely language(s), based on Latin letter frequences, appear in the second panel, at the center of your screen.

5. You can also increase or decrease the number of guessed languages, which is between 1 and 14, because the following 14 languages are currently supported:
English, French, German, Spanish, Portugese, Esperanto, Italian, Turkish, Swedish, Polish, Dutch, Danish, Icelandic, Finnish.  
The frequences of letters in languages were taken from this page:  
http://en.wikipedia.org/w/index.php?title=Letter_frequency&oldid=611734497

--- .class #id
<b>1. Example usage #1. Text input:</b>  
Accurate average letter frequencies can only be gleaned by analyzing a large amount of representative text. With the availability of modern computing and collections of large text corpora, such calculations are easily made. Examples can be drawn from a variety of sources (press reporting, religious texts, scientific texts and general fiction) and there are differences especially for general fiction with the position of 'h' and 'i', with H becoming more common.  
<b>Output:</b>  
 English ?  mse = 2.15  
 Swedish ?  mse = 2.47  
 French ?  mse = 2.91
<br><br>
<b>2. Example usage #2. Text input:</b>  
Accurate frequenze medie lettere possono essere raccolte solo analizzando una grande quantità di testo rappresentativo. Con la disponibilità di informatica moderna e collezioni di grandi corpora testo, questi calcoli sono fatti facilmente.  
<b>Output:</b>  
 Italian ?  mse = 1.66  
 French ?  mse = 2.81  
 English ?  mse = 2.91  

--- .class #id 
## How guesses are computed and what MSE is

1. Once the input text is given, frequences of letters are calculated; for example if your text is "<b>this is the end</b>", then freq("t") = freq("h") = freq("i") = freq("s") = freq("e") = 2/15, freq("n") = freq("d") = 1/15. No distinction between upper and lower case letters. Non-letters and non-Latin letters are ignored.

2. MSE is defined as follows: if $\hat{Y}$ is a vector of 26 reference letter frequencies, and $Y$ is the vector of the 26 frequencies derived from input text, then the MSE of the input text is:
$$\operatorname{MSE}=\frac{1}{26}\sum_{i=1}^{26}(\hat{Y_i} - Y_i)^2.$$

3. For each of the 14 supported languages, an MSE value is computed.

4. The lower MSE is, the closer is input text to the vector of language's reference letter frequencies. So the first guess is the language with lowest MSE, the second guess is the language with 2nd lowest MSE, and so on. The default value for the number of guesses is 3.

--- .class #id

For example, if frequencies in input text are

```r
freq = c(1/15, 1/15, 2/15, 2/15, 2/15, 2/15, 2/15, rep(0, 19))
```

while reference letter frequences in (a hypothetical language) lalang are all 1/30:

```r
langFreq = rep(1/30, 26)
```

then the MSE for this input text is:

```r
sum((langFreq - freq)^2)
```

```
## [1] 0.07333
```

That is,

```r
2 * (1/15 - 1/30)^2 + 5 * (2/15 - 1/30)^2 + 19 * (0 - 1/30)^2
```

```
## [1] 0.07333
```

