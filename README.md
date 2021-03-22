### How to setup the Shift Left demo story with all stop-gaps
You can clone this repo and put it in the demo-build gogs server, so that can in turn be used in the demo-build Jenkins server with all pre-existing vars and passwors.
You can also point the demo-build server to this repo, once it becomes public, or to your fork, instead of copying to the gogs server.

Until this is committed to the demo-build source code and then its own Gogs and Jenkins servers, you'll need to clone this repo's content into your demo-build gogs server and then add a new pipeline in your Jenkins server based on that.

The Git scan is performed against https://github.com/se-cloud-emea/evil.petclinic so you'll need to add that into your Twistlock Console as a Git repo to scan, including a token: https://docs.twistlock.com/docs/compute_edition/vulnerability_management/code_repo_scanning.html

Link to running this demo: https://drive.google.com/file/d/1fFPoteVm61hqeQOb40Uzhdxw-DKDkLtG/view

PC_USER,PC_PASS and PC_CONSOLE are pre populated from the yaml file from the central demo build. 

### If you're using the demo-build env for this, then:
1. Clone this repo into the gogs server in demo-build, so all permissions are re-used. If you just point the Jenkins new pipeline to this repo, make sure you add an auth key to GitHub.
2. Add a new project in the Jenkins demo-build server and point its configuration to the `Jenkinsfile` that's in this repo you just cloned into gogs.
3. All auth keys are already part of the demo-build env and the config files here
4. To have the IaC scan work properly (with the Prisma Cloud Enterprise) in the DevOps inventory, then you'll need to have your demo-build deployment yaml file be configured with these fields (also documented in the demo-build how-to's):
  ```
  pc_token: abcdwxyz-1234-1234-1234-abc123xyz123       << access key from Prisma Cloud tenant (I use the SESandbox)
  pc_secret: Fblablablablablablablablabla=             << corresponding secret key
  pc_api: us-east1.cloud.twistlock.com/us-1-111573457  << The Compute Console URL in the tenant you're on (That's the SESandbox one)
  ```
  If the above isn't in your config, then you'll need to edit the demo-build deploy yml file and re-deploy, so it's pushed across the environment.

### Stop-gaps to set up before a demo:
1. In the evilpetclinic repo (i.e. in se-cloud-emea https://github.com/se-cloud-emea/evil.petclinic) verify that the pom.xml file has the `jackson-databind` dependency un-commented and with a vulnerable version i.e. 2.10.2
2. Defend > Vulnerabilities > Code repositories: add the evilpetclinic repo (i.e. in se-cloud-emea: https://github.com/se-cloud-emea/evil.petclinic to scan it
3. Defend > Vulnerabilities Images > CI: set up a rule that scopes evilpetclinic (or all) to fail on High vulnerabilities or above.
4. Defend > Compliance > Containers and images > CI: set up a rule that scopes evilpetclinic (or all) to fail on malware.

Above stop gaps will fail these steps in the Jenkins pipeline:
1. Git repo scan
2. Image scan
IaC scan isn't exiting with a failure on purpose, to avoid having too many recurrences, but can definitely be added in the `Jenkinsfile`. We still show the findings in the pipeline step *and* in the DevOps inventory in Prisma Cloud Enterprise (SaaS).
