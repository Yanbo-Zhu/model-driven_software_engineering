

# 1 Process

![](image/Pasted%20image%2020241109111450.png)

![](image/Pasted%20image%2020241109111621.png)



Release 就是打包 
Deploy 就是 推送这个包到production system 

# 2 CI CD CDE 三个相互比较 

![](image/Pasted%20image%2020241109112736.png)


![](image/Pasted%20image%2020241109113707.png)


Continuous Delivery (CD) = create a production-ready software artifact automatically on every codechange (to the “master” branch), possibly deploy to ==test/staging environment==

Continuous Deployment (CDE) = automatically deploy the artifact ==into production== on every change

In practice, terms are sometimes used interchangeably è needs a close look

Continuous Delivery and Deployment imply Continuous Integration




# 3 Lean (Building quality)


"Building quality in“ is a principle also found in lean
§ Remove waste
§ Leverage feedback loops
§ Optimize automation
è Build quality into development
§ Non-functional characteristics matter! (more on that later)


# 4 DevOps KPIs

- Deployment frequency – the frequency in which the deployment occurs
- Lead time to changes – the lead time to go from code committed to running in production
- Time to restore service – how long does it take to restore a service after a service incident
    - In other words
    - Meantime to failure recovery – the average time taken to recover from a failure
- Change failure rate – what percentage of changes to production result in degrade service sometimes also: Percentage of failed deployments – the number of times the deployment fails
- Reliability (as in operational performance) – the degree to which a team can keep promises/assertions about the software they operate

![](image/Pasted%20image%2020241109112136.png)


# 5 DevOps with C.A.L.M.S. (Recap)


Culture
Mindset: focus on common goal
Strive to become full-stack engineers

Automation
Automate all the boring stuff
Submit everything into version control

Lean
Don’t “over-engineer”, focus on simplicity
Ensure iterations are fast

Measurement
Real-time monitoring
Meaningful and actionable errors / notifications

Sharing
Ideas, thoughts, experiences
Tools, e.g. for automation and insights




















