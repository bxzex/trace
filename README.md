# Trace

A regular expression parser that draws your pattern as a railroad diagram and
matches text with its own backtracking engine. Nothing here calls `RegExp`.

Live: https://bxzex.github.io/trace/

## The three pieces

**A recursive descent parser** over the pattern, producing an AST. The grammar
runs loosest to tightest: alternation, then sequence, then repetition, then
atom. Groups recurse back to the top. Syntax errors carry the position they
happened at, and the page reports them instead of throwing.

Supported: literals and escapes, `.`, character classes with ranges and
negation, `\d \w \s` and their negations, groups (capturing, non capturing and
named), alternation, `* + ?`, `{m}`, `{m,}`, `{m,n}`, lazy quantifiers,
backreferences, anchors and `\b`. Lookaround is rejected with a clear message
rather than silently mis-parsed.

**A backtracking matcher** in continuation passing style. Each node receives the
rest of the work as a function, so alternation is an ordered loop over branches
and laziness is the same two calls in the other order. Repetition refuses to
count an iteration that consumed nothing, which is what stops `(a*)*` from
hanging. A step budget aborts pathological patterns with an explanation instead
of freezing the tab.

**An SVG renderer** that measures every node bottom up, then lays the tree out
left to right. Alternation fans into parallel tracks, repetition draws a loop
back under its body, and a lazy loop is drawn dashed.

## Verification

The engine is differential tested against the browser's own `RegExp`: twenty
patterns covering alternation, backreferences, lazy quantifiers, classes,
anchors and flags all produce identical match lists.

## Notes

One HTML file. No libraries. No build step.

Built by [bxzex](https://bxzex.com).
