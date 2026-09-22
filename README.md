# Trace

Type a regex and see it drawn as a railroad diagram, then matched against your text by its own engine. It never calls `RegExp`.

https://bxzex.github.io/trace/

The parser is recursive descent and reports syntax errors at the right position. The matcher is a backtracking engine written in continuation passing style. It handles classes, groups (including named ones), lazy quantifiers, backreferences, anchors and `\b`. Lookaround isn't supported, and the page tells you so.

`(a*)*` doesn't hang, because a loop iteration that consumes nothing is refused. There's also a step budget, so a catastrophic pattern gives you an error instead of freezing the tab.

I checked it against the browser's own RegExp on twenty patterns, and the match lists came out identical.
