# 🖥️ VIM Commands Cheat Sheet

A quick reference guide for commonly used **VIM commands**.

---

## 🔹 Navigation

- `w` → Move to the beginning of the **next word**
- `b` → Move to the beginning of the **previous word**
- `e` → Move to the **end of the word**
- `0` → Beginning of the line
- `$` → End of the line
- `gg` → Go to the **beginning of the file**
- `G` → Go to the **end of the file**
- `Ctrl + u` → Move up half a screen
- `Ctrl + d` → Move down half a screen
- `Ctrl + f` → Move forward a full screen
- `Ctrl + b` → Move backward a full screen

---

## 🔹 Insert Mode

- `i` → Insert before the cursor
- `a` → Insert after the cursor
- `o` → Open a new line **below** the current line
- `O` → Open a new line **above** the current line
- `Esc` → Exit insert mode

---

## 🔹 Editing

- `x` → Delete a character
- `r` → Replace a character
- `cw` → Change word under cursor
- `cc` → Change (replace) the entire line
- `dd` → Delete the current line
- `yy` → Copy (yank) the current line
- `p` → Paste after the cursor
- `P` → Paste before the cursor
- `u` → Undo last change
- `Ctrl + r` → Redo last undone change
- `.` → Repeat last command

---

## 🔹 Search & Replace

- `/(word)` → Search for a word
- `n` → Repeat search forward
- `N` → Repeat search backward
- `:%s/old/new/g` → Replace all occurrences of `old` with `new`
- `:%s/^/   /g` → Add spaces at the **start** of each line
- `:%s/^   //g` → Remove spaces at the **start** of each line

---

## 🔹 File Commands

- `:w` → Save (write) file
- `:w filename` → Save file with a new name
- `:q` → Quit VIM
- `:q!` → Quit without saving
- `:wq` or `ZZ` → Save and quit
- `:e filename` → Open another file
- `:ls` → List open buffers
- `:b n` → Switch to buffer `n`
- `:sp filename` → Open file in a new horizontal split
- `:vsp filename` → Open file in a new vertical split

---

## 🔹 External (Shell) Commands

- `:!command` → Run a shell command inside VIM
- `:!ls` → List files in directory
- `:!mv oldfile newfile` → Rename file
- `netstat -a | grep ssh` → Check SSH connections (outside VIM)

---

✨ With these commands, you can move, edit, search, and manage files efficiently in VIM!
