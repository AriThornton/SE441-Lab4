Ari Thornton

SE 441

Prof. Dong Jae Kim

Lab 4

# Part 1 – Set Up
1)	According to the most recent LTS release from January 2026, it requires Java 17, 21 or 25 and I currently have 17, 21, and 24 installed on my system. 
2)	The Jenkins default port is 8080 and is specifically configured on my system at /Library/LaunchAgents/homebrew.mxcl.jenkins-lts.plist.
3)	Screenshot after log in.

    ![image](https://github.com/AriThornton/SE441-Lab4/blob/9d96befa4343c8728fbd31cee585dc03cbe43978/screenshots/image.png)

4)	An executor in Jenkins is a thread on a node that executes pipeline stages.
5)	Jenkins runs as a service user rather than a personal user account for things like security, stability, isolation, and process management.

# Part 2 – Create a Simple Workflow
1)	The build failed. 

    ![image 1](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-1.png)

2)	The build failed because “cowsay not found. Installing cowsay...” and then “Homebrew not found. Please install Homebrew first.” which led to build step execute shell being marked build as failure.
3)	It seems like the failure was mac specific, so in order to fix it, I added export PATH="/opt/homebrew/bin:/usr/local/bin:$PATH" because of the homebrew not found message.

    ![image 2](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-2.png)

# Part 3 – Chained Free-Style Workflow
## Create a Jenkins build job:
1)	The build passed.

    ![image 3](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-3.png)

2)	The build passed because it only needed to send a HTTP GET request to a public API to return a joke in JSON format and save it to the joke.json file.
3)	It did not fail.
## Create a test job:
1)	The build technically passed but there was still an error during the processing. 

    ![image 4](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-4.png)

2)	I believe the build passed because there was nothing stopping the build even though it could not locate the file joke.json but still said it extracted the joke from joke.message and stated the word count and that it was short. 
3)	Technically it passes even with the error.
## Fix the test job:
1)	The initial build in both ASCII-BuildJob and ASCII-TestJob passed.

    ![image 5](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-5.png)

    ![image 6](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-6.png)

2)	Both builds passed because ASCII-BuildJob was successful in getting a joke and saving it to joke.json and with the update to the configuration to automatically build ASCII-TestJob which was able to locate the joke.json file from the first build.
3)	The build passed. No fixes needed.
## Install Copy Artifact:
1)	Builds initially failed.

    ![image 7](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-7.png)

    ![image 8](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-8.png)

2)	I believe it failed because I did not tell ASCII-BuildJob which file to archive. 
3)	I fixed this by updating the post-build actions in ASCII-BuildJob to archive joke.json.

    ![image 9](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-9.png)

    ![image 10](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-10.png)

## Check workspace for test job:
1)	The initial builds passed.

    ![image 11](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-11.png)

    ![image 12](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-12.png)

## Create a deployment job:
1)	Console output after building ASCII-DeployJob.

    ![image 13](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-13.png)
 
## Install a new plug in:
1)	The main reasons test job failed during part 3 was because it could not access joke.json and also could not copy the srtifacts from ASCII-BuildJob.
2)	A Jenkins workspace is a directory on a node where that specific job’s code is built and tested.
3)	Files can be transferred safely between jobs in Jenkins by archiving artifacts in the upstream job and copying them to the downstream job using the Copy Artifact Plugin. I used the Copy Artifact Plugin like I was directed to in the lab instructions.

# Part 4 – Failure Mode
1)	When you click build now it just keeps loading.

    ![image 14](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-14.png)

2)	This kind of behavior happens because adding sleep 10000 to the freestyle job script will cause the build to pause for 10000 seconds and will show itself as running.
3)	Doing this can cause delays leading to bottlenecks because it can potentially blocking parallel builds and resource contention.

# Part 5 – Pipeline Demo
1)	The first build passed but the second failed.
2)	It failed because ‘Tool type "maven" does not have an install of "M3" configured’.
3)	Fixed by adding Maven installations in Tools.
## Using Pipeline from source control system
1)	The main difference between pipeline script and pipeline script from SCM is where the pipeline definition is stored and versioned.
2)	When you use the “Pipeline script from SCM” in Jenkins, the pipeline definition is stored as a Jenkinsfile.
3)	If the branch specifier does not match the branch where your Jenkins file is located, then you will get an error like “couldn’t find remote ref refs/heads/master”.
4)	If the Jenkins file is not committed to GitHub first, then you would get an error that says it is unable to find the Jenkins file.
5)	If it fails, I think the first place you should look is the console output for the error and then at the configuration to see if anything needs changed or updated, and tehn look at github.

    ![image 15](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-15.png)
 
# Part 6 – Pipeline with Parameters
1)	I am unable to access the GitHub Classroom to complete part 6.

    ![image 16](https://github.com/AriThornton/SE441-Lab4/blob/a8019448721ed942512fb29558ad7390239551ac/screenshots/image-16.png)
 
2)	Parameterization can improve pipeline reusability by replacing hardcoded values with dynamic variables which allows the same workflow to execute across different configurations without changing the underlying code.

# Part 7 – Failure Mode on Pipeline
1)	Like above, I am unable to complete part 6 nor part 7.
2)	Pipelines are more resilient than Jenkins Freestyle jobs because they’re defined as code in version control which makes them recoverable, reproducible, and portable.
