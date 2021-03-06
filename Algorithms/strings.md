# Strings

## [Pangrams](https://www.hackerrank.com/challenges/pangrams)

```
from collections import defaultdict

counter = defaultdict(int)

s = input().strip().replace(' ','').lower()
#print(s)

for i in s:
    counter[i] += 1

pangram = True
for i in 'abcdefghijklmnopqrstuvwxyz':
    if counter[i] == 0:
        pangram = False

if pangram:
    print('pangram')
else:
    print('not pangram')
```

## [Alternating Characters](https://www.hackerrank.com/challenges/alternating-characters)

All test cases passed in < 0.4 seconds. Save time by not actually removing from string/list
```
T = int(input().strip())

for _ in range(T):
    s = list(input().strip())
    length_s = len(s)
    i = 1
    deletions = 0
    last_letter = s[i-1]
    while i < length_s:
        if s[i] == last_letter:
            i+=1
            deletions += 1
        else:
            last_letter = s[i]
            i+=1

    print(deletions)
```

## [The Love-Letter Mystery](https://www.hackerrank.com/challenges/the-love-letter-mystery)

The idea behind this is to take the absolute difference between symmetric opposites in the string and sum each of them. This is because the absolute difference is the amount a letter has to be lowered to reach the other.
```
import string
from collections import defaultdict

abc_dict = defaultdict(int)
abc = string.ascii_lowercase
for i,a in enumerate(abc):
    abc_dict[a] = i


T = int(input().strip())

for _ in range(T):
    s = list(input().strip())

    count = 0
    for i in range(len(s)//2):
        count += abs(abc_dict[s[i]]-abc_dict[s[-i-1]])
    print(count)
```

## [Gemstones](https://www.hackerrank.com/challenges/gem-stones)

```
from functools import reduce


T = int(input().strip())

rocks = []
for _ in range(T):
    rocks.append(set(input().strip()))

print(len(reduce(set.intersection,rocks)))
```

## [Funny String](https://www.hackerrank.com/challenges/funny-string)

```
def is_funny(s):
    for i in range(1,len(s)):
        if abs(ord(s[i])-ord(s[i-1])) != abs(ord(s[len(s)-i])-ord(s[len(s)-i-1])):
            return 'Not Funny'
    return 'Funny'

N = int(input().strip())
for _ in range(N):
    s = input().strip()
    print(is_funny(s))

```

## [Anagram](https://www.hackerrank.com/challenges/anagram)

```
from collections import Counter

def anagram_finder(S):
    if len(S)%2:
        return -1
    S1 = S[:len(S)//2]
    S2 = S[len(S)//2:]
    S1_count = Counter(S1)
    S2_count = Counter(S2)
    if S1_count == S2_count:
        return 0
    else:
        total = 0
        for k,v in S2_count.items():
            if v > S1_count[k]:
                total+= v-S1_count[k]
        return total

N = int(input().strip())
for _ in range(N):
    S = input().strip()
    print(anagram_finder(S))
```

## [Palindrome Index](https://www.hackerrank.com/challenges/palindrome-index)

```
def is_palindrome(S):
    '''
    >>> is_palindrome('aaab')
    3
    >>> is_palindrome('baa')
    0
    >>> is_palindrome('aaa')
    -1
    >>> is_palindrome('quyjjdcgsvvsgcdjjyq')
    1
    >>> is_palindrome('hgygsvlfwcwnswtuhmyaljkqlqjjqlqkjlaymhutwsnwcflvsgygh')
    8
    >>> is_palindrome('hgygsvlfcwnswtuhmyaljkqlqjjqlqkjlaymhutwsnwcwflvsgygh')
    44
    '''
    skipped = False
    r_offset = 0
    removed = -1
    for i in range(len(S)//2):
        #print(S[i], S[len(S)-1-i-r_offset])
        if S[i] == S[len(S)-1-i-r_offset]:
            continue
        elif not skipped:
            skipped = True
            #You need to check if the letter pair after the first adjusted pair continue the palindrome
            if S[i+1] == S[len(S)-1-i] and S[i+2] == S[len(S)-2-i]:
                removed = i
                r_offset -= 1
                continue
            elif S[i] == S[len(S)-1-i-1] and S[i+1] == S[len(S)-2-i-1]:
                removed = len(S)-1-i
                r_offset +=1
                continue
            else:
                return -1
        else:
            #print(len(S), i, r_offset)
            return -1
    return removed

if __name__ == '__main__':
    import doctest
    doctest.testmod()

    T = int(input().strip())
    for _ in range(T):
        S = input().strip()
        print(is_palindrome(S))
```

