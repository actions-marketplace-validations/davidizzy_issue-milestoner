# Issue Milestoner

A GitHub Action that automatically assigns issues to milestones based on specified criteria.

## Features

- ✅ Assigns issues to target milestones
- ✅ Prevents reassignment if issue already has a milestone
- ✅ Optional issue type filtering based on labels
- ✅ Comprehensive logging and error handling
- ✅ Configurable for any repository

## Usage

### Basic Usage

```yaml
- name: Assign Issue to Milestone
  uses: davidizzy/issue-milestoner@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    issue-number: ${{ github.event.issue.number }}
    target-milestone: "v1.0.0"
```

### Advanced Usage with Issue Type Filter

```yaml
- name: Assign Bug to Milestone
  uses: davidizzy/issue-milestoner@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    issue-number: ${{ github.event.issue.number }}
    target-milestone: "v1.0.0"
    issue-type: "bug"
    repository: "owner/repo"
```

### Workflow Example

```yaml
name: Auto Milestone Assignment

on:
  issues:
    types: [opened, labeled]

jobs:
  assign-milestone:
    runs-on: ubuntu-latest
    steps:
      - name: Assign to Current Sprint
        uses: davidizzy/issue-milestoner@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          issue-number: ${{ github.event.issue.number }}
          target-milestone: "Sprint 24"
          issue-type: "enhancement"
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `github-token` | GitHub token with repository access | Yes | - |
| `issue-number` | Issue number to process | Yes | - |
| `target-milestone` | Target milestone to assign to the issue | Yes | - |
| `issue-type` | Optional issue type filter (e.g., bug, feature, enhancement) | No | - |
| `repository` | Repository in the format owner/repo | No | Current repository |

## Outputs

| Output | Description |
|--------|-------------|
| `assigned` | Whether the issue was assigned to the milestone (true/false) |
| `milestone` | The milestone that was assigned |
| `reason` | Reason for the action taken or not taken |

## Behavior

1. **Milestone Check**: If the issue already has a milestone assigned, the action will not reassign it
2. **Type Filtering**: If `issue-type` is provided, the action checks if any issue label matches the specified type
3. **Milestone Matching**: The action finds the target milestone by name (case-insensitive)
4. **Assignment**: Only assigns the milestone if all conditions are met

## Permissions

The GitHub token needs the following permissions:

- `issues: write` - to update issue milestones
- `metadata: read` - to read repository information

## Development

This is a **composite action** using shell scripts for simplicity and maintainability.

### Quick Start

```bash
# Clone and test
git clone https://github.com/your-username/issue-milestoner.git
cd issue-milestoner
./tests/test-composite.sh

# Local testing (requires GitHub CLI and token)
export GH_TOKEN=your_token
./test-local.sh
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed development guidelines.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Contributing & Support

- **Contributing**: See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines
- **Issues**: [Create an issue](https://github.com/davidizzy/issue-milestoner/issues/new) for bugs or feature requests
- **Discussions**: Use GitHub Discussions for questions and help
