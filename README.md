# Follow the white rabbit

This is my solution for Follow the White Rabbit challenge.
The challenge asked to find the correct phrase, in order to satisfy the following
constraints:

- An anagram of the phrase is: "poultry outwits ants"
- The MD5 hash of the easiest secret phrase is "e4820b45d2277f3844eac66c903e84be"
- The MD5 hash of the more difficult secret phrase is "23170acc097c24edb98fc5488ab033fe"
- The MD5 hash of the hard secret phrase is "665e5bcb0c20062fe8abaaf4628bb154"

## Performance

The final algorithm was tested on Google Colab, which mounts 2 CPUs Intel(R) Xeon(R)
CPU 2.30 GHz and 13 GB as RAM.
The difficult hash is found in 2 s, the easy one 5 s, the hard one  195 s

## Reproducibility

To reproduce results a notebook version that can be run Google Colab can be found
here:
## Usage

Before running this script makes sure the `wordlist` file is in the same script's
directory.
Then, to run this script in a bash shell type:
```
$python solution.py

Filtering out all the unnecessary words...
Found 1788 anagrams...
Creating words combinations...
ty outlaws printouts match hash: 23170acc097c24edb98fc5488ab033fe
1.6810383796691895 s
printout stout yawls match hash: e4820b45d2277f3844eac66c903e84be
5.265990734100342 s
Updating sentence length to 4
wu lisp not statutory match hash: 665e5bcb0c20062fe8abaaf4628bb154
194.59327912330627 s
```

## Strategy

I tried to follow a simple and efficient approach, in order to have a readable
and not heavy code. These are the steps:
1. Filter all the `wordlist` in order to find all words whose `collections.Counter`
is a subset of the main ANAGRAM ("poultry outwits ants"). This ensures that the
remaining words can be combined together and they can form anagrams of ANAGRAM
2. Cycle through each remaining word and create possible candidates for each of them.
Candidates are chosen, so that the total length of the candidate-word pair is at
least equal to the minimal characters length needed. Furthermore, the `collections.Counter`
must be a subset of ANAGRAM, to ensure anagrams can be created
3. Create all the permutations with candidates and respective words and make sure
`collections.Counter` is satisfied.
4. Verify permutations hash with given hashes

One of the most promising improvements is to kick in `multiprocessing` and use
one thread to create combinations and another to verify.
