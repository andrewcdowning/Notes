# Devcon Notes

## Sessions of interest

Devcon broadcast channel https://broadcast.amazon.com/channels/6534

#### Emo 
- One of the best topics of the conference IMO

https://broadcast.amazon.com/videos/615612

#### Making Midway authed services
https://broadcast.amazon.com/videos/611490

#### Testing
https://broadcast.amazon.com/videos/611480

https://broadcast.amazon.com/videos/610135

#### Tech Debt
https://broadcast.amazon.com/videos/611256


#### Career 
https://broadcast.amazon.com/videos/610700

#### Personal stacks for development
https://broadcast.amazon.com/videos/610314

#### New SDE roel guidelines
https://broadcast.amazon.com/videos/610016

#### True Code
https://broadcast.amazon.com/videos/610016



## Tools:

#### CDK - contribute to l2 constructs
  - write rfc and @kneil

#### Canary Islands - Hosted canary services
* Create canary test package using CI VS
* send a sim to the team
* Java only

Full onboarding https://w.amazon.com/bin/view/MADBooksTech/WaldoTechTeam/Projects/Canary_Islands/

#### Barrium - ide support for Brazil Config
* for vs code update to Viceroy 1.48.0
* for other ide - toolbox install barium
* install LSP client for your IDE
* configure client per wiki

https://w.amazon.com/bin/view/Barium/

#### Viceroy is now a fully supported and funded project

https://w.amazon.com/bin/view/VisualStudioCode/Viceroy/

#### Coverlay/Cloudcover
* Cloudcover helps visualize runtime test coverage and set deployment gaurdrails to block untested/undertested code to prod
  * configure line cov thresholds in pipeline approval process
  * Generate coverage reports

   https://w.amazon.com/bin/view/CloudCover
* Coverlay 
  * Important note: Coverlay will begin a new campaign for packages that deploy to prod meet at least 70% coverage
  * [coverlay dashboard](https://us-east-1.quicksight.aws.amazon.com/sn/dashboards/5a3ad958-2ce9-40ce-bd3d-ffa74cdf2399)

  https://w.amazon.com/bin/view/Coverlay/
  
#### Local Serverless Testing Service
Run lambdas and coral lambda endpoints on your local dev machine

Onboarding steps
* write integ tests as you would for hydra
* add CleLocalServer to your Config
* Follow instructions in README.md
* run bb server
* point client to local server