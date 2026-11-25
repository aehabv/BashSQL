# BashSQL: a tiny database in Bash

![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnubash&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)
![License](https://img.shields.io/badge/License-CC%20BY--NC%204.0-0a2c46?style=flat)

What does a database actually do underneath? I tried to find out by building a very small one out of nothing but Bash scripts, folders and text files. It has users, permissions, databases, tables, inserts, selects and deletes, all driven from a menu in the terminal.

## How it works

Everything is plain files, so you can open any of it in a text editor:

| Database idea | How BashSQL stores it |
|---|---|
| Admin users | One username per line in `DB_admins.db` |
| A database | A folder under `MySQL/DataBases/` |
| Who owns a database | An `owner.txt` file inside that folder |
| A table | A `.table` file. The first line holds the column names, then one comma-separated row per line |
| Primary key | The first column, which must be unique |

When you start it, `main.sh` checks who you are with `whoami`. Admins get the full menu. Everyone else can only read.

## What you can do

| Menu option | Script | Who can use it |
|---|---|---|
| Create a database user | `create_user.sh` | Admins |
| Delete a database user | `delete_user.sh` | Admins (the built-in `oracle` user can't be deleted) |
| Create a database (it adds `DB` to the name) | `create_database.sh` | Admins |
| Delete a database | `delete_database.sh` | The database's owner |
| Create a table | `create_table.sh` | The database's owner |
| Insert rows | `insert_table.sh` | The database's owner |
| Show or search a table | `select_search_table.sh` | Everyone |
| Delete rows matching some text | `delete_row.sh` | The database's owner |

There's a sample `EmployeesDB` in the repo, with a few familiar names from *The Office*.

## Caveats

It's a learning project, and the shortcuts show:

- **Values can't contain spaces or commas.** The insert script counts values by splitting on spaces, so "Michael Scott" counts as two values and the row is rejected. A comma inside a value would break the file format.
- **The primary key check is loose.** It searches the whole file for the key, so a matching value in any column counts as a duplicate.
- **No locking.** Two people writing to the same table at once could corrupt it.

## How to run

You need Bash on Linux, macOS or WSL.

```bash
git clone https://github.com/aehabv/BashSQL.git
cd BashSQL/MySQL
echo "$(whoami)" >> DB_admins.db   # make yourself an admin
bash main.sh
```

The scripts use relative paths, so run `main.sh` from inside the `MySQL` folder.

## What I'd like to do next

Store tables as proper CSV with quoting, so values can have spaces and commas, and add a simple `WHERE column = value` filter instead of free-text search.
