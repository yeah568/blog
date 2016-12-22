---
layout: post
title: CPSC 121, Easy Mode
summary: A couple tips on making your life easier from a 121 TA.
tags:
- ubc
- university of british columbia
- computer science
- cs
- boolean logic
- theory
- models of computation
- vancouver
---
I quite like CPSC 121. I've been TA-ing it for two terms (aka. all the terms where I've been a TA), and I've done my fair share of tutorials and marking for both of them. There's some things I've picked up on as common mistakes during the quality time I've spent with my <span class="strike">failing</span> marking pen, and I want you (yes, you reading this article. Tim.) to be able to avoid these pitfalls. I'm not going to be able to give a summary of all the topics in the course (I'm much too lazy for that), but I'll try to cover the more common mistakes and questions that I get.

If you've been in any of my tutorials, you've likely heard most of these already. Also, you're probably no longer taking this course, so I really don't know why you'd read this, unless you really like my writing or something. Hey, I don't judge.

## Binary
There's a surprising amount of confusion about how and when to use two's complement and such. First, however, a quick rundown of signed versus unsigned integers. An n-bit signed number uses the first (furthest left) bit to signal whether the number is positive or negative, and can represent the values $$-2^{n-1}$$ to $$2^{n-1}-1$$. Unsigned integers on the other hand, don't have that bit, which means that they can represent numbers starting at 0, and going up to $$2^{n}-1$$.

When converting a number to decimal, the first step is to determine if the number is signed or unsigned (the question will tell you this).

#### Unsigned
If the number is unsigned, you _never_ take the two's complement. Simply add up the powers of 2, and that's your answer.

#### Signed
Signed numbers require another decision. If the sign bit (the aforementioned leftmost bit) is 0, that means the number is positive, and you don't need to take the two's complement, and can simply do the same thing you'd do with an unsigned integer. The only case where you do is if the sign bit is 1, indicating the number is negative. In this case, take the two's complement by "flipping the bits" and adding 1. Convert this number to decimal as per usual unsigned integer rules, then stick a negative sign in front of your final answer. Done!

## Predicate Logic
I often get asked about whether to use an AND ($$\wedge$$) or an IF ($$\rightarrow$$) when creating statements. As a general rule of thumb, you'll be much more likely to use an AND when dealing with existential statements, and universal statements tend to use IFs. When dealing with existentials, your intent is generally to say that there is some thing with the properties P, Q, and R, for example. You want to make sure that the things you're talking about have all of these properties, so you'll use the AND.

eg. $$\exists x \in D, P(x) \wedge Q(x) \wedge R(x)$$

On the other hand, with universals, you're much more likely to say "All P's have the property Q and R", where P is some subset of the domain D. In this case, you want to make sure you're _only_ talking about P's, and not everything else in D. By using IF, you ensure that you only go on to say x has the properties Q and R if it's a P.

eg. $$\forall x \in D, P(x) \rightarrow (Q(x) \wedge R(x))$$

If you used an AND instead of the IF in this case, you'd be saying that all of the elements in D have the properties P, Q, and R. While this is the intended meaning in some instances, you'll generally want to specify the subset of items you're talking about in the antecedent first.

## Proofs
In general, there's not much I can offer in terms of strategies for proving statements. It's definitely something that gets easier as you do it more, but that certainly doesn't help much when you're just starting out, as you probably are while taking this course. The best advice I have is to just try things out, and see what works. There are a couple patterns that can help make it a bit easier to pick a strategy to start on (for example, if you have a statement where the main logical connective is an IF, antecedent assumption is usually good, if you have a statement that starts with a universal quantifier, WLOG is good, and so on). Steve Wolfman wrote a very excellent [Guide to Proof Strategies](/assets/files/proof-strategies.pdf) which outlines when to use each strategy. (Most profs link to this on the course website, but I'm linking it here because links change and break and sometimes they don't link to it and blah this is just easier.)

With regards to WLOG (without loss of generality), just because you've used it does not mean you then get to specify what the value of that variable is. I've run into the sentence "WLOG, let x equal 3" more times than I'd like while marking. By specifying x, you are losing _all_ of your generality. Please do not do this. You will make me sad when I mark it, and then I'll cry a little. You don't want your TA to cry, do you?

## Circuits & DFAs

![A typical DFA circuit layout.](/assets/img/posts/cpsc-121-easy-mode/dfa-circuit.png)
*Simply hook up the input/constants to the appropriate inputs on the multiplexers, and you've got a fully functional DFA circuit. This circuit supports up to $$2^2=4$$ states, but the layout is similar for larger circuits.*


