# actions-pipelines
GitHub actions, Azure Pipelines


```
- name: Commit files
  id: commit
  run: |
    git config --local user.email "action@github.com"
    git config --local user.name "github-actions"
    git add --all
    if [-z "$(git status --porcelain)"]; then
       echo "::set-output name=push::false"
    else
       git commit -m "Add changes" -a
       echo "::set-output name=push::true"
    fi
  shell: bash
- name: Push changes
  if: steps.commit.outputs.push == 'true'
  uses: ad-m/github-push-action@master
  with:
     github_token: ${{ secrets.GITHUB_TOKEN }}
```

Using GITHUB_TOKEN to authorize the push prevents such loops implicitly:

When you use the repository's GITHUB_TOKEN to perform tasks, events triggered by the GITHUB_TOKEN, with the exception of workflow_dispatch and repository_dispatch, will not create a new workflow run.

https://github.com/marketplace/actions/write-file
