---
title: "Folding Python code in Emacs"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2018-03-19
description: "An Emacs Lisp function to automatically fold all top-level Python blocks using vimish-folding."
---

## Motivation

I use Python daily and Emacs as my editor. The `vimish-folding` package lets me define and persist custom folds, but unlike Vim, it doesn’t fold functions and classes automatically.

Here’s a small function that fills that gap: it folds every top-level code block in a Python buffer.

## The function

```emacs-lisp
(defun fold-python-blocks ()
  "Fold all top-level code blocks in a Python buffer."
  (interactive)
  (forward-word)
  (setq p (point))
  (while (forward-word)
    (backward-word)
    (setq col (current-column))
    (forward-word)
    (if (= col 0)
        (progn
          (setq p1 (car (bounds-of-thing-at-point \='word)))
          (vimish-fold p p1)
          (setq p p1)
          (goto-char p)
          (forward-word))))
  (vimish-fold p (buffer-size))
  (goto-char 1))
```

Add this to your `init.el`, then invoke it with `M-x fold-python-blocks`. It will fold each top-level block, including code after `if __name__ == "__main__":`.