Once you get the part of the course where labs start moving away from actual, physical chips and wires and begin doing everything on the computer, do yourself a _huge_ favour and download [Logisim](http://www.cburch.com/logisim/) (it's completely free! Based GPL.), especially if you're translating from DFAs to circuits. The ability to rearrange stuff, and actually have states is a fantastic way of actually seeing your circuits work, instead of attempting to remember what state your D flip-flops are in as you run through the rest of the circuit.

Protip: If you press `Ctrl+T`, you can tick the circuit, essentially causing any clocks in your circuit to change state. Pressing `Ctrl+R` resets your circuit back to its initial state.

When drawing out a circuit from DFAs, it's important to remember that all DFA circuits are nearly identical. You plop down enough D flip-flops to represent each state ($$\lceil \log_{2}(S) \rceil$$, where S is the number of states). Then, depending on what your professor likes, stick either one multiplexer down, or one multiplexer for each D flip-flop[^1]. At this point, you've already got most of the circuit done! Hook up the output of the multiplexer back up to the input of the D flip-flop, have the output of the D flip-flops to the select input of the multiplexer(s), and hook up a clock to those flip-flops, and you're nearly done. The final bit is simply to decide what the next state should be based on the multiplexer input, and that's really the only bit of a DFA to circuit translation that requires some thinking. It's useful if you have a truth table for the DFA, consisting of the state numbers, the possible inputs, and the resulting state number for each number, as it makes finding the appropriate next value to feed into the multiplexer a bit simpler.

[^1]: I know Patrice Belleville prefers having the single one, whereas Steve Wolfman likes having one for each bit. I personally prefer having more than one, as I find it a bit more clear and understandable, but it really is personal preference. It's like the Vim vs. Emacs debate for DFA circuit draw-ers.

## Induction
Lots of people are confused about induction in this class - heck, I was completely baffled until after the course had ended. The main thing to remember (especially on exams, where induction proofs are often quite long and tedious) is that there are three main steps - the base case(s), the induction hypothesis, and the induction step. The base case is basically the base case when you're dealing with recursion, if you've learned that in CPSC 110 - it's the smallest[^2] valid value that you stop/start at. The induction hypothesis is generally the thing you're trying to prove - it's essentially free marks for just restating the question. Learn how your professor likes the induction hypothesis written, and stick it down on your exam paper. Finally, there's the induction step - this is where things get a little tricky. This is the place where you assume your hypothesis holds for x, and then try to prove it for x + 1[^3]. How exactly you do that depends on what the question is, so I can't help much there. More mathematical solutions are common in the induction step, though.

[^2]: For the most part, your base case is the smallest value in the range. If you were, say, proving something for all positive integers, you'll probably want to start with a base case of 1, and then prove for x + 1. On the other hand, if you were to prove something for all negative integers, you'd be more likely to start with -1, and prove for x - 1. You (probably) won't have to deal with this kind of proof though, so I wouldn't worry about it.
[^3]: Or assume for x - 1 and prove for x; again, personal preference. There are occasionally cases where one makes the resulting math a bit easier, but both are equally valid.

Induction often seems magical and incomplete when you first learn about it. If you think about how a crystal grows, you can kind of imagine the base case as the seed crystal - it's the initial proven thing that you then build up off of. The induction step then adds on to the range of proven values/crystal, layer by layer. Once you've proven your hypothesis for, say, x = 1, you can use the induction step to prove it for x = 2, and then use it again for x = 3, and so on and so on until infinity. It's genuinely a really powerful tool for proofs, especially in later courses (for example, things like proving sorting algorithms work relies on this kind of proof). If there's one thing you take away from this course, let it be this.

## Bonus - Easy Arts Credits
If you're anything like me and you're looking for Arts credits to fulfil your requirement, and you did pretty well in CPSC 121, then I highly recommend taking PHIL 220. There's a Distance Ed version of it, so you can even do all the work in the comfort of your own bed (like me). It's (surprisingly) not on the credit exclusion list, and the materials covered are very similar. Some of the terminology is different, and there's a couple new ideas, but in general, if you found the predicate logic and proof-y bits of 121 relatively okay, then you'll have no problem with PHIL 220.

## Conclusion
Hopefully, this covers most of the common questions that people have about 121, and if not, feel free to leave a comment! I'll try to update this if anyone asks any more questions. There's a lot of material that this course covers, so there's bound to be a few things that I've missed.