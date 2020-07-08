# Deploy an app to your own cloud.gov sandbox
## Manually deploy an app to your cloud.gov sandbox
1. Fork a copy of FEC demo-apps into your personal github by clicking the "Fork" button in the top right hand corner

2. Choose your own repo to fork to

3. Clone the forked copy to your local:

`git clone git@github.com:[github_handle]/demo-apps.git`

4. Target the sandbox and your space name:

`cf target -o [sandbox_org_name] -s [space_name]`

5. Go into the directory where the app was cloned:

`cd demo-apps/`

6. Push your app up to the sandbox:

`cf push [app_name]`

7. Check to see if your app has started successfully. You should see the assigned urls to be able to check on your app in the browser:

`cf apps`


## Integrate CI/CD into your app deployment
You can deploy your application using a CI/CD tool such as CircleCI. 
### Set up a deployer in your sandbox space
1. Target the sandbox and your space name:
`cf target -o [sandbox_org_name] -s [space_name]`

2. Create the Cloud.gov deployer service:

`cf create-service cloud-gov-service-account space-deployer [service_name]`

3. Create the deployer service key:

`cf create-service-key [service_name] [service_name_key]`

4. Grab the keys:

`cf service-key [service_name] [service_name_key]`

### Set up CircleCI environment variables for deployment
1. Go to your circle project dashboard: https://app.circleci.com/projects/project-dashboard/github/[your-github-handle]

2. Click on the “Set up project” button next to your app’s repo you wish to deploy using this tool

3. Select the “Add manually” option, since we already have a .circleci/config.yml file in our forked app repo.

4. Add the necessary environment vars into your CircleCI project settings by going to your project settings
https://app.circleci.com/pipelines/github/[github_handle]/demo-app. You can get the username and password variables by grabing the deployer keys

Variables include the following:
```
FEC_CF_ORG
FEC_CF_SPACE
FEC_CF_USERNAME_DEMO
FEC_CF_PASSWORD_DEMO
```
5. Go ahead and run your CircleCI build. Within the deployment process, we also incorporate autopilot, which is a zero down time deployment process.

6. After a successful deploy, you should be able to see in the circle logs the url in which the app was pushed.

## Customizing app configurations
You can customize app configurations by modifying the manifest.yml file. This includes customizing things like app name, route, buildpacks, and memory.
