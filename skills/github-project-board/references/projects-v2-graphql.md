# GitHub Projects v2 GraphQL Reference

Use these snippets with `gh api graphql`. Replace variables with `-F` or `-f`
arguments; do not paste secrets into commands.

## Prerequisites

```bash
gh auth status
gh repo view --json owner,name --jq '{owner:.owner.login,name:.name}'
```

If GraphQL returns a project-scope error, ask the user to run:

```bash
gh auth refresh -s project
```

## List Active CloudPeek Projects And Status Options

```bash
gh api graphql -f query='
  query($owner:String!) {
    organization(login:$owner) {
      projectsV2(first:20) {
        nodes {
          id
          number
          title
          url
          closed
          fields(first:50) {
            nodes {
              ... on ProjectV2FieldCommon {
                id
                name
                dataType
              }
              ... on ProjectV2SingleSelectField {
                id
                name
                dataType
                options {
                  id
                  name
                }
              }
            }
          }
        }
      }
    }
  }' -F owner='Cloud-Peek'
```

Current active CloudPeek board phases, to be verified live before writes:

| Project | Number | Status phases |
| --- | ---: | --- |
| `CloudPeek` | 1 | `Backlog`, `Ready`, `In progress`, `In review`, `Done` |
| `Bug Fixes` | 8 | `Open`, `Claimed`, `Release Candidate (PR)`, `Closed (merged)`, `Stale (fix not required)` |
| `Permissions V2 Roadmap` | 7 | `Backlog`, `Ready`, `In progress`, `In review`, `Done` |
| `CloudPeek GTM` | 6 | `Backlog`, `Ready`, `In progress`, `In review`, `Done` |
| `Mastec` | 5 | `Todo`, `In Progress`, `Done` |

## Find Issue Project Items

```bash
gh api graphql -f query='
  query($owner:String!, $repo:String!, $number:Int!) {
    repository(owner:$owner, name:$repo) {
      issue(number:$number) {
        id
        number
        title
        url
        state
        labels(first:30) { nodes { name } }
        assignees(first:20) { nodes { login } }
        projectItems(first:20) {
          nodes {
            id
            project { id number title url closed }
          }
        }
      }
    }
  }' -F owner='Cloud-Peek' -F repo='CloudPeek' -F number='123'
```

## Find Pull Request Project Items

```bash
gh api graphql -f query='
  query($owner:String!, $repo:String!, $number:Int!) {
    repository(owner:$owner, name:$repo) {
      pullRequest(number:$number) {
        id
        number
        title
        url
        state
        baseRefName
        headRefName
        labels(first:30) { nodes { name } }
        assignees(first:20) { nodes { login } }
        projectItems(first:20) {
          nodes {
            id
            project { id number title url closed }
          }
        }
      }
    }
  }' -F owner='Cloud-Peek' -F repo='CloudPeek' -F number='123'
```

## Read A Project Item's Current Status

```bash
gh api graphql -f query='
  query($item:ID!) {
    node(id:$item) {
      ... on ProjectV2Item {
        id
        fieldValues(first:30) {
          nodes {
            ... on ProjectV2ItemFieldSingleSelectValue {
              name
              field {
                ... on ProjectV2FieldCommon { id name dataType }
              }
            }
          }
        }
        content {
          ... on Issue { number title url state }
          ... on PullRequest { number title url state }
        }
      }
    }
  }' -F item='PROJECT_ITEM_ID'
```

## Resolve The Status Field For A Project

```bash
gh api graphql -f query='
  query($project:ID!) {
    node(id:$project) {
      ... on ProjectV2 {
        id
        number
        title
        field(name:"Status") {
          ... on ProjectV2SingleSelectField {
            id
            name
            options { id name }
          }
        }
      }
    }
  }' -F project='PROJECT_ID'
```

## Add An Issue Or PR To A Project

Use the issue or PR content `id` from the lookup query.

```bash
gh api graphql -f query='
  mutation($project:ID!, $content:ID!) {
    addProjectV2ItemById(input:{projectId:$project, contentId:$content}) {
      item { id }
    }
  }' -F project='PROJECT_ID' -F content='ISSUE_OR_PR_ID'
```

If the content is already on that project, GitHub may return the existing item
or an error depending on API behavior. Re-query project items before retrying.

## Move A Card To A Status Phase

Use the project ID, project item ID, Status field ID, and matching option ID
discovered from live queries.

```bash
gh api graphql -f query='
  mutation($project:ID!, $item:ID!, $field:ID!, $option:String!) {
    updateProjectV2ItemFieldValue(input:{
      projectId:$project,
      itemId:$item,
      fieldId:$field,
      value:{singleSelectOptionId:$option}
    }) {
      projectV2Item { id }
    }
  }' -F project='PROJECT_ID' -F item='PROJECT_ITEM_ID' -F field='STATUS_FIELD_ID' -F option='OPTION_ID'
```

## Assign Or Comment On An Issue

Use issue/PR commands for repository metadata; these do not update Projects v2
Status by themselves.

```bash
gh issue edit 123 --repo Cloud-Peek/CloudPeek --add-assignee @me
gh issue comment 123 --repo Cloud-Peek/CloudPeek --body 'Claiming this now.'
```

For PRs:

```bash
gh pr edit 123 --repo Cloud-Peek/CloudPeek --add-assignee @me
gh pr comment 123 --repo Cloud-Peek/CloudPeek --body 'Moving this to review.'
```

## Verification Checklist

- Re-query the item field values after the mutation.
- Confirm the target project title and number match the intended board.
- Confirm the displayed status name equals the requested phase.
- Include old status, new status, board URL, and item target URL in the final
  response.
