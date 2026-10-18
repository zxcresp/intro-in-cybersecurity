# Linux - Access Permissions

## What Are Access Permissions?

**Access permissions** determine who can read, modify, or execute a file, as well as view the contents of a directory.

In Linux, there are three categories of users:

- `u` - User: the owner of the file.
- `g` - Group: the group associated with the file.
- `o` - Others: all other users.
- `a` - All: all categories combined.

**Note: There are only 3 actual categories; "a" (All) is not a separate category—it simply represents all of them together.
Don't confuse the two!**

## Types of Permissions

| Symbol | Permission | For a File | For a Directory |
|---|---|---|---|
| `r` | Read (4) | Read the contents | View file names |
| `w` | Write (2) | Modify the contents | Create, delete, and rename entries |
| `x` | Execute (1) | Execute the file | Traverse the directory and access objects within it (provided other necessary permissions are met) |

## Viewing Permissions

Command:

```
ls -l
```

Example output:

```
-rwxr-xr-- 1 user developers 1200 Oct 18 script.sh
```

Breakdown:

```
- | rwx | r-x | r--
| u  | g  | o
```

- The first character indicates the type of object: `-` for a regular file, `d` for a directory.
- `rwx` - permissions for the owner.
- `r-x` - permissions for the group.
- `r--` - permissions for others.

In this example, the owner can read, modify, and execute the file; the group can read and execute it; and others can only read it.

## Changing Permissions - chmod

The `chmod` command is used to change access permissions. ### Symbolic Notation

Add execute permission for the owner:

```
chmod u+x script.sh
```

Remove write permission for the group:

```
chmod g-w script.sh
```

Set read permission for everyone:

```
chmod a+r file.txt
```

### Numeric Notation

Permissions are represented by numbers:

- `r = 4`
- `w = 2`
- `x = 1`

The numbers are added together for each category.

| Number | Permissions |
|---|---|
| `7` | `rwx` |
| `6` | `rw-` |
| `5` | `r-x` |
| `4` | `r--` |
| `0` | `---` |

Examples:

```
chmod 755 script.sh
chmod 644 file.txt
chmod 700 private.sh
```

- `755` → owner `rwx`, group `r-x`, others `r-x`
- `644` → owner `rw-`, group `r--`, others `r--`
- `700` → only the owner has `rwx` permissions

## How does it work?

The numbers are added together to obtain the desired set of permissions:

```
Calculation	Result	Permissions
4 + 2 + 1	7	rwx
4 + 2	        6	rw-
4 + 1	        5	r-x
4	        4	r--
2 + 1	        3	-wx
0	        0	---
```

## Changing the Owner - chown

The `chown` command changes the owner of a file or directory.
```
sudo chown user file.txt
```

Change the owner and group:

```
sudo chown user:developers file.txt
```

Recursively change the owner for a directory and its contents:

```
sudo chown -R user:developers directory/
```

The `-R` flag applies the change to all contents of the directory.

## Directory Permissions

For directories, permissions work differently than they do for regular files:

- `r` — allows viewing the list of names (files/subdirectories) within the directory.
- `w` — allows modifying the directory's entries (e.g., creating or deleting files), provided the `x` permission is also present.
- `x` — allows traversing (entering) the directory and accessing objects within it, provided the necessary permissions for those objects are held.

For example, without the `x` permission, a user might be unable to open a file via its path, even if they know the file's name.

## Important Security Considerations

- Do not grant `777` permissions unless absolutely necessary: ​​this allows anyone to read, modify, and execute a file, or—in the case of a directory—to modify its contents and traverse it.
- Do not run commands with `sudo` unless you understand why elevated privileges are required.
- While permissions help restrict user actions, they are not a substitute for other security mechanisms.
- Use `ls -l` to view the owner, group, and permissions.

## Key Takeaways

- `ls -l` — view permissions.
- `chmod` — change permissions.
- `chown` — change owner and group.
- `r = 4`, `w = 2`, `x = 1`.
- `755` — the owner has full access; the group and others can read and execute.
- `644` — the owner can read and modify; others can only read.
