# 02 – Processes, Searching, and Filtering

## Finding files

Search entire filesystem (slow, noisy):

```bash
find / -type f -name <query>
````

Fast indexed lookup (may lag reality):

```bash
locate <name>
```

## Wildcards

* `?` single character
* `[]` character ranges
* `*` any length

Used incorrectly, wildcards cause catastrophic deletes. Respect them.

## Process visibility

List processes:

```bash
ps
ps aux
```

Filter processes:

```bash
ps aux | grep python3
```

Never trust grep alone. Validate PID and parent process.
