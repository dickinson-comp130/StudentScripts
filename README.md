# StudentScripts
Scripts to be used by the students to get and submit assignments.

The files in this repository are:
* __DCgitSetup__
  * Run once on each machine that the student uses.  
  
  * Behavior:
    *  Generates the configuration file used by these scripts and also configures git to know the user and their password.
    *  All other scripts will look to see if the configuration file generated by this file exists.  If not, they will suggest running _DCgitSetup_ before using other commands.
    
* __DCgitBegin__ _AssignmentName_ [ _PartnerGitHubID_ ]
  * Run once at the beginning of an assignment to produce an editable version of the assignment that is available to the student, and possibly a partner. 

  * Parameters:
    * _AssignmentName_ : The name of the repository in the course organization on GitHub.
    * [ _PartnerGitHubID_ ] : The GitHub username of a partner for a partnered assignment.
      * _NOTE_: Each assignment is configured to be indiviudal or partnered in the course repository. For assignments configured to be individual, the script will terminate if a _PartnerGitHubID_ is specified.
      
   * Behavior:
     * If the repository does not exist in the student's GitHub or the GitHub indicated by the _PartnerGitHubID_ it is copied from the course organization into the student's GitHub as a private repository.
     * If the assignment is partnered and a _PartnerGitHubID_ is specified, the partner will be established as a collaborator on the private repository.
     * The instructors for the course, as configured by running the _DCgitSetup_ script, are added as collaborators on the private repository.
    
* __DCgitPull__ _AssignmentName_ [ _PartnerGitHubID_ ] [ ForceLocal | ForceRemote | Merge ]
  * Run at the beginning of each work session to pull the editable version of the assignment down from GitHub to the student's local machine.
  
  * Parameters:
    * _AssignmentName_ : The name of the repository in the course organization on GitHub.
    * [ _PartnerGitHubID_ ] : The GitHub username of the partner for a partnered assignment.
    * [ ForceLocal | ForceRemote | Merge ] : Force merge conflicts to be resolved in favor of the local version, in favor of the version currently on GitHub, or to launch a merge tool that will allow the conflicts to be resoved.
      
  * Behavior:
    * If the student is not in the requested repository on the local machine:
      * Attempt to clone the repository from the student's GitHub.  If the repository does not exist on the student's GitHub and a partner is specified, the script then attempt to clone the repository from the partner's GitHub.  If the repository does not exist in the partner's GitHub, the script fails and suggest that _DCgitBegin_ may need to be used if this is a new assignment.
      * A log file is created in the local the repository and the date/time/user are written in as the creation message.
      
    * If the student is in the requested repository on the local machine:
      * Pull the contents of the repository from the origin (either their GitHub or their partner's GitHub).  
        * If there are merge conflicts the script will terminate with a message suggesting the use of one of the _[ ForceLocal | ForceRemote | Merge ]_ flags.
      * The date/time/user, the command used and the results are added to the log file in the local repository.
    
* __DCgitPush__ _AssignmentName_ [ ForceLocal ]
  * Run at the end of each work session to push the current version of the assignment from the student's local machine up to GitHub.
    
  * Parameters:
    * _AssignmentName_ : The name of the repository in the course organization on GitHub.
    * [ ForceLocal ] : Forces the assignment on GitHub to look identical to the version on the local machine.
    
  * Behavior:
    * The date/time/user and the command being used are written to the log file in the local repository.
    * All of the changes to the local repository are committed using the date/time/username as the commit message.
    * The local repository is pushed from the local machine to GitHub. 
      * If there are merge conflicts the script terminates, a message including date/time/user is added to the log file and a suggestion is made that either a _DCgitPull_ be done to resolve the conflict or that the [ ForceLocal ] flag be used.
    
* __DCgitExpunge__ _AssignmentName_ [ _PartnerGitHubID_ ]
  * Run in order to start an assignment over again.
  
  * Parameters:
    * _AssignmentName_ : The name of the repository in the course organization on GitHub.
    
  * Behavior:
    * If the student is in the requested repository on the local machine the local repositiory is removed.
      * Otherwise the script terminates with a message indicating that the script must be run from the local respository and a message including date/time/user and the command is added to the log file.
    * If the local repository is successfully deleted the GitHub repository will be deleted.
      * If not run by the owner of the GitHub repository 
    

_NOTE_: These scripts have been produced for the specific use cases invovled in our course.  They are not designed to handle every use case. Nor are they designed to prohibit sufficiently motivated and unethical students from working around the basic restrictions that they impose.  The code has been released under a Creative Commons Licence (see below) so please feel free to adapt to your purposes. Pull requests that handle additional use cases or improve the robustness of the scripts in a sufficently general way that does not impact our specific usecases are welcome and will be considered on a case-by-case basis.

___
![Creative Commons Attribution-NonCommercial 4.0 International License](https://i.creativecommons.org/l/by-nc/4.0/88x31.png "Creative Commons Attribution-NonCommercial 4.0 International License")
###### All materials used in Dickinson College COMP 130</span> are licensed under a [Creative Commons Attribution-NonCommercial 4.0 International ](http://creativecommons.org/licenses/by-nc/4.0/)
