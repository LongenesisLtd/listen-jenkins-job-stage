# Listen Jenkins Job Stage - A GitHub Action

The [Listen Jenkins Job Stage](https://github.com/LongenesisLtd/listen-jenkins-job-stage) is a [composite run step custom action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-run-steps-action) that can be used to listen to Jenkins Job stage, wait for it to complete, and return the results.  Please make sure to read this README completely, as there are some caveats to using this action.

This action does not require but is made to work in synergy with [Trigger Jenkins Job](https://github.com/LongenesisLtd/trigger-jenkins-job)

## Caveats

The following are known issues with this action:

- The action only works on Linux runners
- The Linux runners must have `curl` & `jq` installed
- Currently the action only works with jobs with no parameters. Parameters will be available in a future 
- Jenkins must have [Pipeline: REST API Plugin](https://plugins.jenkins.io/pipeline-rest-api/) installed

## Example Workflow

```
jobs:
  trigger-a-jenkins-job:
    runs-on: ubuntu-latest
    steps:
      - id: triggerjenkinsjob
        uses: LongenesisLtd/trigger-jenkins-pipeline@v1
        with:
          build-url: "${{ steps.trigger_jenkins.outputs.build-url }}" # full URL of the jenkins job build
          jenkins-url: "${{ secrets.JENKINS_URL }}" # URL of the jenkins server
          stage-name: "build" # The name of the jenkins stage to listen
          jenkins-basic-auth: "${{ secrets.JENKINS_TOKEN }}" # basic auth for jenkins
          poll-time: 10 # how often to poll the jenkins server for results
          verbose: true # true/false - turns on extra logging
```

## Input Variables

Variable Name | Description
------------- | -----------
**jenkins-url** | Text string the URL to your Jenkins server. Example: http://myjenkins.acme.com:8080. It is suggested you put this value in a secret.
**build-url** | full URL of the jenkins job build
**stage-name** | The name of the jenkins stage to listen
**jenkins-basic-auth** | basic auth for jenkins, can use token or password for second part. Store this in a secret for security
**poll-time** | Time, in seconds, of how often the action should poll the Jenkins job to see if it has completed
**verbose:** | true/false. This value, when true, enables extra logging in the build output. By default it is false, meaning minimal logging