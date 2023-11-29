Following is the data flow.

VSCode Update --> Git --> Git Action (Git Hook) --> Jekins Pipeline --> Update S3.

Git Repo Setting:
Payload URL: Jekins Build URL as I am using local host... its not possible to give local payload url... as the data cant be transfered from internet... 
    For that I am using socketxp.com 
    Step 1: Created account in socketxp.com and got authtoken.
    Step 2: Install socketxp on same host.
            https://www.socketxp.com/download/
    step 3: socketxp login eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjI2NTMxNTEzMzgsImtleSI6IjBiNDBjODIwLTAzNTQtNGE1Zi1hZTEzLTFjNTBkMzhiNmYwNSJ9.o4t2NMO5WVZljRLAZF5DdKMW8pgARYDGUo5QEZUyfmE
    step 4: socketxp connect https://localhost:8080

                Connected to SocketXP Cloud Gateway.
                Public URL -> https://michaelraj-eate-52fdbc43-8693-4224-b2e5-6b7e3f6f9ae2.socketxp.com
    Step 5: create webhook in git repo settings.
            Payload url: https://michaelraj-eate-52fdbc43-8693-4224-b2e5-6b7e3f6f9ae2.socketxp.com/git-webhook
            content type:  application/json
    Step 6: Create Jenkins File.
    
    This is it.

    Set the Jenkins API token as the GitHub webhook secret token
    jenkins api token - 1186c14e398da9152c7480fd673ced1931



    You're using 'Known hosts file' strategy to verify ssh host keys, but your known_hosts file does not exist, please go to 'Manage Jenkins' -> 'Security' -> 'Git Host Key Verification Configuration' and configure host key verification.


    Following are the steps.
    1. Create AWS_CREDENTIAL_ID for AWS Credentials.
    2. Create GIT_CREDENTIAL_ID for GIt credentials.
    3. Check this jenkins file.