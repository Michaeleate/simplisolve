Following is the data flow.

VSCode Update --> Git --> Git Action (Git Hook) --> Jekins Pipeline --> Update S3.

    Following are the steps.
    1. Create AWS_CREDENTIAL_ID, AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY for AWS Credentials.
    2. Create GIT_CREDENTIAL_ID for GIt credentials.
    3. Check this jenkins file attached.
        Provide the following details.
        BRANCH_NAME: <Branch Name>
        AWS_DEFAULT_REGION: <AWS Default Region>
        S3_URL: <AWS S3 URL>
        REPO_URL: <GIT Repo URL>
    4.  Configs at Jenkins End:
        Build Trigger:
                Enable github hook trigger for GitSCM Polling.
        Go to Jenkins User and click configure.
        Add new API token
        Click Generate and save this for future use.
        Now as admin.
                Administration --> system --> Automation rules.
                create a rule.
                select the trigger changeset..
                save.
    5.  Configs at github side:
        In git repo setting page, click on webhooks
                Payload URL : <Jenkins-url>/github-webhook/
                Secret token : Jenkins API token copied above.
        save the git webhook.

        Git Hub Actions:
            You can configure a GitHub Actions workflow to be triggered when an event occurs in your repository, such as a pull request being opened or an issue being created. Your workflow contains one or more jobs which can run in sequential order or in parallel. Each job will run inside its own virtual machine runner, or inside a container, and has one or more steps that either run a script that you define or run an action, which is a reusable extension that can simplify your workflow.

        Workflows
            A workflow is a configurable automated process that will run one or more jobs. Workflows are defined by a YAML file checked in to your repository and will run when triggered by an event in your repository, or they can be triggered manually, or at a defined schedule.

            Workflows are defined in the .github/workflows directory in a repository, and a repository can have multiple workflows, each of which can perform a different set of tasks. For example, you can have one workflow to build and test pull requests, another workflow to deploy your application every time a release is created, and still another workflow that adds a label every time someone opens a new issue.

            You can reference a workflow within another workflow - reusing workflows.

        Events
            An event is a specific activity in a repository that triggers a workflow run. For example, activity can originate from GitHub when someone creates a pull request, opens an issue, or pushes a commit to a repository. You can also trigger a workflow to run on a schedule, by posting to a REST API, or manually.

        Jobs
            A job is a set of steps in a workflow that is executed on the same runner. Each step is either a shell script that will be executed, or an action that will be run. Steps are executed in order and are dependent on each other. Since each step is executed on the same runner, you can share data from one step to another. For example, you can have a step that builds your application followed by a step that tests the application that was built.

            You can configure a job's dependencies with other jobs; by default, jobs have no dependencies and run in parallel with each other. When a job takes a dependency on another job, it will wait for the dependent job to complete before it can run. For example, you may have multiple build jobs for different architectures that have no dependencies, and a packaging job that is dependent on those jobs. The build jobs will run in parallel, and when they have all completed successfully, the packaging job will run.

            Using Jobs.
        
        Runners
            A runner is a server that runs your workflows when they're triggered. Each runner can run a single job at a time. GitHub provides Ubuntu Linux, Microsoft Windows, and macOS runners to run your workflows; each workflow run executes in a fresh, newly-provisioned virtual machine. GitHub also offers larger runners, which are available in larger configurations.If you need a different operating system or require a specific hardware configuration, you can host your own runners.
        
        How to creae workflows via YAML
            GitHub Actions uses YAML syntax to define the workflow. Each workflow is stored as a separate YAML file in your code repository, in a directory named .github/workflows.You can create an example workflow in your repository that automatically triggers a series of commands whenever code is pushed. In this workflow, GitHub Actions checks out the pushed code, installs the bats testing framework, and runs a basic command to output the bats version: bats -v
                1. In your repository, create the .github/workflows/ directory to store your workflow files.
                2. In the .github/workflows/ directory, create a new file called learn-github-actions.yml and add the following code.
                    name: learn-github-actions
                    run-name: ${{ github.actor }} is learning GitHub Actions
                    on: [push]
                    jobs:
                    check-bats-version:
                        runs-on: ubuntu-latest
                        steps:
                        - uses: actions/checkout@v4
                        - uses: actions/setup-node@v3
                            with:
                            node-version: '14'
                        - run: npm install -g bats
                        - run: bats -v
                3. Commit these changes and push them to your GitHub repository.
                
                new GitHub Actions workflow file is now installed in your repository and will run automatically each time someone pushes a change to the repository.
        
        Understanding the above workflow:
            name: learn-github-actions
                Optional - The name of the workflow as it will appear in the "Actions" tab of the GitHub repository. If this field is omitted, the name of the workflow file will be used instead.
            run-name: ${{ github.actor }} is learning GitHub Actions
                Optional - The name for workflow runs generated from the workflow, which will appear in the list of workflow runs on your repository's "Actions" tab. This example uses an expression with the github context to display the username of the actor that triggered the workflow run.
            on: [push]
                Specifies the trigger for this workflow. This example uses the push event, so a workflow run is triggered every time someone pushes a change to the repository or merges a pull request. This is triggered by a push to every branch;
            jobs:
                Groups together all the jobs that run in the learn-github-actions workflow.
            check-bats-version:
                Defines a job named check-bats-version. The child keys will define properties of the job.
            runs-on: ubuntu-latest
                Configures the job to run on the latest version of an Ubuntu Linux runner. This means that the job will execute on a fresh virtual machine hosted by GitHub.
            steps:
                Groups together all the steps that run in the check-bats-version job. Each item nested under this section is a separate action or shell script.
            - uses: actions/checkout@v4
                The uses keyword specifies that this step will run v4 of the actions/checkout action. This is an action that checks out your repository onto the runner, allowing you to run scripts or other actions against your code (such as build and test tools). You should use the checkout action any time your workflow will use the repository's code.
            - uses: actions/setup-node@v3
                with:
                    node-version: '14'
                This step uses the actions/setup-node@v3 action to install the specified version of the Node.js. (This example uses version 14.) This puts both the node and npm commands in your PATH.
            - run: npm install -g bats
                The run keyword tells the job to execute a command on the runner. In this case, you are using npm to install the bats software testing package.
            - run: bats -v
                Finally, you'll run the bats command with a parameter that outputs the software version.
        
        Visualizing the workflow file
            In this diagram, you can see the workflow file you just created and how the GitHub Actions components are organized in a hierarchy. Each step executes a single action or shell script. Steps 1 and 2 run actions, while steps 3 and 4 run shell scripts. 
        
        Viewing the activity for a workflow run
            When your workflow is triggered, a workflow run is created that executes the workflow. After a workflow run has started, you can see a visualization graph of the run's progress and view each step's activity on GitHub.

            On GitHub.com, navigate to the main page of the repository.
            Under your repository name, click  Actions.
            In the left sidebar, click the workflow you want to see.
            From the list of workflow runs, click the name of the run to see the workflow run summary.
            In the left sidebar or in the visualization graph, click the job you want to see.
            To view the results of a step, click the step.

S3 Deploy.
    https://github.com/actions/checkout/tree/v4
    https://github.com/Reggionick/s3-deploy

    uses: reggionick/s3-deploy@v4
    with:
        folder: build
        bucket: ${{ secrets.S3_BUCKET }}
        bucket-region: us-east-1

    Example.

    name: Example workflow for S3 Deploy
    on: [push]
    jobs:
        run:
            runs-on: ubuntu-latest
            env:
                AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
                AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            steps:
                - uses: actions/checkout@v3

                - name: Install dependencies
                    run: yarn

                - name: Build
                    run: yarn build

                - name: Deploy
                  uses: reggionick/s3-deploy@v4
                  with:
                    folder: build
                    bucket: ${{ secrets.S3_BUCKET }}
                    bucket-region: ${{ secrets.S3_BUCKET_REGION }}
#                    dist-id: ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }}
#                    invalidation: /
                    delete-removed: true    
                    no-cache: true
                    private: true
                    files-to-include: '{.*/**,**}'