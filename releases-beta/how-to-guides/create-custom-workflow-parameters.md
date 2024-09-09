# Create Custom Workflow Parameters

Custom workflow parameters allow you to define and manage key-value pairs that can be accessed during your software build and deployment process. This ensures that your release pipeline can use specific configurations for different environments, such as staging, production, or custom setups.

## Generate Workflow Parameters in Your Project or Environment Config

1. Go to the Project Config in your Aviator release project.
2. In the build section, fetch and select your workflow.
![](../../.gitbook/assets/release-build.png)
3. In the "Additional workflow parameters", click "ADD" button to define a new workflow parameter.
4. Enter the key (name of the variable) and the value. Instead of a custom value, you can also select a pre-defined value from the dropdown.
   - **Key**: This should be a unique identifier for the variable (e.g., user).
   - **Value**: This is the specific configuration or secret (e.g., Aviator).
5. After adding all necessary variables, click Save to store the configurations.
![](../../.gitbook/assets/release-custom-env.png)
6. If you'd like to create custom workflow parameters for a specific environment, navigate to the Environment Config for that environment. In the "Additional workflow parameters" section, add the parameters in the same way as described above.
![](../../.gitbook/assets/release-env-config.png)

## Add the Custom Workflow Parameters to your Pipeline

1. Go to your CI/CD workflow (GitHub Actions in this case).
2. Add the variables as inputs on workflow dispatch.
```yaml
name: build
on:
  workflow_dispatch:
    inputs:
      user:
        description: "Username"
        required: false
        type: string
      commit_hash:
        description: "Commit hash"
        required: false
        type: string
      env:
        description: "Environment tier"
        required: false
        type: string
      env_name:
        description: "Environment name"
        required: false
        type: string
```
3. Create a job which prints these inputs.
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Print the inputs
        run: |
          echo "Inputs:"
          echo "user=${{ inputs.user }}"
          echo "commit_hash=${{ inputs.commit_hash }}"
          echo "env=${{ inputs.env }}"
          echo "env_name=${{ inputs.env_name }}"
```
4. On the release dashboard, cut a release to trigger the workflow.
![](../../.gitbook/assets/release-cut-custom-env.png.png)
5. Verify the logs of the triggered pipeline to get the value of the custom workflow parameters in the workflow.
![](../../.gitbook/assets/release-workflow-logs.png)

