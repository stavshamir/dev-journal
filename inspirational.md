## [When Worse Is Better: Incrementally Escaping Local Maxima][when worse is better]

Kent Beck writes about how sometimes incremental improvement becomes too hard, suggesting the strategy of incremental degradation may help.

He presents an example in which a poorly designed class contains many method which are not broken down logically. Instead of trying to incrementally improve each method, he suggests in-lining all the methods into one big pile of code (thus degrading them) and then break this one big method into new methods that now make sense.

[when worse is better]: https://www.facebook.com/notes/kent-beck/when-worse-is-better-incrementally-escaping-local-maxima/498576730175196/

---
