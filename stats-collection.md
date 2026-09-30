# Preconditions

* Install [`gh`](https://github.com/cli/cli#installation)
* Log in to GitHub:
```sh
gh auth login
```

# Some :cool: Commands

## Org level

* List all veraison repositories:
```sh
gh api /orgs/veraison/repos -q '.[].name'
```

* List only public veraison repositories:
```sh
gh api /orgs/veraison/repos -q '.[] | select( any(.; .visibility == "public") ) | .name'
```

* List veraison repositories with some interesting metadata:
```sh
gh api /orgs/veraison/repos --paginate --template '{{range .}}{{.name}}
  license: {{ if ( and .license .license.spdx_id ) }} {{.license.spdx_id}} {{else}}UNSET{{end}}
  stars: {{.stargazers_count}}
  watchers: {{.watchers_count}}
  visibility: {{.visibility}}
{{end}}'
```

## Repo level

* List contributors for the "services" repo:
```sh
gh api /repos/veraison/services/contributors --jq '[ .[].login ]'
```

* Extract some interesting metadata about the "services" repo:
```sh
gh api /repos/veraison/services --template '{{.full_name}}
  language: {{.language}}
  license: {{.license.spdx_id}}
  forks: {{.forks}}
  watchers: {{.watchers}}
  stars: {{.stargazers_count}}
'
```

* List PRs to the "services" repo that have been merged after a certain date:
```sh
gh pr list -R veraison/services --search "merged:>=2023-06-01" --limit 100
``` 
 
* List issues of the "services" repo that have been closed after a certain date:
```sh
gh issue list -R veraison/services --search "closed:>=2023-06-01" --limit 100
```
