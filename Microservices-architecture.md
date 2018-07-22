
<!-- TOC -->

- [Microservices Architecture - Aligning principles practices and culture](#microservices-architecture---aligning-principles-practices-and-culture)
    - [The Microservices way](#the-microservices-way)
        - [Birth of Microservices](#birth-of-microservices)
        - [Characterestics of Microservices](#characterestics-of-microservices)
        - [decentralization phobia and other larger questions](#decentralization-phobia-and-other-larger-questions)
            - [truth is](#truth-is)
        - [Soul of microservices](#soul-of-microservices)
        - [Layers of Microservices](#layers-of-microservices)
            - [Modularity](#modularity)
                - [Monolith first](#monolith-first)
                - [Gall's law](#galls-law)
            - [Cohesiveness](#cohesiveness)
            - [System elements](#system-elements)
        - [Microservices maturity model](#microservices-maturity-model)
    - [chapter 3 - designing microservice systems](#chapter-3---designing-microservice-systems)
        - [Standardization and coordination](#standardization-and-coordination)
        - [microservice design process](#microservice-design-process)
            - [Define optimization goals](#define-optimization-goals)
            - [Development principles](#development-principles)
            - [Sketch](#sketch)
            - [Implement, observe and adjust](#implement-observe-and-adjust)
    - [Chapter 4 - Establishing a Foundation](#chapter-4---establishing-a-foundation)
        - [Goals for Microservices](#goals-for-microservices)
            - [Redcuce costs](#redcuce-costs)
            - [Increase speed](#increase-speed)
            - [Resilience](#resilience)
            - [Visibility](#visibility)
            - [the right balance](#the-right-balance)
                - [Immutability](#immutability)
                - [Separation of concerns](#separation-of-concerns)
            - [Unix's rules and operating principles](#unixs-rules-and-operating-principles)
            - [Reference principles](#reference-principles)

<!-- /TOC -->
# Microservices Architecture - Aligning principles practices and culture
By Irakli Nadareishvili, Ronnie Mitra, Matt McLarty &  Mike Amundsen

## The Microservices way

- Traditionally it has come to be accepted that in software, Safety, reliability and resiliency almost seems inversely proportional to pace of feature development and releases. Almost seems impossible at scale
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
So is it possible that microservices can be born out of only a monolith? Is it possible to start building a microsrevice without a monolith first approach? The belief stated seems to stem from Gall's law.

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

### Standardization and coordination

- Standardization influences repeatability and therefore evolutionary improvements
- this promotes Agility
- this does not mean an organizational level of Standardization but more at a team level.
- Standardization needs to be achieved for objectives/outputs, process and tools.
- objective Standardization
  - microservices have a standard objective - to build APIs
  - APIs need to have a uniform / standard (ReST or such)
  - Should emit events, subscribe to events
- Standardization of skills
  - a minimum standard skill level of a team member is paramount to success of the team.
  - standardizing talent is an effective way of introducing more autonomy in the team.
  - Such a team will be able to independently make decisions that align with overall objectives with little governance
- Tool Standardization
  - interface Standardization, process Standardization all have a consequence on tools
  - the automatically limit the autonomy of tool selection
  - this by itself can have unintended consequences on adaptability of the system.
  - But this is necessary for defining the boundaries of automony of the team and also shape the overall emergent system

### microservice design process

- They key to a good design is the process itself
- a good process allows a designer to shape the design over time to contiuously get closer to the best desired product.
- The key is making the right assumptions, getting the right advice and keeping an open mind about impact of changes.
- The design framework comprises of the following key steps
  - Identify optimization goals
  - Develop principles
  - Sketch and iterate
  - Implemnt, observe and adjust

#### Define optimization goals

- Set the goals that defines the success of the services
- The goals may evolve over time but at any time, it should reflect what is just needed. remember YAGNI
- This is the single most crucial activity that will impact all design choices.
- The objective is to define the goals that define the optimal system behavior
- The other important thing is to keep the list of goals themselves short. One should beware of the tendency to come up with a long list.
- This does not mean foregoing all goals other than those in the shot list. It rather means that the design choices will be influenced/swung by those goals in the short list. For instance, if a reliable system is desired, it does not mean foregoing performance or security. But rather it means all key deicisions will prioritize reliability over performance.
- Also as stated earlier, one must accept that goals will shift. So it is natural if in future performance takes the top spot. This does not mean that everything gets reversed. But rather it defines the focus needed and the decision tweaks/realignments needed to build on top of the now reliable system to make it more performant.

#### Development principles

- comprises of general policies, constraints and ideals that shall be univerally applied to the actors within the systm to guide decision making and behavior when they exercise their autonomy in decision-making.

#### Sketch

- Sketch the important parts of your system, review and iterate.
- This helps assess decisions taken at a-point-in-time given a set of considerations and information.
- This should cover
  - The organizational structure
  - solution architecture
  - Service design (bounded contexts)
  - processes and tools
- also important is that the above sketches should follow the principles laid down.

#### Implement, observe and adjust

- The key to measurement is identifying the right set of KPIs that will provide insight into system behavior.
- Without this it is likely that decisions will be based on assumptions rather than facts and could lead to wrong decisions that can lead to mounting tech. debt.
- On the other hand the risk of not having insight is trying to overengineer a system that is too complex to build and maintain for the given goals. For instance a system that is designed to handle a change that may never happen.
- At the same time it is also important that the cost of making small changes is cheap and is often sufficient to course correct .
- The emergent design should be larger than the changes made.

## Chapter 4 - Establishing a Foundation

### Goals for Microservices

- The overarching goal of any microservice is to balance speed and Safety at scale

#### Redcuce costs

- It is important to build-fast and fail-fast to keep costs low
- avoid long drawn experiments
- short-cycles help quickly assess business value.
- Leverage virtual infrastructure and containers to optimize costs on hardware and tooling.

#### Increase speed

- continuing the same philosophy of build-fast and fail-fast it is important to keep release cycles super short.
- leverage automation wherever possible such as CI/CD, Container orchestration etc.,
- Some companies have automated to such a degree that they can hit production several times (even 100) a day.

#### Resilience

- This is the first element of Safety
- systems that can withstand failures
- In other words systems that dont crash
- design for resiliency
- This does not mean designing a bug-free system (a chimera) but rather systems that  can handle failures gracefully.
- Continuous testing is a key aspect of ensuring resiliency. Automated tests at all levels (code, infrastrcture, process) is important to assess a systems response to failures or unexpected behavior. 
- This is also called an *end-to-end* testing.
- Consider a blue-green approach to deployment where a users are gradually onboarded to a new deployment with close monitoring.
- Netflix uses chaos-monkey to test their resilience to failure.

#### Visibility

- Stakeholder visibility is very healthy. Work to improve it
- code quality, coverage, other smells
- tech. debt.
- bugs
- backlog
- build metrics
- Runtime behavior such as performance of APIs
- error reports
- alerting
- utilization such as CPU, Memory, network
- infrastrcture # of servers, scaling patterns

#### the right balance

- It is not always possible to prioritize just one of the above
- it is important that a right balance is struck between optimizing costs, improving speed of development, resiliency and transparency.
- The key to this balance is the goal definition that will help with prioritization

#### Principles

- Principles define the guidelines for meeting the goals
- They offer concrete guidance to achieving the stated goals through examples
- It is important to note that the principles are not rules but more of guidelines or recipes

##### Immutability

- Netflix focuses on building immutable systems
- systems that are stateless and therefore horizontall scalable
- The chaos-monkey tool regularly removes instances from prouction to test immutability and resiliency of their systems
- additionally a red-black deployment is done to ensure immutability. This is another name to the blue-green approach mentioned earlier.

##### Separation of concerns

- in accordance with Conway's law a team is responsible for building, deploying, maintaining and evolving a set of micro-services.
- They also will present a stable agreed contractual interface, SLAs to consumers for those services.
- Microservice should also build only relevant services

#### Unix's rules and operating principles

- Make each program do one thing well
- build and test early
- dont hesitate to rebuild clunk components
- use tools

#### Reference principles

- **do one thing well** - this can vary for services. Depending on the bounded contexts.
- **build afresh - avoid** repurposing old components to be microservices.
- **expect output to become input** - always expect that the output of your system is an input to hitherto unknown system.
- **dont rely on interactive input** - reduce / avoid dependency on human interactions to provide inputs.
- **try early** - build-fast fail-fast
- **dont hesitate to throw it away** - dont hesitate to retire old components that have outlived their use. Rebuild clunky components.
- **Toolmaking - Build tools**, focus on productivity,speed and automation. Build your ownt tools as necessary.

### Platforms

- Platforms are a good way of realizing many microservice best practices and goals.
  - For example Docker has made it very easy to realize immutability, automation and deployability
  - Microservices platforms are a great way of bootstrapping one's Microservice journey.
  - Many companies are doing it and even OOSing their work.
- In general microservice platforms provides services that all microservices need to deal with. They can be classified into
  - Shared concerns
  - local concerns

#### Shared Services / Concerns

- Areas such as infrastructure, Container tech., database technologies etc.,
- These are services all microservices would need and use.
- The case for sharing components such as database can be argued based on various considerations that are important to each team. Some may favour sharing components or technologies for cost optimization and safety of changes. Whereas others may focus on speed to market more than changes.
- The following are typically share capabilities
  - hardware (procurement, provisioning, baselining, IaaS teams)
  - source control and Ci/CD pipeline
  - Data stores
  - service orchestration
  - UIAA
  - Architecture policy - standard patterns to adopt, availability guidelines, process for DR & HA

#### Local capabilities

- Maintained by the team or platform services customized by the team to the team's needs.
- Automating tooling built by the team for local needs
- Feature gating
- Service discovery
- Business frameworks / routing requests / handling requests
- Monitoring and alerting

### Culture

- 