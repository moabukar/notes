# Disaster Recovery

## How to acheive DR?

- There are mutliple ways to achieve DR
  - Active-Active is NOT the only solution

- RPO/RTO are the key metrics to measure DR
- RPO: Recovery Point Objective
  - Amount of data you can afford to lose
  - How much data can you afford to lose?
  - If you have an RPO of 1 hour, you can lose 1 hour of data
- RTO: Recovery Time Objective
  - Amount of time you can afford to be down
  - How much time can you afford to be down?
  - If you have an RTO of 1 hour, you can be down for 1 hour

- You can reduce RPO:
  - By having more frequent backups
  - Real time RPO: real time replication. DB needs to be replicated at another DR region.
- You can reduce RTO:
  - By having a more robust DR strategy


## DR Strategies


