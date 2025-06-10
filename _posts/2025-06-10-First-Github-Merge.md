---
layout: post
title: First GitHub Merge!
subtitle: Cpython Merge
cover-img: /assets/img/python1.jpg
thumbnail-img: /assets/img/pythonlogo.png
share-img: /assets/img/python1.jpg
tags: [Cpython]
author: Yash Vijay
---
After 7 days of going back and forth, 26 commits, and 44 comments, My first pull 
request finally merged on Github... that too on the official interpreter of 
[python](https://github.com/python/cpython)! 

## How did I end up here?

Well summer vacation started and I didn't have the time this year to apply for GSOC, 
so over the summer I promised myself that I would spend the time to learn about the 
interpreter of python. Feeling ambitious and excited I just straight up went and ordered
Anthony Shaw's Cpython Internals book to follow through over the summer. Reading that ancient
codebase made in C and some parts in python was well.. kinda impossible to navigate on my own.

## 'First Issue' Dilemma

I opened up the devguide, joined the mailing list, **EVEN CHANGED MY OPERATING SYSTEM 
FROM WINDOWS TO FEDORA** because well.. wanted to feel like a developer (I know was not necessary
but hey it's my broke student version of a tech bro buying a 5000 usd setup to code things that could
be done on a thinkpad). Now came the hard part, which is actually contributing to the codebase. I 
looked through the devguide and noticed that they asked to work with github issues labelled 
[good first issues](https://github.com/python/cpython/issues?q=is%3Aissue%20state%3Aopen%20label%3Aeasy).

Now a bigger problem comes up: First Issues get taken up in literal minutes after posting... I realized that
after seeing someone or the other is more or less assigned to a Project anyways. Dreams of changing anything over
the summer vacation sort of shattered, I thought that now I just learn about Cpython over the entire summer and 
hopefully come across something much later to land a contribution.

## Noticing my debut 

I was reading like the first real chapter of Cpython Internals book trying to keep up with it, reading documentation
alongside the codebase as it introduced me to the Python grammar files, while reading the documentation I came across a
section in the documentation saying:

~~~
for_stmt ::= "for" target_list "in" starred_list ":" suite
             ["else" ":" suite]
~~~

And then I started wondering the starred_list even means. Asked AI and it told me it meant expressions_list and **was an error**
Lets just say i felt quite excited seeing it an error as that meant I have an issue to now go and officially fix. Opened
up an issue for it and then started waiting for some reviewer to look at it and tell me what it all is. Funny part is
some person replied asking me they would like to work on it, and I had to go all "Um bro I dont even know if this is supposed
to be an issue, and if it is one, I want to work on it" in a more polite tone, ofcourse. [Skirpichev](https://github.com/skirpichev) 
responded to me with grammar files stating:

~~~
 star_expressions[expr_ty]: 
     | a=star_expression b=(',' c=star_expression { c })+ [','] { 
         _PyAST_Tuple(CHECK(asdl_expr_seq*, _PyPegen_seq_insert_in_front(p, a, b)), Load, EXTRA) } 
     | a=star_expression ',' { _PyAST_Tuple(CHECK(asdl_expr_seq*, _PyPegen_singleton_seq(p, a)), Load, EXTRA) } 
     | star_expression 
  
 star_expression[expr_ty] (memo): 
     | '*' a=bitwise_or { _PyAST_Starred(a, Load, EXTRA) } 
     | expression
~~~

I thought he meant that starred_list is some term defined here. And I just replied with a polite "Thank You" and closed the issue.
Well the he reopened the issue saying I had misunderstood and the documentation was infact incorrect. Felt overjoyed again and started
to wonder "hmm... if starred_list is incorrect... what do I put here"

## Learning The Meanings

Spent the next like two days just going over what term fit there.. AI told me it was "expressions_list" but that seemed incorrect as it was
the previous term used was infact "expressions_list" which got changed to "starred_list". I looked over the [expressions glossary](https://docs.python.org/3/reference/expressions.html#). Realized there was a term "starred_expressions_list". I quickly changed it to that and clicked submit.
Meanwhile i went through what phrase was actually trying to state too.

## Realizing how PRS work for Cpython

Man did I underestimate the making a "pull request" that too on cpython. Despite it being a documentation fix, I was grilled a little on why 
"starred_expression_list" should be a good fix, me constantly changing small things like typos unrelated to the PR (they take their one fix
at a time policy very seriously even if you have small typo changes on documentation). Each line has a maximum character limit. Basically
it took literally 5 days of going back and forth, literally 44 comments and 26 commits to finally get it merged. But boy how glad I felt
afterwards is something only I know.. I was one of the 3,000 people who have fixed an inconsistency between python.gram and its documentation. 
All from catching the issues to fixing it on my own. [The Merged PR is here](https://github.com/python/cpython/pull/134034).

Hopefully this is just a beginning to a killer profile work on The official Interpreter of python.
