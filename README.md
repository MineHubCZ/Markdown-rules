# Rules
This is simple (yet unefficient) script for compiling game server rules from custom Markdown superset to standard Markdown output including mentions function.

## Requirements
- PHP >7.4

## Why not just use Markdown?
This script enables you to automatically number parts of rules with option to mention certain rules without having to replace all mentions when new rule is added before the mentioning one.

## What is the syntax?
- The input file syntax includes all formatting from Markdown except for h1.
- Structure of the rules file is **head - section - paragraph** (or **hlava - paragraf - odstavec** in Czech).
- It uses three types of character sequences to structure the rules:
  - `#` - head,
  - `§` - section,
  - `i:` (with `i` being any integer) - paragraph.
- It also uses `@i` (with `i` being any integer) for mentioning rules.
- The `i` integer stands for unique rule number to be used in mentions.

## Example
Let's say this is our input file called `in.md`:
```markdown
# General rules
    § Behavior
        1: Do not steal from other players
        2: Be nice to other players
    § Punishments
        3: If you misbehave in chat, you will get muted
# Discord rules
    § Chat
        4: Do not spam
        5: Do not use profanities (@2)
        6: Do not send unsolicited Discord invites (or you will get muted - @3)
```
after compiling it using
```bash
$ php compile in.md out.md
```
these are contents of `out.md` file:
```markdown
## Hlava I. - General rules
### § 1 - Behavior
1. Do not steal from other players
2. Be nice to other players

### § 2 - Punishments
1. If you misbehave in chat, you will get muted

## Hlava II. - Discord rules
### § 3 - Chat
1. Do not spam
2. Do not use profanities (§ 1 odst. 2)
3. Do not send unsolicited Discord invites (or you will get muted - § 2 odst. 1)
```
As you can see:
 - all heads got numbered with romenian numerals and turned into h2,
 - all sections got numbered and turned into h3,
 - all paragraphs got numbered **within** each section and turned into ordered list,
 - the rule mentions (`@2`, `@3`) got transcripted into information about the section and paragraph numbers of the mentioned paragraphs (`§ 1 odst. 2`, `§ 2 odst. 1`).

### Adding new rules
If we were to add new rule just before the rule number 3, we would add it right there with new unique number:
```markdown
# General rules
    § Behavior
        1: Do not steal from other players
        2: Be nice to other players
    § Punishments
        7: If you misbehave in game, you will get banned
        3: If you misbehave in chat, you will get muted
# Discord rules
    § Chat
        4: Do not spam
        5: Do not use profanities (@2)
        6: Do not send unsolicited Discord invites (or you will get muted - @3)
```
after compiling again we would get:
```markdown
## Hlava I. - General rules
### § 1 - Behavior
1. Do not steal from other players
2. Be nice to other players

### § 2 - Punishments
1. If you misbehave in game, you will get banned
2. If you misbehave in chat, you will get muted

## Hlava II. - Discord rules
### § 3 - Chat
1. Do not spam
2. Do not use profanities (§ 1 odst. 2)
3. Do not send unsolicited Discord invites (or you will get muted - § 2 odst. 2)
```
with all paragraphs in correct order and with correct information about mentioned paragraphs.