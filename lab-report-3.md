# Lab Report 3
Name: Arjun Sarup

PID: A17527244

Email: asarup@ucsd.edu

---

In this lab report, I'm going to explore some different options that can be used with the `grep` command that modify the basic output of returning lines matched to a given substring. Using `man grep`, you can already see that there are a ton of different options that literally vary from `-a` to `-z` (including some capitalized variations) that each have unique effects.

**Note: All of the following options were ones that I picked myself after finding them by using `man grep` and reading about their effects. I experimented with them until I found suitable options with accompanying examples. No external sources were used.**

## Using `-c`

One such option is `grep -c`, which returns the number of lines in a file that match the substring, rather than the lines themselves. In fact, we did a similar exercise in this lab itself, instead making use of the `>` operator to store the contents of `grep` in a new file, and subsequently using `wc` on that file to see how many lines were matched. This is useful if you only care about how frequent the matches are, and don't want to have to go to the trouble of manually counting out every matched line.

For example, say I wanted to get a sense of how many lines of `technical/biomed/rr74.txt` contain the letter "y", I could do the following:
```
% grep -c y rr74.txt
197
```
I just saved myself a lot of time spent counting up 197 lines of text, and I didn't have to create a new file like I did when I used `wc` in the lab. Another such example might look like this:
```
% grep -c whistleblower pmed.0020281.txt
4
```
Using the `-c` option, I now know that the file `technical/plos/pmed.0020281.txt` has 4 lines that contain the word "whistleblower", without having to look at what those lines actually are. Instead of returning those lines to me, the shell simply counted them up and returned that value instead.


## Using `-i`

Another option is `grep -i`, which returns the lines of a file that match the given substring while ignoring case. This is a pretty common functionality to want from a keyword-matching command; if the goal is to see all occurrences of a word, it is very often the case that we don't discriminate between uppercase and lowercase versions of the word. Another way to do this would be using the pattern-/regex-matching functionality of `grep`, but that can be a lot of work if all you want to do is search for a word in a file without caring whether or not it's capitalized. 

For example, say I wanted to find all occurrences of the word "clinical" in the file `techical/biomed/1468-6708-3-1.txt`:
```
% grep -i clinical 1468-6708-3-1.txt
        decreased mortality. Clinical trials powered to detect
        whom risk factors, subclinical disease, and morbidity are
          baseline. Clinical covariates include hypertension,
        significantly greater than zero. A clinical trial of a
          Implications for clinical trials
          YHL, but not YOL. Clinical trials of weight modification
          found for underweight older adults. Clinical trials whose
          outcome measure. Both YOL and YHL would be clinically
        YHL as the outcome measure in clinical trials involving
```
As you can see, lines 1, 3, 6, and 7 all found matches that use "Clinical" rather than "clinical". I wouldn't have found these lines if I had just used `grep` without the `-i` flag. Here's another example where I search for the word "SYMPTOM" in `technical/biomed/ar104.txt`:
```
% grep -i SYMPTOM ar104.txt 
          ameliorate disease symptoms. One potential problem with
        cartilage and bone erosion. Currently the symptoms of
        symptoms. In particular, inhibitors of TNF-α and IL-1 have
```
Despite the fact that I typed my keyword in all caps, `grep -i` matched any occurrence of the word "symptom"--capitalized or not.


## Using `-w`

Very often with `grep`, a source of frustration is wanting to match to a particular *word* rather than a *substring*. What I mean by this is that searching for the word "over" shouldn't include results that contain the word "overweight", since the intention was to match specifically for the word "over" and not other possibly-related words that contain it. Luckily, `grep` has the `-w` option that searches specifically for a word (defined as a non-word character, followed by the substring, followed by a non-word character) and returns the matching lines. 

Here's an example doing the aforementioned task in `techical/biomed/1468-6708-3-1.txt` that includes the output both without and with the `-w` tag to make the significance clear:
```
% grep over 1468-6708-3-1.txt 
        even though there is little evidence that overweight is
        associated with increased mortality in those over age 65.
        recommendations remain controversial, however, because the
          BMI of 18.5 to 24.9; overweight as 25 to 29.9; and
          examining health status over time, we added a sixth
          the possibility of 'over-adjustment', controlling
        thus substantial change in EVGFP over time, in both
        do not overlap, or overlap only slightly, are significantly
        classified as normal, overweight or obese all had about the
        women and men. Women who were normal or overweight averaged
        women than for men in the normal and overweight groups, but
        underweight category. The effect size comparing overweight
        overweight (for men or women) or obese (for men) affects
          Optimal weight and overweight
          overweight (as opposed to the obese) are no different
          interventions for older adults who were merely overweight
          follow-up. Biases caused by over-adjustment are probably
          differences between the overweight and normal weight
        'overweight' by the NHLBI guidelines. This suggests using
        that address older adults who are merely overweight.
% grep -w over 1468-6708-3-1.txt
        associated with increased mortality in those over age 65.
          examining health status over time, we added a sixth
          the possibility of 'over-adjustment', controlling
        thus substantial change in EVGFP over time, in both
          follow-up. Biases caused by over-adjustment are probably
```
Without `-w`, I got 20 lines that matched words like "*over*weight" and "contr*over*sial", which obviously wasn't what I was looking for. Using `-w` cut it down to 5 lines that specifically contained the word "over". Here's another example with `technical/government/Media/5_legal_groupts.txt`:
```
% grep project 5_Legal_Groups.txt
Legal Center at 205 N. 400 West is a project of "And Justice for
Community Legal Center is the first joint office project of public
become one of the major supporters of the project.
ways to go, with a bit more than half of the $4 million projected
% grep -w project 5_Legal_Groups.txt
Legal Center at 205 N. 400 West is a project of "And Justice for
Community Legal Center is the first joint office project of public
become one of the major supporters of the project.
```
Using `-w` here eliminated the extra line that matched the word "projected" to the keyword "project", thus returning 3 lines instead of 4. 


## Using `-n`

This particular `grep` option is fairly simple; it just prints the line numbers along with the matched lines. It's very useful if you want to know  where in a particular file the matched lines are, and can give you a sense of how sparsely the word is used or if there's a particular part of the text that uses the word often. 

This is an example of using `grep -n` on `technical/government/Media/predatory_loans/txt`:
```
% grep -n finance predatory_loans.txt
23:refinanced their Denver home, gave them some cash and escalated
93:The Ledfords expanded and refinanced their house over time, once
103:They refinanced in August 1998 with Advanta, a subprime lender
107:In October 1999 they refinanced again with New Century Mortgage,
111:In December 2000, they refinanced with Fieldstone, getting
163:refinance the Ledfords' home again.
```
Not only do I now know where to look for the word "finance" if I were to open `predatory_loans.txt`, but I can also tell that between lines 100-115, the article mentions a lot of details about refinancing a house, and mentions elsewhere are much sparser. Here's another example on `technical/government/Media/water_fees.txt`:
```
% grep -n farm water_fees.txt
18:Alpaugh Irrigation District, which supplies water to farmers in the
42:Tuesday when a screaming and crying farmworker arrived.
43:The farmworker thought the water was getting turned off today,
```
With the `-n` option, I have some context as to where I might read more about this incident with the screaming farmworker; the output tells me that this event is mentioned around lines 42/43 of the article.

---

All in all, these are only a few of many ways to manipulate `grep`. There are many more options that give even more agency over its output, and it'll take a lot of time to get used to using them all effectively.


