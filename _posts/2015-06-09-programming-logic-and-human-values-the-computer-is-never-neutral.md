---
ID: 4452
post_title: 'Programming Logic and Human Values: The Computer is Never Neutral'
author: James DiGioia
post_date: 2015-06-09 16:57:05
post_excerpt: ""
layout: post
permalink: http://jamesdigioia.com/programming-logic-and-human-values-the-computer-is-never-neutral/
published: true
---
Since I started working in web development and programming, my dad has been fond of repeating this somewhat common phrase about the field:

> Programming is all logic.

He's not wrong, in a sense: Given a particular environment, a computer program will respond to a set of inputs the same way no matter how many times you run it.[^1] An `if` statement will always switch the same way if you pass it `true`, and we rely on this consistency when writing our programs.

However, when I hear this statement and place it in the context in which we discuss computers and technology today, I fear is obscures more than it illuminates. Simplifying programming down as "logic" gives it an other-worldli-ness that treats this logic as if it's infallible; that the results of the machine are distinct from human concerns.

I work daily in a web language called JavaScript. JavaScript is known as a dynamic scripting language, meaning two things:

1.  It's "dynamically typed", meaning variables can be whatever you want them to be and all you have to do is assign them to a variable and they become that. By comparison, a "statically typed" language means to need to declare the variable's type (see footnote for types[^2]) and the variable can only be that type. If you try to assign a different type to it, the program won't even run.
2.  It's a scripting language, meaning the program is compiled as it is run. All programs need to be compiled from the code we read and write down into machine code computers understand before they can be run. Some languages, like C and Java, are compiled as a separate step before they can be run. JavaScript (and PHP, the language WordPress is written in) is compiled as it's executed.

These two properties result in some interesting properties of the language, but take a look at this odd quirk the first property above results in:

    0 == "0"; // true
    false == 0; // true
    false == "0"; // false
    

**Where's your logic now?!**

All kidding aside, what's actually happening behind the scenes makes perfect sense: Because JavaScript is dynamically typed, when you do these loose comparisons, the program has to convert between types in order to do this comparison[^3]. In the first case, the string `"0"` is converted to an integer `0`, so that results in `true`. In the second case, integer `0` is converted to a boolean as `false` (any other positive number would resolve to `true`), so that's results in `true`. In the third case, however, the string is *not* converted to a number first; it's converted to a boolean, and any string with characters in it converted to a boolean is `true` (an empty string `""` would convert to false), which results in `false == true`, which is obviously `false`.

Why does any of this matter? This is just one tiny way, embedded into one language, that the internal logic of a programming language is made up of *human value judgements.* The designers of JavaScript, way back when, could have decided to resolve this typing problem by converting strings to numbers first, then to booleans, in order to ensure this kind of mathematical transitive property, but they didn't.[^4] They could have made JavaScript a statically-typed language[^5], but they didn't.

This is admittedly a small value judgement, compared to the much more significant judgements that go into which content to show you on a daily basis on Facebook, or which search results you're likely to find the most useful for a particular search term, but this seems to me like a very obvious and easy-to-grasp value judgement that actually has a pretty significant impact on how the language actually gets written and used on the web.

These type of small value judgements are embedded in *everything*. We often treat technology, the Internet, and the spaces those things create as separate from politics and values. It's how we end up with inane ideas like the ["Technium,"][1] as if technology ["wants"][2] anything on its own. Even the platitude "Information wants to be free" becomes a marvelous flattening of complex social dynamics, structures, and values.

MOOC Mania in education suffers from the same delusion that an expansion of technology will result in [the flattening of hierarchies][3] and the future will be ["borderless, gender-blind, race-blind, class-blind and bank account-blind."][4]. You saw this same logic on display recently with the Facebook ["It's Not Our Fault" Study][5], which treated the Facebook algorithm as if it's something completely distinct from human values.

If you can see it woven into the very fabric of the languages themselves that we use to build these tools, that you must for certain see it everywhere else. But at some point, we've ceded our sense of technology as an ideology itself and instead see it as an erasure of ideology.

That's a lie. Don't be fooled.

[^1]:    
    Granted, you can introduce randomness into your programs, but that's almost like another input, so we'll ignore that corner case.

[^2]:    
    For those of you who don't know, most, if not all, programming languages have a concept of primitives, like String (`"string"`), Number (`0`), and Boolean (`true`), known as "types." More complex languages have more types, like `HashMap`, but we're just going to deal with these types for now.

[^3]:    
    JavaScript and most other dynamically typed languages also have a strict comparison operator (`===`) that will not convert the types, which makes all of these `false`.

[^4]:    
    FWIW, I don't think that solution would have made sense, but it certainly was an option, regardless of what you think of its relative merit.

[^5]:    
    Historically speaking, there was no way making JavaScript a static language would have been a good idea. JavaScript was famously [designed and built in 10 days][6], and a number of the design decisions embedded into the language are a result of this limited development time. Another example of our computers being very much the result of human limits.

 [1]: https://edge.org/conversation/the-technium
 [2]: http://www.newrepublic.com/article/books/magazine/84525/morozov-kelly-technology-book-wired
 [3]: http://hackeducation.com/2015/03/26/technofantasies/
 [4]: http://www.theguardian.com/education/2013/jun/15/university-education-online-mooc
 [5]: http://www.wired.com/2015/05/facebook-not-fault-study/
 [6]: http://www.computer.org/csdl/mags/co/2012/02/mco2012020007.html