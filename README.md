# Magic-Gradle-Steps
1. Cloned FRC2021 code
2. Followed Thomas's magic steps described below

These steps worked to add the robototes 2022 WPILib-compatible version of the 2910 common library into our base robot project. See 2022 FRC code for implementation.

## Thomas works his magic: An Explanation
### 0. Need to get correct submodule from 2910 or robototes or some other team that has updated the OG 2910 library to the latests VSCode
* get `2022_updates` branch and pull the branch down

3 command line arguments: 
1. `git init` (to be able to do the other things) 
2. `git submodule add https://github.com/robototes/2910Common.git common` clones master branch 
3. `git config -f .gitmodules submodule.common.branch 2022_update` switches the branch to the 2022_update one 
4. You may need to do a git fetch (?). Doing `git submodule update --remote common` updates to 2022 branch

### 1. build.gradle and settings.gradle in main robot project
1. `include 'common'` in settings.gradle (at bottom of page)
2. `implementation project(':common')` inside dependencies{} in build.gradle

### 2. Set `id 'net.ltgt.errorprone' version '2.0.2'` in plugins{} function in the Common build.gradle
* we added version `2.0.2` where there previously was no version specification
* `net.ltgt.errorprone` is a github repo with version `2.0.2`. The `com.google.errorprone` stuff below in dependencies{} is different
this step may no longer be needed if 2910/robototes updates

### 3. Build 2910 Common stuff FIRST
* The main project is dependent on the smaller project's `.jar` file
* If the main project tries to compile before 2910 it's a problem because it needs the jar file from the 2910 compilation

The following steps manually determine build order and fix it
1. Put `gradlew build -Dorg.gradle.java.home="C:\Users\Public\wpilib\2022\jdk"` (or whatever VSCode terminal runs) into the command prompt. Do this inside the COMMON directory FIRST.
2. Then put the same prompt into the MAIN (robot code w/ subsystems/commands) NEXT.
3. Should be able to build in VSCode fine after.

Additional notes:
* There is probably a way to specify build order in gradle
