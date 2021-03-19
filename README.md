You can clone this repo and put it in the demo-build gogs server, so that can in turn be used in the demo-build Jenkins server with all pre-existing vars and passwors.
You can also point the demo-build server to this repo, once it becomes public, or to your fork, instead of copying to the gogs server.

Until this is committed to the demo-build source code and then its own Gogs and Jenkins servers, you'll need to clone this repo's content into your demo-build gogs server and then add a new pipeline in your Jenkins server based on that.

The Git scan is performed against https://github.com/se-cloud-emea/evil.petclinic so you'll need to add that into your Twistlock Console as a Git repo to scan, including a token: https://docs.twistlock.com/docs/compute_edition/vulnerability_management/code_repo_scanning.html

Link to running this demo: https://drive.google.com/file/d/1fFPoteVm61hqeQOb40Uzhdxw-DKDkLtG/view

PC_USER,PC_PASS and PC_CONSOLE are pre populated from the yaml file from the central demo build. 
