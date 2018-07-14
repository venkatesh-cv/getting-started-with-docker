# Microservices Architecture - Aligning principles practices and culture
By Irakli Nadareishvili, Ronnie Mitra, Matt McLarty &  Mike Amundsen

## The Microservices way
- Traditionally it has come to be accepted that in software, Safety, reliability and resiliency almost seens inversely proportional to pace of feature development and releases. Almost seems impossible at scale
- Seems Microservices is a way to break that inverse proportionality and also at scale.

### Birth of Microservices
- Came from a meeting of architects who discovered that there is a pattern/theme to a way of software development being adopted by a set of modern companies
- came out of a need to break down the challenge of building large complex systems into smaller manageable, replaceable ones.
- It also came about that large systems encounter a different set of problems at scale / hyper-scale
- Microservices is about building a large complex system out of building blocks of smaller, autonomous, manageable, scalable and replaceable systems rather than finding that one pattern which can infinitely scale.

- **Definition** A microservice is an *independently deployable* component of *bounded scope* that supports *interoperability through message-based communication*. Microservices Architecture is a system of engineering highly automated , evolvable software software systems made up of capability-aligned microservices.

### Characterestics of Microservices
- smaller
- messaging enabled
- bounded by contexts
- autonomously developed
- independently deployable
- decentralized
- built and released via automated processes- 


### decentralization phobia and other larger questions
- who is in charge?
- autonomy - trust each team to make the right decisions for their scope
- Advantage - faster rollouts, lesser bottlenecks
- How do we hedge reputational risk against autonomy / balancing microservices' free-style against a risk-averse enterprise.
- who identifies the microservices, how are they governed?
#### truth is
- with microservices governance is easier but architecture more complex.
- this complexity is managed via tools and automation.


### Soul of microservices
- speed and Safety
- speed not for speed's sake  but a business driver.
- Agility
- composability
- comprehensibility
- deployability
- organizational alignment - smaller, focused teams
- polyglotism - right tools for the job

### Layers of Microservices
#### Modularity
##### Monolith first
There is a belief that microservices can be derived only from a large monolith that will help define / learn the bounded contexts
So is it possible that microservices can be born out of only a monolith? is it possible to start building a microsrevice without a monolith first approach? The belief stated seems to stem from Gall's law.

##### Gall's law
- a complex system that works is invariably found to have evolved from a simple system that worked.

#### Cohesiveness
- modular components need to be cohesive to be cohesive to form a system
- *Cohesiveness* - the quality of forming a united whole (defintiion)
- the modular components should provide complementary set of services that complete a system's Characterestics

#### System elements
- This is the last and important layer of a microservice
- The ability to understand inter-dependencies
- the ability to understand the impact of changes on the whole system can be analyzed quickly.

### Microservices maturity model
- ![model](https://d3ansictanv2wj.cloudfront.net/msar_0201-a762ef3efa52bf559517ac80e291c64a.png)

## chapter 3 - designing microservice systems
- The key to building microservices is to define well and understand the behavior and Characterestics of the overall system. 
- In other words, the sum of all parts.
- Systems exhibit what is called an *Emergent Behavior* which states that the sum of all parts is larger than the sum itself.
- The other nature of emergent behavior is that its difficul to predict change in behavior from an impact to a component.
