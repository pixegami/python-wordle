# python-wordle
An implementation of Wordle in Python than can be played via the terminal.

## Bug Fix Update

It was pointed out to me that the game didn't behave like Wordle does when two identical letters
are in a 'guess', but only one copy of that letter is in the word.

### Problem

For example, if the guess is HELLO and the secret is APPLE, then the previous behavior causes
the first occurrence of the L to be yellow (because it does appear in the word), but then the second
L will be green (because it is in the right position).

On a letter-by-letter basis, this behavior appears fine. But when the entire word is considered, that
first L shouldn't appear yellow. Instead, it should appear grey. That's because there is already another
L in the guess that 'claims' the occurrence of the L in the secret.

### Solution

* Instead of resolving all the letter results in one-pass, we now first initialize it as an array and
default all guess letters to grey (not in the word).
* We create an list, which is a copy of the secret. Each element corresponds to the letter in the string. 
For example `["A", "P", "P", "L", "E"]`.
* We then do a first pass through the guess, and identify any letters in the correct position. If we find one,
then that letter becomes 'green', and the secret list is updated to void out that letter so it can't be used
in a subsequent comparison. So if we guess "HELLO", the remaining secret becomes something like this:
`["A", "P", "P", "*", "E"]`.
* We then do another loop this time identify the guessed letters which are in the word (by using a nested loop),
but not in position.