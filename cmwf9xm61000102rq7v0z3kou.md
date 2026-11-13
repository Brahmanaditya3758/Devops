---
title: "(Day 57) Task : Getting Started with Vim: Essential Commands for Beginners"
datePublished: 2026-11-13T17:52:02.403Z
cuid: cmwf9xm61000102rq7v0z3kou
slug: day-57-task-getting-started-with-vim-essential-commands-for-beginners
tags: tricks, aws, aws-trricks

---

If you work on Linux, sooner or later you will have to edit a file in the terminal. That is where vi and vim come in. Vi (Visual editor) is a classic command-line text editor, and Vim (Vi IMproved) is its more powerful modern version. Both are pre-installed on almost every Linux system, which makes them worth learning even if you prefer other editors like Emacs or the graphical gedit.

Vim feels strange at first because you don't just start typing. This guide covers the three modes and the commands you'll use every day.

# The Three Modes of Vim

### Vim works in three modes:

1. **Command mode:** the default when you open a file. Keys are commands, not text.
2. **Insert mode (edit mode):** where you actually type text.
3. **Extended command mode:** where you run commands that start with :, such as saving and quitting.

### To switch between them:

- Press `i` in command mode to start inserting text.
- Press `Esc` to return to command mode.
- Press `:` in command mode to enter extended command mode.

## **Command Mode: Moving Around :**

| Command | What it does |
| --- | --- |
| `gg` | Go to the beginning of the file |
| `G` | Go to the end of the file |
| `w` | Move forward one word |
| `b` | Move backward one word |
| `nw` | Move forward n words (e.g. `5w`) |
| `nb` | Move backward n words (e.g. `5b`) |

## **Undo and Redo :**

| Command | What it does |
| --- | --- |
| `u` | Undo the last change |
| `U` | Undo all changes on the current line |
| `Ctrl+R` | Redo the change you undid |

## **Copy and Paste :**

In Vim, copying is called "yanking".

| Command | What it does |
| --- | --- |
| `yy` | Copy the current line |
| `nyy` | Copy n lines (e.g. `4yy`) |
| `p` | Paste below the cursor |
| `P` | Paste above the cursor |

## **Deleting Text :**

| Command | What it does |
| --- | --- |
| `x` | Delete the character under the cursor (like the Delete key) |
| `X` | Delete the character before the cursor (like Backspace) |
| `dw` | Delete a word |
| `dd` | Delete the entire line |
| `ndd` | Delete n lines from the cursor (e.g. `5dd`) |

For example, typing `3dd` deletes three lines starting at the cursor. If you delete too much, press `u` to bring it back.

## **Searching :**

Type `/` followed by a word and press Enter to search forward, for example `/error`. Press `n` to jump to the next match and `N` to go to the previous one.

## **Extended Command Mode: Save and Quit :**

| Command | What it does |
| --- | --- |
| `:w` | Save the file |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |

## **The Question Every Beginner Asks: How Do I Exit?**

If you are stuck, don't panic. Press `Esc` to make sure you are in command mode, then type `:q!` and press Enter to leave without saving. To keep your work, type `:wq` instead.