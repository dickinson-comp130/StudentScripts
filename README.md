# StudentScripts
Scripts to be used by the students to manage assignments
  
Typical Instructor Usage:
  * Create an organization for the course.
  * Fork this repository into the organization to create a new repository for the specific course.
  * Update the configuration file in your fork to reflect the relevant information for the specific course.
  * Add repositories to the organization for each assignment.
    * See the assignment template repositories provided.
  * The scripts in the InstructorScripts repository can be used to pull/push all student work on a given assignment for grading purposes.

Typical Student Usage:
  * Create a GitHub account.
  * Use git to clone the course specific fork of this repository to their local machine (they do not need to fork it)
    * If they work on multiple machines they will need to do this on each one.
  * Run __DCgitSetup__ once to complete the configuation
    * If they work on multiple machines they will need to do this on each one.
  * Run __DCgitBegin__ once at the start of each assigment to get the assginment starter materials.
  * Run __DCgitPull__ at the start of each work session to pull the latest work from their GitHub to their local machine.
  * Work on the assignment.
  * Run __DCgitPush__ at the end of each work session to push the latest work from the local machine to their GitHub.

Below are the details on each of the files in repository.

* __DCgitSetup__ _CourseID_
  * Run once for each course on each machine that the student uses to setup the common configuration parameters.
  
  * Parameters: 
    * None
    
  * Behavior:
    * Updates student specific information within the configuration file.
    * Configures git to know the username and password of the student.
    
* __DCgitBegin__ _AssignmentID_ [ _PartnerGitHubID_ ]
  * Run once at the beginning of an assignment to produce an editable version of the assignment that is available to the student.

  * Parameters:
    * _AssignmentID_ : The identifier of the repository in the course organization on GitHub.
    * [ _PartnerGitHubID_ ] : The GitHub username of a partner for a partnered assignment.
      * _NOTE_: Each assignment is configured to be indiviudal or partnered in the course repository. For assignments configured to be individual, the script will terminate if a _PartnerGitHubID_ is specified.
      
   * Error Conditions:
     * If the confifguration file for the course does not exist or has not been updated the script terminates with a suggestion to be sure that _DCgitSetup_ is run prior to use of the other scripts.
     * If the _PartnerGitHubID_ is specified and does not exist on GitHub then the script terminates with a suggestion to check the _PartnerGitHubID_.
     * If the _PartnerGitHubID_ is specified but the assignment is an individual assignment then the script terminates with an indication that the assignment is not partnered and the _PartnerGitHubID_ should be omitted.
     * If the repository already exists in the student's GitHub, the partner's GitHub and/or on the local machine then the script terminates indicating that the assignment already exists and that _DCgitPull_ may be the intended action. 
     * If the repository does not exist in the course organization the script terminates with a suggestion to check the _AssignmentID_.
     
   * Behavior:
     * The repository is copied from the course organization into the student's GitHub as a private repository.
       * _NOTE:_ The repository is copied rather than being forked because it will be a private repositiory in the student's GitHub
     * If a _PartnerGitHubID_ is specified, the partner will be established as a collaborator on the private repository.
     * The instructor(s) for the course are added as collaborators on the private repository.
     * The repository for the assignment is cloned to the student's local machine.
     * If a _PartnerGitHubID_ is specified the partner's origin is also setup as a remote.
    
* __DCgitPull__ [ ForceLocal | ForceRemote | Merge ]
  * Run within the assignment repository at the beginning of each work session to pull the editable version of the assignment down from GitHub to the student's local machine.
  
  * Parameters:
    * [ ForceLocal | ForceRemote | Merge ] : Force merge conflicts to be resolved in favor of the local version, in favor of the version currently on GitHub, or to launch a merge tool that will allow the conflicts to be resoved.
      
  * Error Conditions:
    * If the confifguration file for the course does not exist or has not been updated the script terminates with a suggestion to be sure that _DCgitSetup_ is run prior to use of the other scripts.
    * If the origin repository does not exist on GitHub the script fails, explaining that the assignment has been removed from GitHub and that help should be sought to resolve the issue.
  
  * Behavior:
    * Pull the contents of the repository from the origin.  
      * If there are merge conflicts the script will terminate with a message suggesting the use of one of the _[ ForceLocal | ForceRemote | Merge ]_ flags.
    
* __DCgitPush__ [ ForceLocal ]
  * Run within the assignment repository at the end of each work session to push the current version of the assignment from the student's local machine up to GitHub both in the student's GitHub and the partner's if the assignment is paired.
    
  * Parameters:
    * [ ForceLocal ] : Forces the repository on GitHub to look identical to the version on the local machine.
    
  * Error Conditions:
    * If the confifguration file for the course does not exist or has not been updated the script terminates with a suggestion to be sure that _DCgitSetup_ is run prior to use of the other scripts.
    * If all remote repositories (origin plus the partners if it is a partnered assignment) do not exist on GitHub the script fails, explaining that the assignment has been removed from GitHub and that help should be sought to resolve the issue.
    
  * Behavior:
    * All of the changes to the local repository are committed using the date/time and local username as the commit message.
    * The local repository is pushed from the local machine to all remotes on GitHub. 
      * If there are merge conflicts the script terminates and a suggestion is made that either a _DCgitPull_ be done to resolve the conflict or that the [ ForceLocal ] flag be used.
    
* __DCgitExpunge__
  * Run from within an assignment repository to remove an assignment completely from the local machine and the student's GitHub.  This is typically used when something goes horribly wrong and they want to start over from scratch.

  * Parameters:
    * None
    
  * Error Conditions:
    * If the confifguration file for the course does not exist or has not been updated the script terminates with a suggestion to be sure that _DCgitSetup_ is run prior to use of the other scripts.
    
  * Behavior:
    * The local repositiory is removed.
    * The repository on the student's GitHub is removed.

_NOTE_: These scripts have been produced for the specific use cases invovled in our course.  They are not designed to handle every use case. Nor are they designed to prohibit sufficiently motivated and unethical students from working around the basic restrictions that they impose.  The code has been released under a Creative Commons Licence (see below) so please feel free to adapt to your purposes. Pull requests that handle additional use cases or improve the robustness of the scripts in a sufficently general way that does not impact our specific usecases are welcome and will be considered on a case-by-case basis.

___
![Creative Commons Attribution-NonCommercial 4.0 International License](https://i.creativecommons.org/l/by-nc/4.0/88x31.png "Creative Commons Attribution-NonCommercial 4.0 International License")
###### All materials used in Dickinson College COMP 130</span> are licensed under a [Creative Commons Attribution-NonCommercial 4.0 International ](http://creativecommons.org/licenses/by-nc/4.0/)
