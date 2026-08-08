https://www.youtube.com/watch?v=GQQqf-C2ha4

Resuming : 

Use the "basic" GIT Workflow, the one with different branches for each release, only when working with software that needs to be installed like "photoshop" or others.
This method is not good for CI/CD

### Github workflow
Very similar to what we did at LeCab: There is a main branch that is always deploy-able, and that is the basic and only rule ! You can have as many branches as you want, that can be deleted, but what is important is, you merge to "main" only when the code is ready for production

## Trunk workflow
Smaller commits with feature flags: The team develop a new feature, you may need more than one people to develop this feature, each dev add the new feature with a flag in the code. 
When all the devs have finished the work, they can activate feature with the flag!
This need rock solid testing before sending the code.

