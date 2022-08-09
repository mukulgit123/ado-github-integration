# Azure DevOps Github Integration

Automatically links a GitHub PR to Azure DevOps Work Item and/or set a custom state for it.\
Please read the annotations for the yaml file below!

## Example Usage

```yaml
name: "Update DevOps Work Item and PR Description"
on:
  pull_request:
    types: [opened, ready_for_review]

jobs:
  update-devops-workitem:
    runs-on: ubuntu-latest
    steps:
      - name: "Update DevOps Work Item"
        uses: mukulgit123/ado-github-integration@v1
        with:
          repo-token: ${{ secrets.YOUR_GITHUB_TOKEN }} # This will need to be stored in Github secrets
          devops-work-item-regex: "[0-9]+" # Regex which gets applied to title, body and branch name
                                                             # (in this order) to find the DevOps Work Item Id (only 
                                                             # first match gets used)
                                                             # The action automatically converts it to /[0-9]+/ or the
                                                             # desired regex used by match function
          set-to-state: "" # The state you want the work item to be set to (exact string match) e.g. In progress Keep empty to skip.
          dont-set-state-while-prs-open: false # Set to true if you don't want to set the state while GitHub PRs 
                                               # associated with the DevOps Work Item are still open
          add-pr-link: true # Wheather you want to add the PR to DevOps (requires GitHub Integration into DevOps and 
                            # the repo to be actively linked!)
          devops-organization: "org" # The url-slug of the devops organization
          devops-pat: ${{ secrets.DEVOPS_PAT }} # The Personal-Access-Token (PAT) to authorize the action to  
                                                #  communicate with DevOps. As of now the PAT needs the following 
                                                # rights: 
                                                # - "Work Items - Read, write, & manage" to move the work item between
                                                #    states 
                                                # - "Full Access" to link the PR to the work item (currently there is 
                                                #    no specific right to only allow this, it only works with full 
                                                #    access. Be careful who you give this token!)
                                                # This will need to be stored in Github Secrets
          fail-on-error: true # If you don't want the action to fail (and create failed checks) on error (e.g. when 
                              # the work item id couldn't be found via the regex or an unforseen error occurs) set
                              # this to false. Setting this to false will also allow partial completion (e.g. only 
                              # link pr but not move the state)
          
```

## Build
```shell
npm run build && npm run package
```
