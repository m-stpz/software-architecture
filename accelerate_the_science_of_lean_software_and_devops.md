# Accelerate: The science of lean software and devops

4 metrics for high-performing teams:

1. Lead time for changes: time from commit to production
2. Deployment frequency: how often you deploy to production
3. Change failure rate: % of deployments causing failure
4. Mean time to restore (MTTR): how quickly your recover from failures

## Core principles

### Continuous delivery

- Small batches, trunk-based development, automated testing and deployments
- Teams that deploy frequently with automation have lower failure rates
  - speed and stability reinforce each other

### Architecture matters

- Loosely couple architectures let teams deploy independently without coordination
- Teams should be able to test, deploy, and change their systems without depending on other teams

### Lean management

- Limit work in progress [WIP]
- Use lightweight change approvals instead of heavy-weight review boards
- Make work visibile and use monitoring/alerting for feedback

### Culture: Westrum model

- Generative cultures, where failure leads to inquiry, not blame
- Psychological safety enables learning and experimentation

### Technical practices

- Version control everything: including infrastructure
- Automate testing and deployment pipelines
- Shift security left: integrate it early, not as a gate at the end
- Use trunk-based development over long-lived feature branches
