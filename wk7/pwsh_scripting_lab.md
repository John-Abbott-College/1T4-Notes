# Lab 2: Scripting with PowerShell
## 420-1T6 Productivity Tools, Fall 2022
**Date**: September 26, 2022
**Worth**: 2% of your total grade (from the Assignment 2 evaluation component)
**Submission**: In-class
**Due**: TBD (end of class most likely)

# Objective
- to get hands-on experience creating, running, and debugging powershell scripts
- to become familiar basic powershell commands and error messages

# Instructions
- Feel free to collaborate with your partners/classmates for this lab, however, your submissions will be assessed independently
- Scripting is difficult to get used to! You will encounter error messages and unexpected behavior. This lab will help you get some experience recognizing error messages and taking steps to solve them independently.
- In that spirit, if you are stuck, do the following before raising your hand to ask me a question:

    * Read all error messages slowly and carefully
    * Try to resolve the error messages (mostly they come down to typos and/or small mistakes)
    * Reread the course notes about the command you are running to ensure you understand how to use it
    * Ask your classmates if they understand the problem and can help (you will likely face similar mistakes)
    * When you do raise your hand, be ready to tell me what you have tried already! If you have tried nothing yet, I will tell you to try to solve the problem yourself first.


## Getting Started
Like we saw last week, the power of a command line shell over a GUI shell comes from its ability to run several commands in a row in a **script**.

> A script is a text file that contains multiple shell commands that can be executed sequentially.
>
> In **PowerShell specifically**, a script is a simple`` text file that ends with the **file extension .ps1**

To begin this lab, please visit the [Scripting w/ PowerShell](https://michaelhaaf.github.io/1T6-F22/#/wk7/pwsh_scripting) course notes from last week, and follow all instructions before the "Excercises" header. Raise your hand when you have finished running all of the commands and are prepared to show me that you have. **Do not skip this step, I want to make sure everyone's computer has permissions and everyone can run a few basic commands independently.**

## Practise
Now, we will use the commands we have learned over the past week to manually complete the File Compression task. Below are exercises to complete; please raise your hand when finished all of the exercises so I can assess. Review the course slides [Intro to the Command Line Shell](https://michaelhaaf.github.io/1T6-F22/#/wk7/intro-command-line) for instructions about how to use these commands if you are stuck.

1. Open "Windows Powershell ISE" to complete the following tasks. Open the script file you have created in the previous step, and add the following lines:

```
$input_file_path = ".\File_compression.zip"
$unzipped_folder = ".\script_compression"
$output_file = ".\compression_size.txt"
$output_folder = ".\Practice1"
```
Run the script by pressing F5 (or the Play button at the top)

In the terminal within Powershell, run the following commands:

```
> echo $input_file_path
> echo $unzipped_folder
> echo $output_file
> echo $output_folder
```

Now open a new terminal (not in Powershell ISE) and run the commands again. What is the difference in the output, and why? Write your answers in a .docx file

