## Site reliability engineering

### SLI, SLO, SLA

- Service Level Indicator (SLI)
  - A direct measurement for the system, e.g. what percentile of requests are over 300ms
- Service Level Objective (SLO)
  - A precise numerical target for system availability based on SLIs
- Service Level Agreement (SLA)
  - An agreement between parties about the required level of SLOs and consequences, if they are not met

### Risk and error budget

- Risk and error budgets are set of limits, when met, determine when a development team should focus on reliability instead of shipping new features

### Toil and toil budget
- Toil means manual action you have to perform upon failure
- Toil budget means the time spent on the manual things
- The amount of toil should be lowered by automating things when it's reasonable

### Risk analysis

- As an example
  1. A database backup has to be taken every month and it takes 120 minutes, 1440 minutes in total per year
  2. The service is not available for users during that time
  3. If our error budget is 99.5% i.e 2628 minutes per year, this backup consumes over half of it
  4. When combined with other errors, the budget might be exceeded
  5. By analysing all these risks, we can determine, which one of them we could tackle to achieve our SLAs

### Observability of distributed systems

- Observability consists of:
  - Structured logging
    - E.g. application logs, access logs etc.
  - Metrics
    - E.g. counters (rate), distribution (heat map)
  - Traces
    - Individual execution flows to be traced including timing and dependencies

### Incident management

- Process for declaring incidents
- Dashboard for viewing current incidents
- Database of who to contact for each kind of incident
