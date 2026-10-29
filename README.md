# nxnative-ipa-builder

I adapted and updated the guide taken from u/PersonalAnswer8664 [Reddit].

This beginner-friendly guide explains how to use GitHub Actions to build an iOS IPA with nxnative without needing a Mac. You will use Dropbox to host your game files and GitHub to run the build on a cloud macOS machine.
You do not need to know how to code, and you do not need to fork either nxnative source repository. You will use this GitHub repository containing the build workflow.

1. Before starting, check if your game has been checked to work fine on the nxnative original repo.
WARNING: Any game not on this list is not guaranteed to work. Even games on the list may fail to build or run correctly on your particular device. This is a list of reported titles, not a guarantee that every game is compatible.
Use game dumps and keys you are legally entitled to use.

2. What you need
Before starting, gather these items:
A GitHub account: https://github.com/
A Dropbox account: https://dropbox.com/
A legally obtained Nintendo Switch game dump, normally an .nsp or .xci file, or an archive containing one.
The matching prod.keys file.
The matching game update, if applicable.
The matching DLC files, if applicable.
You do not need a Mac. GitHub Actions provides the cloud build machine.
Keep your keys private. Do not post them publicly or share them with other people. Anyone with access to your keys may be able to access protected game content.

3. Upload your files to Dropbox
Dropbox will host the files so the GitHub workflow can download them during the build.
Step 1: Upload the files
Open https://dropbox.com/ and sign in. IMPORTANT: the original guide used Google Drive cause it offered much more free apace, with dropbox i advise you to start the free month trial.
Click +.
Choose File upload.
Select your base game file and wait for it to finish uploading.
Repeat for your prod.keys file.
Upload the game update and DLC files if you have them and intend to use them.
Keep each file separate so you can identify which link belongs to which file.

4. Create a GitHub account
If you already have a GitHub account, skip to Section 5.
Open https://github.com/.
Click Sign up.
Enter your email address and create a password.
Complete the verification steps.
Verify your email if requested.
Sign in to GitHub.
You do not need a paid plan just to try this workflow, although usage limits and runner availability can vary.

5. Add your Dropbox links as GitHub secrets
GitHub secrets store the links privately so you don’t have to write them directly into the workflow.
Open your new repository.
Click Settings.
In the sidebar, find Secrets and variables.
Click Actions.
Under Repository secrets, click New repository secret.
Enter the secret name exactly as shown below.
Paste the matching Google Drive sharing link into the Secret field.
Click Add secret.
IMPORTANT: for every Dropbox link, change the last number of the url, the 0, into a 1. This will make your link a direct download, which Github can work on.
Required secrets
First secret:
Game_URL
Dropbox sharing link for the base game
Second secret:
Keys_URL
Dropbox sharing link for your prod.keys file
Optional secrets
Secret name
Third secret:
Update_URL
Dropbox sharing link for the game update
Fourth secret:
DLC_URL
Dropbox sharing link for the DLC 
Only add the update and DLC secrets if you have the corresponding files and want to use them.
In the case of more than 1 DLC or Update, I don't know what to do. The og guide didn't explain it, so for my builds I just used one DLC and one Update. Choose wisely! 
Important: The names must match exactly, including capitalization and underscores. Game_URL is not the same as game_url.
Do not paste the contents of your keys file into a secret. The workflow expects a link from which it can download the file.
To create or change a secret:
Open Settings → Secrets and variables → Actions.
Select the secret you need to create or update.
Paste the appropriate Dropbox link.
Save it.
GitHub won’t show you the full value of an existing secret after saving it, so double-check the link before you click Add secret and make sure to put the 1 instead of the 0 at the end of it.
GitHub’s instructions for repository secrets: https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets

7. What if the Dropbox link doesn’t work?
The workflow needs to download the actual file, not a page that asks you to sign in. Make sure:
You copied the link for the correct file.
The file is fully uploaded.
Dropbox isn’t blocking the download or requiring an extra confirmation.

