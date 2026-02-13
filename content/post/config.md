---
title: "Emacs config file"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2019-09-26
description: "My full Emacs configuration in org-mode style: Evil, Python, Org, Magit, Helm, and more."
toc: true
---

I recently migrated my configuration to an `org` file, and the result is much tidier—a proper literate programming approach to editor configuration. The file is continuously updated; see the latest version [here](https://github.com/rodogi/emacs/blob/master/config.org).

## Personal info

```emacs-lisp
(setq user-full-name "Rodrigo Dorantes Gilardi"
      user-mail-address "rodgdor@gmail.com"
      calendar-latitude 19.4326
      calendar-longitude -99.13
      calendar-location-name "Mexico City")
```

## use-package

This configuration draws from Harry Schwartz’s [config file](https://github.com/hrs/dotfiles/blob/master/emacs/.emacs.d/configuration.org). Start by ensuring all packages are installed automatically:

```emacs-lisp
(require \='use-package-ensure)
(setq use-package-always-ensure t)
```

Always compile packages and prefer the newest version:

```emacs-lisp
(use-package auto-compile
  :config (auto-compile-on-load-mode))
(setq load-prefer-newer t)
```

## Sensible defaults

```emacs-lisp
(global-display-line-numbers-mode)
(fset \='yes-or-no-p \='y-or-n-p)
(setq ns-pop-up-frames nil)
(setq-default tab-width 2)
(setq ring-bell-function \='ignore)
```

Line wrapping: 100 columns for text, 80 for code:

```emacs-lisp
(dolist (hook \='(text-mode-hook latex-mode-hook tex-mode-hook))
  (add-hook hook (lambda () (set-fill-column 100))))
(dolist (hook \='(python-mode-hook prog-mode-hook list-mode-hook))
  (add-hook hook (lambda () (set-fill-column 80))))
```

```emacs-lisp
(add-hook \='text-mode-hook \='auto-fill-mode)
(add-hook \='org-mode-hook \='auto-fill-mode)
```

Better pop-up windows with [popwin](https://github.com/m2ym/popwin-el), search occurrence counting with anzu:

```emacs-lisp
(use-package popwin
  :config
  (popwin-mode 1)
  (push \='(help-mode :position right :width 0.45) popwin:special-display-config))

(use-package anzu
  :config
  (global-anzu-mode +1)
  (global-set-key [remap query-replace] \='anzu-query-replace)
  (global-set-key [remap query-replace-regexp] \='anzu-query-replace-regexp))

(setq column-number-mode 1)
(blink-cursor-mode -1)
(setq-default backup-inhibited t)
```

## Evil

```emacs-lisp
(use-package evil
  :config (evil-mode 1))

(use-package evil-surround
  :config (global-evil-surround-mode 1))

(use-package evil-escape
  :init (evil-escape-mode)
  :config (setq-default evil-escape-key-sequence "jk"))

(use-package evil-visualstar
  :config
  (global-evil-visualstar-mode)
  (setq evil-visualstar/persistent nil))
```

## UI

```emacs-lisp
(tool-bar-mode 0)
(menu-bar-mode 0)
(scroll-bar-mode 0)
(set-window-scroll-bars (minibuffer-window) nil nil)

(setq frame-title-format
      \='((:eval (if (buffer-file-name)
                   (abbreviate-file-name (buffer-file-name))
                 "%b"))))
```

Zenburn theme with moody mode line:

```emacs-lisp
(use-package zenburn-theme
  :config
  (load-theme \='zenburn t)
  (let ((line (face-attribute \='mode-line :underline)))
    (set-face-attribute \='mode-line nil :overline line)
    (set-face-attribute \='mode-line-inactive nil :overline line)
    (set-face-attribute \='mode-line-inactive nil :underline line)
    (set-face-attribute \='mode-line nil :box nil)
    (set-face-attribute \='mode-line-inactive nil :box nil)
    (set-face-attribute \='mode-line-inactive nil :background "#f9f2d9")))
(set-frame-font "IBM Plex Mono-14" nil t)

(use-package moody
  :config
  (setq x-underline-at-descent-line t)
  (moody-replace-mode-line-buffer-identification)
  (moody-replace-vc-mode))

(use-package minions
  :config
  (setq minions-mode-line-lighter ""
        minions-mode-line-delimiters \='("" . ""))
  (minions-mode 1))

(global-hl-line-mode)

(use-package diff-hl
  :config (add-hook \='prog-mode-hook \='turn-on-diff-hl-mode))

(setq evil-insert-state-cursor \='((bar . 2) "yellow")
      evil-normal-state-cursor \='(box "yellow"))
```

## Spelling

```emacs-lisp
(dolist (hook \='(org-mode-hook latex-mode-hook tex-mode-hook git-commit-mode-hook))
  (add-hook hook (lambda () (flyspell-mode 1))))
(setq ispell-program-name "/usr/local/bin/aspell")
(setq ispell-dictionary "english")
```

## Python

```emacs-lisp
(add-hook \='python-mode-hook #\='(lambda () (modify-syntax-entry ?_ "w")))

(use-package elpy
  :init (elpy-enable)
  :bind ("M-." . elpy-goto-definition)
  :config
  (setq exec-path (append exec-path \='("/usr/local/bin")))
  (setenv "PATH" (concat (getenv "PATH") ":/usr/local/bin"))
  (setq elpy-rpc-python-command "/usr/local/bin/python3"))

(use-package python
  :mode ("\\.py\\\'" . python-mode)
  :interpreter ("python" . python-mode)
  :config
  (setq python-shell-interpreter "/usr/local/bin/jupyter"
        python-shell-interpreter-args "console --simple-prompt"
        python-shell-prompt-detect-failure-warning nil)
  (add-to-list \='python-shell-completion-native-disabled-interpreters "jupyter"))

(use-package jedi
  :config
  (add-hook \='python-mode-hook \='jedi:setup)
  (setq jedi:complete-on-dot t))

(use-package flycheck
  :config (add-hook \='elpy-mode-hook \='flycheck-mode))

(use-package py-autopep8
  :config (add-hook \='elpy-mode-hook \='py-autopep8-enable-on-save))
```

## Org

```emacs-lisp
(use-package org)
(when (version<= "9.2" (org-version))
  (require \='org-tempo))

(use-package org-bullets
  :init (add-hook \='org-mode-hook \='org-bullets-mode))

(setq org-ellipsis "⤵")
(setq org-src-tab-acts-natively t)

(global-set-key "\C-ca" \='org-agenda)
(setq org-agenda-files \='("~/Dropbox/org/"))

(setq org-todo-keywords
      \='((sequence "TODO(t)" "|" "DONE(d!)" "CANCELED(c@/!)")))
(setq org-archive-location "~/org/archive.org::datetree/")
(setq org-log-done \='time)

(global-set-key "\C-cc" \='org-capture)
(setq org-capture-templates
  \='(("b" "Blog idea"
       entry
       (file "~/Dropbox/notes/blog_ideas.org")
       "* %?\n")))
```

### Export

```emacs-lisp
(require \='ox-beamer)
(use-package ox-hugo :after ox)
(use-package ox-twbs)

(org-babel-do-load-languages
 \='org-babel-load-languages \='((C . t) (python . t) (emacs-lisp . t) (shell . t)))

(setq org-latex-pdf-process
      \='("pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"
        "bibtex %b"
        "pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"
        "pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"))

(add-to-list \='org-latex-packages-alist \='("" "minted"))
(setq org-latex-listing \='minted)
(use-package org-ref)
```

## Magit

```emacs-lisp
(use-package magit
  :config (global-set-key (kbd "C-x g") \='magit-status))
```

## Helm

```emacs-lisp
(use-package helm
  :config
  (helm-mode 1)
  (global-set-key (kbd "C-x C-f") \='helm-find-files))

(global-set-key (kbd "C-x C-a") \='helm-apropos)
(global-set-key (kbd "M-x") \='helm-M-x)
(setq helm-M-x-fuzzy-match t)
(global-set-key (kbd "M-y") \='helm-show-kill-ring)
(global-set-key (kbd "C-x b") \='helm-mini)
(global-set-key (kbd "C-x C-b") \='helm-mini)
(setq helm-buffers-fuzzy-matching t
      helm-recentf-fuzzy-match t)

(use-package semantic :config (semantic-mode 1))
(global-set-key (kbd "C-x C-m") \='helm-semantic-or-imenu)
(setq helm-semantic-fuzzy-match t
      helm-imenu-fuzzy-match t)
(global-set-key (kbd "C-x C-o") \='helm-occur)
```

## Dired

```emacs-lisp
(setq dired-listing-switches "-AlShr")
```

## Projectile

```emacs-lisp
(use-package projectile
  :config
  (projectile-mode +1)
  (define-key projectile-mode-map (kbd "s-p") \='projectile-command-map)
  (define-key projectile-mode-map (kbd "C-c p") \='projectile-command-map))
```
