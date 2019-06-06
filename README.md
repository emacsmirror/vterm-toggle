# vterm-toggle Toggle vterm buffer

[emacs-libvterm](https://github.com/akermu/emacs-libvterm)  implements a bridge to libvterm to display a terminal in a emacs buffe.

and this  package provides the command `vterm-toggle` which toggles between the
 vterm buffer and whatever buffer you are editing.

 This is done in an "intelligent" way.  Features are:
 1. Starts a vterm if none is existent.
 2. Minimum distortion of your window configuration.
 3. When done in the vterm-buffer you are returned to the same window
    configuration you had before you toggled to the shell.
 4. If you desire, you automagically get a `cd` command in the shell to the
    directory where your current buffers file exists(`even remote file through tramp is supported `);
    just call `vterm-toggle-cd` instead of `vterm-toggle`.

```
(global-set-key [f2] 'vterm-toggle)
(global-set-key [C-f2] 'vterm-toggle-cd)

;Switch to next vterm buffer
(define-key vterm-mode-map (kbd "s-n")   'vterm-toggle-forward)
;Switch to previous vterm buffer
(define-key vterm-mode-map (kbd "s-p")   'vterm-toggle-backward)
```
# Customize
## show vterm buffer in current window
```
(setq vterm-toggle-fullscreen-p nil)
(setq display-buffer-alist
      '(
        ("vterm.*" ;; match your vterm buffer name
         (display-buffer-reuse-window display-buffer-same-window))
        ))


```
## show vterm buffer in left side
```
(setq vterm-toggle-fullscreen-p nil)
(setq display-buffer-alist
      '(
        ("vterm.*"
         (display-buffer-reuse-window display-buffer-in-side-window)
         (reusable-frames . visible)
         (side . left)
         (window-width . 0.5)
         )))
```
## show vterm buffer in bottom side
```
(setq vterm-toggle-fullscreen-p nil)
(setq display-buffer-alist
      '(
        ("vterm.*"
         (display-buffer-reuse-window display-buffer-in-side-window)
         (reusable-frames . visible)
         (side . bottom)
         (window-height . 0.5)
         )))
```
## vterm-toggle-prompt-regexp
you need make sure your shell prompt match this regexp

# work with awesome-tab (select next/prev vterm buffer)

   https://github.com/manateelazycat/awesome-tab

  you can custom awesome-tab and make all vterm buffer in a tab group
  and using `awesome-tab-forward` and  `awesome-tab-backward`
  switch from one vterm buffer to another.

```
(global-set-key  (kbd "s-n") 'awesome-tab-forward)
(global-set-key  (kbd "s-p") 'awesome-tab-backward)
```

```
(setq awesome-tab-buffer-groups-function 'vmacs-awesome-tab-buffer-groups)
(defun vmacs-awesome-tab-buffer-groups ()
  "`awesome-tab-buffer-groups' control buffers' group rules.
    All buffer name start with * will group to \"Emacs\" "
  (list
   (cond
    ((derived-mode-p 'eshell-mode 'term-mode 'shell-mode 'vterm-mode)
     "Term")
    ((string-match-p (rx (or
                          "\*Helm"
                          "\*helm"
                          "\*tramp"
                          "\*Completions\*"
                          "\*sdcv\*"
                          "\*Messages\*"
                          "\*Ido Completions\*"
                          ))
                     (buffer-name))
     "Emacs")
   ;; ((not (vmacs-show-tabbar-p)) nil) ; donot show tab for this buffer
    (t "Common"))))
```