8. Run the build
Open the repository.
Click the Actions tab.
Select the workflow from the left sidebar.
Click Run workflow.
Select the branch containing your workflow, usually main.
Choose the device option.
The workflow provides these intended build paths:
iPhone 15 or older (BC fallback): selects the pinned BC-texture fallback build.
iPhone 15 Pro/Pro Max or newer, iPads with M chips (standard): selects the standard upstream build.
These are build-path choices, not guarantees that every game will work on every device. If you aren’t sure which option to choose, consult the compatibility information for your device and build.
If the workflow asks for a profile ID override, leave it blank or at its default unless you know you need to override automatic detection.
IMPORTANT: the workflow couldn't detect my game ID so I manually put it in the box when asked.
Click Run workflow to start the build.
GitHub will allocate a runner and execute the workflow. The build can take some time while dependencies are installed, files are downloaded, and the game is compiled.

9. Monitor progress and troubleshoot errors
After starting the workflow:
Stay on the Actions tab.
Click the new workflow run.
Click the job to open its detailed log.
Watch each step as it runs.
A green check mark means the workflow completed successfully. A red X means something failed.
Common problems
A. Dropbox download fails
Possible causes:
The file is still Restricted.
The URL was copied incorrectly.
The file is too large or Dropbox has temporarily limited downloads.
Check the sharing settings and confirm that the secret points to the correct file.
B. The keys file is missing or invalid
Check that Keys_URL points to the actual keys file and that the file is valid and compatible with your game dump.
C. The game file cannot be detected
Check that the game dump downloaded completely and is in a supported format. If the workflow cannot identify the title ID, the log may contain a detection error.
D. The build fails while installing dependencies or compiling
Read the first meaningful error in the log. Later errors may simply be consequences of the first failure. The standard upstream source and pinned fallback source may also change or behave differently.
E. The build succeeds, but the game doesn’t run properly
A successful build does not guarantee compatibility. Check whether the game appears in the reported game list, whether you chose the intended device option, and whether the game dump, update, DLC, and keys match.
Use an AI assistant to diagnose errors
If you’re stuck, copy the relevant error message and surrounding log lines into an AI assistant.
You can use this prompt:
I’m trying to build an iOS IPA using nxnative and a GitHub Actions workflow. The build failed at the following step. Please identify the first actual error, explain what caused it, and give me step-by-step instructions to fix it. Don’t assume the build succeeded if the logs say otherwise.

[Paste the relevant log lines here.]
Include the failed step’s name and the device option you selected.
Remove sensitive information before sharing logs. Check for Google Drive URLs, tokens, private repository information, and other credentials. Never upload your keys file or game dump to an AI assistant just to diagnose an error. Usually, the error message and nearby log lines are enough.

10. Download the finished IPA
Once the workflow completes successfully:
Open the completed run under the Actions tab.
Scroll down to the Artifacts section on the run summary page.
Find the IPA artifact. Its name may identify the build type, such as BC-Fallback or Standard.
Click the artifact name to download it.
GitHub will normally download the artifact as a ZIP file.
Open the ZIP and extract the .ipa file.
You now have the IPA produced by the workflow. You can proceed with your usual iOS sideloading method.
If there is no IPA artifact, check the workflow log for the first failed step. A build that fails before the upload step will not produce the expected artifact.

Final checklist
☐ Game dump and matching keys prepared.
☐ Update and DLC prepared, if applicable.
☐ Files uploaded to Dropbox.
☐ Required files shared with the appropriate link access.
☐ GitHub account and repository created.
☐ Required secrets added with exact names.
☐ Correct device option selected.
☐ Build completed successfully.
☐ IPA artifact downloaded and extracted.
That’s it! Add your own secrets, and use the workflow whenever you want to build another compatible game.
Remember that this is a community workflow, not a guarantee that every game will build or run correctly on every device. If a build fails, save the relevant log lines and troubleshoot the first error before trying again.