## [Two Strings](https://www.hackerrank.com/challenges/two-strings)

```
def is_substring(S1,S2):
    '''
    >>> is_substring('hello','world')
    'YES'
    >>> is_substring('hi','world')
    'NO'
    '''

    S1 = set(list(S1))
    S2 = set(list(S2))
    if S1.intersection(S2):
        return 'YES'
    else:
        return 'NO'


if __name__ == '__main__':
    import doctest
    doctest.testmod()

    T = int(input().strip())
    for _ in range(T):
        S1 = input().strip()
        S2 = input().strip()
        print(is_substring(S1,S2))
```

## [Game of Thrones - I](https://www.hackerrank.com/challenges/game-of-thrones)

```
from collections import Counter

def ana_pal(S):
    '''
    >>> ana_pal('aaabbbb')
    'YES'
    >>> ana_pal('cdefghmnopqrstuvw')
    'NO'
    >>> ana_pal('cdcdcdcdeeeef')
    'YES'
    '''
    c = Counter(S)
    count_odds = 0
    #we can only ever have at most one odd count of letters
    #any more prevents a palindrome
    for _,v in c.items():
        if v%2:
            count_odds +=1
            if count_odds == 2:
                return 'NO'
    return 'YES'



if __name__ == '__main__':
    import doctest
    doctest.testmod()

    S = input().strip()
    print(ana_pal(S))
```

## [Make it Anagram](https://www.hackerrank.com/challenges/make-it-anagram)

```
from collections import Counter

def count_dels(S1, S2):
    '''
    >>> count_dels('cde','abc')
    4
    '''
    C1,C2 = Counter(S1),Counter(S2)
    uniques = set(C1.keys()).union(set(C2.keys()))
    total_del = 0
    for x in uniques:
        total_del+= abs(C1[x]-C2[x])
    return total_del


if __name__ == '__main__':
    import doctest
    doctest.testmod()

    S1 = input().strip()
    S2 = input().strip()
    print(count_dels(S1,S2))
```

## [Bear and Steady Gene](https://www.hackerrank.com/challenges/bear-and-steady-gene)


Couldn't get the O(N) solution on my own, got this solution from [here](http://pastebin.com/AVPGcURN)
```
from collections import defaultdict


def is_valid_string(good_len, memo):
    '''
    >>> is_valid_string(2, {'A':2})
    True
    >>> is_valid_string(2, {'A':3})
    False
    '''
    for k,v in memo.items():
        if v>good_len:
            return False
    return True


def find_sub_len(full_length,string):
    '''
    >>> find_sub_len(8, 'GAAATAAA')
    5
    >>> find_sub_len(8, 'GTGCCGCA')
    2
    >>> find_sub_len(8, 'AATTGGCC')
    0
    '''
    good_len = full_length//4
    ext_memo = defaultdict(int)

    t=-1
    for i in range(full_length-1,-1,-1):
        ext_memo[string[i]]+=1
        if not is_valid_string(good_len,ext_memo):
            t = i+1
            ext_memo[string[i]]-=1
            break

    if t==-1:
        return 0 #counts are equal, no invalid string found

    current_solution = 99999999

    for h in range(-1,full_length-1):
        if t >= full_length:
            break
        if h>=t:
            break
        while not is_valid_string(good_len, ext_memo) and t < full_length:
            ext_memo[string[t]]-=1
            t+=1
        if t > full_length or not is_valid_string(good_len,ext_memo):
            break
        new_solution = max(0, t - h - 1)
        if current_solution > new_solution:
            current_solution = new_solution

        ext_memo[string[h+1]]+=1

    return current_solution


if __name__ == '__main__':
    import doctest
    doctest.testmod()

    full_length = int(input().strip())
    string = input().strip()
    print(find_sub_len(full_length,string))
```
