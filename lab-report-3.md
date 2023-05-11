# Lab Report 1
Name: Arjun Sarup

PID: A17527244

Email: asarup@ucsd.edu

---

In this lab report, I'm going to explore some different options that can be used with the `grep` command that modify the basic output of returning lines matched to a given substring. Using `man grep`, you can already see that there are a ton of different options that literally vary from `-a` to `-z` (including some capitalized variations) that each have unique effects.

## Using `-c`

One such option is `grep -c`, which returns the number of lines in a file that match the substring, rather than the lines themselves. In fact, we did a similar exercise in this lab itself, instead making use of the `>` operator to store the contents of `grep` in a new file, and subsequently using `wc` on that file to see how many lines were matched. This is useful if you only care about how frequent the matches are, and don't want to have to go to the trouble of manually counting out every matched line.

For example, say I wanted to get a sense of how many lines of `./technical/biomed/rr74.txt` contain the letter "y", I could do the following:
```
% grep -c y rr74.txt
197
```
I just saved myself a lot of time spent counting up 197 lines of text, and I didn't have to create a new file like I did when I used `wc` in the lab. Another such example might look like this:
```
% grep -c whistleblower pmed.0020281.txt
4
```
Using the `-c` option, I now know that the file `./technical/plos/pmed.0020281.txt` has 4 lines that contain the word "whistleblower", without having to look at what those lines actually are. Instead of returning those lines to me, the shell simply counted them up and returned that value instead.


## Using `-r`

