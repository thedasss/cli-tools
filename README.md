# Get Project Structure CLI

A lightweight **Bash CLI tool** to generate a clean project structure tree with **ignore support**, including `.gitignore` entries, custom excludes, and keyword-based filtering.  

This tool works **globally** on your system and can be run in any project directory to quickly visualize your project files and folders.  

---

## **Features**

- Automatically respects `.gitignore` files.
- Exclude folders/files using `-e` (extra excludes) or keywords with `-k`.
- Option to save the project tree to a file with `-o`.
- Works even if `tree` command is not installed (falls back to `find`).
- Safe handling of files with spaces and special characters.
- Simple to install and run globally.

---

## **Installation**

1. **Clone the repository**:


git clone https://github.com/your-username/get-project-structure.git
cd get-project-structure
Make the script executable:
chmod +x get-project-structure.sh
Move it to a global location:
sudo mv get-project-structure.sh /usr/local/bin/get-project-structure
sudo chmod +x /usr/local/bin/get-project-structure
Now you can use get-project-structure from any project folder.
Usage
Basic Command
get-project-structure
Displays all files and folders in the current project.
.gitignore entries are automatically excluded.
Save output to a file
get-project-structure -o structure.txt
Saves the tree to structure.txt in the current directory.
Exclude specific folders/files
get-project-structure -e node_modules -e dist
Excludes the node_modules and dist folders from the tree.
Exclude by keyword
get-project-structure -k test -k temp
Excludes any files or folders containing test or temp in their name.
Combine options
get-project-structure -o structure.txt -e node_modules -k test
Output is saved to structure.txt.
Excludes node_modules folder and any files/folders containing test.
Help
get-project-structure -h
Displays all options and usage instructions.
Example
Suppose you have the following project:
Mobile_App_React_Native/
├── node_modules/
├── src/
│   ├── App.js
│   └── utils.js
├── dist/
└── README.md
Running:
get-project-structure -o structure.txt -e node_modules -k dist
Output in structure.txt:
.
├── src
│   ├── App.js
│   └── utils.js
└── README.md
Requirements
Bash shell (#!/usr/bin/env bash)
Optional: tree command for prettier output (if missing, find is used)
Git (for cloning)
Contributing
Fork the repository
Make changes
Create a pull request
Star the repo if you find it useful! ⭐
