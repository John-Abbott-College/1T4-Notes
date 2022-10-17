# Assignment 2: Scripting with PowerShell
## 420-1T6 Productivity Tools, Fall 2022

**Version & Date**: <span style="color:red">Version 2, October 17, 2022 (changes in red) </span> 

**Worth**: 6% of your total grade (from the Assignment 2 evaluation component)

**Submission**: A single .zip file named *LastName_Assignment2.zip* containing all scripts

**Due**: <span style="color:red"> Mon, Oct 24, at 23h59 </span>

**Late Policy**: 2.5% deduction in grades per day late by default. Please contact me by MIO if extenuating circumstances preven you from submitting on time.

# Context
In the first lab of this course you compared the compression ratios of different files by manually recording their sizes before and after compression. In subsequent labs we have seen how we can chain together several commands using **scripts**. In this assignment you will create a script to automate the compression ratio calculation problem.

# Getting Started
On LEA, I have shared a .zip file named "a2_scripts.zip". Inside of this file are 4 incomplete scripts. Each script, when complete, will correspond to a set of tasks that we can automate: script_1.ps1 corresponds to Part 1 of this assignment, script_2.ps1 for Part 2, etc. Your task will be to complete these scripts. **Download a2_scripts.zip from LEA and extract the 4 scripts to a location on your lab computer where you will be able to save your progress and find them again.**

Each script can be completed independently from the other scripts; however, the overall tasks will only work when all four are complete. Therefore, you can focus on completing one script at a time. For each script, I have included an explanation of the expected behavior. **Make sure you test your scripts by following the instructions!**

# Important Notes

* Your scripts will create folders/files as well as use existing folders/files. Pay close attention to the names of all files/folders/paths in these instructions. All specified file and folder names are **marked in bold** and must be matched exactly. Pay close attention to upper and lower case letters as well as spaces, dashs, underscores, etc.

# Part 1: Download, Extract, and Create an Output File (1.5%)

Complete the script named `script_1.ps1` so that it can execute the tasks below:

1. Create a folder named **Assignment-2** inside the user’s **Documents** folder. (Hint: the path "\~\Documents" will always lead to the users Documents directory, "\~\Desktop" to the users Desktop directory, etc. The tilde character `~` is a useful shortcut for this behavior)
2. Download the zip file from the URL below and save it inside Assignment 2 under the name compressed_files.zip.
https://github.com/michaelhaaf/1T6-F22/raw/main/wk6/compressed_files.zip
3. Extract compressed_files.zip inside the Assignment-2 folder.
4. <span style="color:red"> Remove the compressed_files.zip file -- it is no longer needed once it has been extracted.</span> 
5. <span style="color:red"> In the same directory as Assignment-2 (the user Document folder), create a new txt file named compression.txt </span>
6. Append the text below as the first line inside compression.txt. This line will act as the header for the data you extract in subsequent scripts.

> Filename,Size(Bytes)

Your script will be complete when you can run the following command from powershell, and can observe that the above folders are created/extracted and the above files are created as specified.

```powershell
PS C:\Folder-Where-Your-Scripts-Are-Stored> .\script_1.ps1
```

# Part 2: Compress Individual Files (1.5%)

Complete the script named `script_2.ps1` so that it can execute the tasks below:

1. Find all of the filenames in the **Assignment-2** directory that need to be compressed
2. Using a for loop (this has been provided to you), execute the Compress-Archive command on each file, saving the .zip version of the file in the **Assignment-2** directory.

Your script will be complete when you can run the following command from powershell, and can observe that all of the files in the **Assignment-2** directory have been compressed.

```powershell
PS C:\Folder-Where-Your-Scripts-Are-Stored> .\script_2.ps1
```

# Part 3: Calculate File Sizes (1.5%)

Complete the script named `script_3.ps1` so that it can execute the tasks below:

1. Find all of the filenames in the **Assignment-2** directory that ARE compressed (.zip in the name: <span style="color:red"> Get-ChildItem and Wildcards will be useful: see the course notes about Wildcards in Week 5) </span>.
2. Using a for loop (this has been provided to you), determine the filename and length (file size) of each compressed file. Append the results to the compression.txt output file created in Part 1, using the same structure as the heading (Filename,Size(Bytes)).
3. Find all of the filenames in the **Assignment-2** directory that are NOT compressed
4. Using a for loop (similar to previous step), determine the filename and length (file size) of each NON-compressed file (that is, everything that DOES NOT have .zip in the name). <span style="color:red"> You will find the `-Exclude` parameter of the `Get-ChildItem` command useful in combination with Wildcards from the previous step. </span>
  
Append the results to the compression.txt output file created in Part 1, using the same structure as the heading (Filename,Size(Bytes)).

6.
(BONUS: you're on your own!). Using the results from the previous steps, attempt to compute the compression ratio and add it to the output file using the structure (Filename,CompressionRatio)

Your script will be complete when you can run the following command from powershell, and can observe that the **compression.txt** file contains the file size information needed. 

```powershell
PS C:\Folder-Where-Your-Scripts-Are-Stored> .\script_3.ps1
```

# Part 4: Create Final Output and Clean-up (1.5%)

Complete the script named `script_4.ps1` so that it can execute the tasks below:

1. Create the folder structure shown in Lab 1 (see the [Lab 1 instructions](wk2/lab1-compression)) 
2. Move the compressed (.zip) files in the **Assignment-2** folder to the appropriate subfolder (audio files in the Audio directory, word files in the MS Word directory, etc.)
3. Delete any uncomressed files (all non .zip files) in the **Assignment-2** folder.
4. Compress the folder **Assignment-2** to the compressed file **Assignment-2.zip**.

Your script will be complete when you can run the following command from powershell, and can observe that the folder **Assignment-2** has been rearranged to the specified folder structure above, and compressed to a .zip file.

```powershell
PS C:\Folder-Where-Your-Scripts-Are-Stored> .\script_4.ps1
```

# Submission

Place all four of your completed scripts inside a .zip file named **LastName_Assignment2.zip** and submit it to LEA.
