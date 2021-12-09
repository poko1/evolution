# Evolution of Stack Overflow Code Snippets on GitHub
Finds out if duplicate codes exist in Git projects that explicitly reference Stack Overflow posts. If duplicates have been found, checks if the number of duplicate lines have changed in the final version of both the files since evolution.

## Datasets Used
* Sotorrent Dataset (24/Jan/2020) (https://console.cloud.google.com/bigquery?project=sotorrent-org&p=sotorrent-org&d=2020_12_31&page=dataset)
* GHCodeSnippetHistory (https://github.com/manessaraj/GHCodeSnippetHistory)

## Instructions
The project scripts are all written in Python 3 using Jupyter Notebook. Make sure you have Jupyter Notebook installed and then download all the scripts in the same directory. When running any scripts on a file, make sure that file is also in the same directory unless told otherwise. For detecting duplicates between files, it uses SIMIAN which can be downloaded from here: https://www.harukizaemon.com/simian/installation.html

### Detecting duplicates between initial/referred version of Stack Overflow and Git code files:
  * Download the ‘Initial’ folder and make sure it is in the same directory as your Jupyter Notebook.
  * Make sure you have the exported jar file inside the ‘Initial’ folder. The ‘Initial’ Folder here already contains the jar file.
  * To run a batch script command on all the files, simply open the script ‘simian initial.ipynb’ from Jupyter Notebook. Edit the directory to match where the dataset resides in your computer.
  * Run the script. It will take around 4-5 hours to run Simian on all 5125 pairs of files. The generated output file is called ‘initial.csv’. It has three columns which shows the names of the git and stack overflow code file pair and the number of duplicate lines that were found.
  * To run SIMIAN on the first 10 files, uncomment the lines below from the script or only keep a few pairs of files in the ‘Initial’ folder.
      * if x>10:
      * break

### Detecting duplicates in the final versions of those Stack Overflow and Git code files whose initial/ referred versions had duplicates
  * Download the ‘Final’ folder and make sure it is in the same directory as your Jupyter Notebook.
  * Make sure you have the exported jar file inside the ‘Final’ folder. The ‘Final’ Folder here already contains the jar file.
  * Make sure the ‘initial_result.csv’ file is inside the ‘Final’ folder. The ‘Final’ Folder here already contains this file but you can replace it with the one you reproduced in the previous step.
  * To run a batch script command on all the files, simply open the script ‘simian final.ipynb’ from Jupyter Notebook. Edit the directory to match where the dataset resides in your computer.
  * Run the script. It will take around 20 minutes to run Simian on all 201 pairs of files. The generated output file is called ‘final_result.csv’. It has three columns which shows the names of the git and stack overflow code file pair and the number of duplicate lines that were found.

## RQ1: What type of SO posts are referenced on Git?
Run the SQL codes from the file ‘RQ1.txt’ on top of the sotorrent dataset using Google Big Query to get the results.

## RQ2: How are SO code snippets reused & evolved on Git?
  * ### Data merge and extraction:
    * Merge all the CommitDB files under each of the worker files (in GHCodeSnippetHistory repo) into one file called CommitDBMerged.
    * Run the script ‘First Commit Date.ipynb’ to produce the file ‘FirstCommitDB.csv’ which contains the date of only the first commit that had introduced the reference to the Stack Overflow post.
    * Filter out the rows that do not have a PostType=2 from ‘FirstCommitDB.csv’ file.
    * Follow the instructions and run the SQL commands written on RQ2.txt using Google Big Query.
    * Remove duplicates from the generated csv file 'file_found.csv' by running ‘remove duplicates.ipynb’ script. It produces the file 'InitialwithSOCodeandGitCode.csv'. Delete the first column.
    
  * ### Initial/Referred Version Extraction:
    * Run 'Initial SOCode.ipynb' script on 'InitialwithSOCodeandGitCode.csv' to get the body of the version of SO posts when they were referrenced. Remove rows where SOCode is empty.
    * Run 'Initial GitCode.ipynb' script on 'InitialwithSOCodeandGitCode.csv' to get body of initial version of git code. Remove rows where GitCode is empty.
    
  * ### Final Version Extraction:
    * Rename 'InitialwithSOCodeandGitCode.csv' to 'FinalwithSOCodeandGitCode.csv' and remove the columns SOCode and GitCode.
    * Run 'Final SOCode.ipynb' script on FinalwithSOCodeandGitCode.csv to get body of the final version of SO posts. Remove rows where SOCode is empty.
    * Run 'Final GitCode.ipynb' script on FinalwithSOCodeandGitCode.csv to get body of final version of git code. Remove rows where GitCode is empty.
    
  * ### Trimming:
    * Now we need to make sure that both the files have the same number of rows and each row is referring to the same git projects, repositories, post id’s etc. that is, the only difference between them are the SOCode and GitCode columns (initial vs final versions). For this, you can write a script or follow the instructions on how we used the free version of Google Big Query mentioned in RQ2 Trimming.txt

  * ### Generating .txt code files:
    * To reproduce the text file pairs that are present inside the ‘Initial’ Folder, rename InitialwithSOCodeandGitCode.csv to InitialSO.csv and run the script ‘convert to file InitialGit’. Make sure to change the path to the directory that has your InitialSO.csv which should also be the directory where you want the script to generate the .txt file pairs. Next, rename ‘InitialSO.csv’ to ‘InitialGit.csv’ and run the script ‘convert to file InitialSO’.
    * Similar steps need to be taken to generate the text file pairs present inside ‘Final’ Folder. Rename FinalSOGit.csv to FinalwithSOCodeandGitCode.csv and run the script ‘convert to file FinalSO’. Make sure to change the path to the directory that has your FinalSO.csv which should also be the directory where you want the script to generate the .txt file pairs. Similarly, rename ‘FinalSO.csv’ to ‘FinalGit.csv’ and run the script ‘convert to file FinalGit’.

## Details on Files Upload:
The InitialwithSOCodeandGitCode.csv and FinalwithSOCodeandGitCode.csv files were used to generate the text files for the Initial and Final folder. Upon running SIMIAN, initial_result.csv and final_result.csv were produced. But to be more accurate, we then ran a script and some excel formulae to detect the forked projects and identified git code files which were present in multiple repositories. Upon filtering those out and only considering unique git code files, we reran SIMIAN and came up with final_change_trim.csv and initial_change_trim.csv files- the result of which were presented in the paper.

The file 'Examples.pdf' contains screenshots and file names for which duplicates were found. 

The file 'final with no duplicates.txt' contains list of files whose initial versions had duplicate lines but final versions no longer had any duplicates between them due to changes made in SO post and none reflected on git code.

## Disclaimer
Since there is a lot of back and forth between scripts, trimming/filtering and sql query, the dataset we have might not be exactly what is reproduced. A few rows here and there might not match but it will more or less be the same. A few scripts and excel formulae were used here and there for sorting and filtering purposes.

**Maliha Sultana**

- [Profile](https://github.com/poko1 "Maliha Sultana")
- [Email](mailto:malihasultana998@gmail.com?subject=Hi "Hi!")
## 🤝 Support

Contributions, issues, and feature requests are welcome!

Give a ⭐️ if you like this project!
