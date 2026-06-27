# File Permissions in Linux

## Project Description

The research team at my organization needed to update the file permissions for certain files and directories within the `projects` directory. The existing permissions did not reflect the level of authorization that should be given. Checking and updating these permissions helps keep the system secure. To complete this task, I performed the following tasks using Linux commands in the Bash shell.

---

## Check File and Directory Details

The following code demonstrates how I used Linux commands to determine the existing permissions set for a specific directory in the file system.

```bash
researcher2@a]c3e415e834:~$ cd projects
researcher2@a]c3e415e834:~/projects$ ls -la
```

**Output:**

```
total 32
drwxr-xr-x 3 researcher2 research_team 4096 Jun 27 10:00 .
drwxr-xr-x 3 researcher2 research_team 4096 Jun 27 10:00 ..
drwx--x--- 2 researcher2 research_team 4096 Jun 27 10:00 drafts
-rw-rw-rw- 1 researcher2 research_team   46 Jun 27 10:00 project_k.txt
-rw-r----- 1 researcher2 research_team   46 Jun 27 10:00 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Jun 27 10:00 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Jun 27 10:00 project_t.txt
-rw-w----- 1 researcher2 research_team   46 Jun 27 10:00 .project_x.txt
```

The first line of the `ls -la` output displays a `d` indicating `drafts` is a directory. The hidden file `.project_x.txt` is identified by the period (`.`) at the beginning of its name. The listing shows 10 characters for permissions, representing the user type and their read (`r`), write (`w`), and execute (`x`) permissions.

---

## Describe the Permissions String

The 10-character permissions string can be deconstructed to determine who is authorized to access the file and their specific permissions. The characters and what they represent are as follows:

| Position | Character | Meaning |
|----------|-----------|---------|
| 1st | `d` or `-` | **File type:** `d` = directory, `-` = regular file |
| 2nd–4th | `rwx` | **User (owner) permissions:** read, write, execute |
| 5th–7th | `rwx` | **Group permissions:** read, write, execute |
| 8th–10th | `rwx` | **Other permissions:** read, write, execute |

**Example:** For `project_k.txt`, the permissions string is `-rw-rw-rw-`:

- `-` → It is a regular file (not a directory).
- `rw-` → The **user** has read and write permissions.
- `rw-` → The **group** has read and write permissions.
- `rw-` → **Other** has read and write permissions.

---

## Change File Permissions

The organization determined that **other** shouldn't have write access to any of their files. To comply with this, I referred to the file permissions that I previously returned. I determined `project_k.txt` must have the write access removed for **other**.

The following code demonstrates how I used Linux commands to do this:

```bash
researcher2@a]c3e415e834:~/projects$ chmod o-w project_k.txt
researcher2@a]c3e415e834:~/projects$ ls -la project_k.txt
```

**Output:**

```
-rw-rw-r-- 1 researcher2 research_team 46 Jun 27 10:00 project_k.txt
```

The `chmod` command changes the permissions on files and directories. The first argument indicates what permissions should be changed, and the second argument specifies the file or directory. In this example, I removed write permissions from **other** for the `project_k.txt` file. After this, I used `ls -la` to review the updates I made.

---

## Change File Permissions on a Hidden File

The research team recently archived `project_x.txt`. They do not want anyone to have write access to this project, but the user and group should have read access.

The following code demonstrates how I used Linux commands to change the permissions:

```bash
researcher2@a]c3e415e834:~/projects$ chmod u-w,g-w,g+r .project_x.txt
researcher2@a]c3e415e834:~/projects$ ls -la .project_x.txt
```

**Output:**

```
-r--r----- 1 researcher2 research_team 46 Jun 27 10:00 .project_x.txt
```

I know `.project_x.txt` is a hidden file because it starts with a period (`.`). In this example, I removed write permissions from the **user** with `u-w`, then removed write permissions from the **group** with `g-w`, and added read permissions to the **group** with `g+r`.

---

## Change Directory Permissions

The organization only wants the `researcher2` user to have access to the `drafts` directory and its contents. This means no one other than `researcher2` should have execute permissions.

The following code demonstrates how I used Linux commands to change the permissions:

```bash
researcher2@a]c3e415e834:~/projects$ chmod g-x drafts
researcher2@a]c3e415e834:~/projects$ ls -la
```

**Output (drafts line):**

```
drwx------ 2 researcher2 research_team 4096 Jun 27 10:00 drafts
```

I previously determined that the **group** had execute permissions, so I used the `chmod` command to remove them. The `researcher2` user already had execute permissions, so they did not need to be added.

---

## Summary

I changed multiple permissions to match the level of authorization my organization wanted for files and directories in the `projects` directory. The first step in this was using `ls -la` to check the permissions for the directory. This informed my decisions in the following steps. I then used the `chmod` command multiple times to change the permissions on files and directories.

---

> **Key Linux Commands Used:**
>
> | Command | Description |
> |---------|-------------|
> | `ls -la` | Lists all files (including hidden) with detailed permissions |
> | `chmod` | Changes file/directory permissions |
> | `cd` | Changes the current working directory |
>
> **Permission Notation:**
>
> | Symbol | Meaning |
> |--------|---------|
> | `u` | User (owner) |
> | `g` | Group |
> | `o` | Other |
> | `+` | Add permission |
> | `-` | Remove permission |
> | `r` | Read |
> | `w` | Write |
> | `x` | Execute |
