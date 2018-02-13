# Overview
This is a model for describing architecture of software systems. <br/>
This uses multiple views to represent the architectural solution to a set of use-cases or scenarios<br/>
These views are targeted at various stakeholders to address their primary concerns<br/>

    - End user
    - Developers
    - Systems Engineers
    - Project Managers 
These views also factor in the solution for both functional and non-functional requirements<br/>

As stated above, these views are designed with the main aim of communicating the architecture solution to address specific scenarios and is typically evolved in an iterative manner.

# The need
Communicating software architecture is difficult. As Martin Fowler puts it, 
> Software architecture is a collective understanding of the structure/form and behavior/function of a system. - Martin Fowler

The collective here is the team.
## Perry and Wolf's equation on architecture
Perry and wolf's equation on architecture states that  
> architecture = (Elements, Form, Rationale/Constraints)

In other words, architecture is a process of assembling select architectural elements in a particular well defined form or shape to meet various functional and non-functional requirements. 

# Architecture Model
Architecture document aims to enhance and influence the collective to arrive at a common understanding of the form and function of the proposed or prevailing system. <br/>
Without getting into the fundamentals of what makes up a software architecture we can agree that a typical software architecture tries to present it in the form of abstract views, system broken down into its constituent parts, a view that depicts inter-dependencies, a view that shows physical locations and connectivities in the physical world and so on. 
In order to target maximum stakeholders, we will be using five views.
- **Logical view** This is an object oriented view of the design.
- **process view** this shows the behavioral view including the concurrency and synchronization aspects
- **physical view** this shows the software deployment view from an infrastructure perspective.
- **Development View** this shows the software organization in development environment.
-**scenario view** the four views can be illustrated using a few scenarios or use cases. This is the fifth view.
<br/>
The four views are evaluated against Perry and Wolf's equation independently. 
To elaborate, each view will feature its *Elements* that are fitted together in a particular *form* following certain established patterns factoring in the constratints along with due rationale. 

### Notations and Styles
This is an opern approach and any suitable notation and style can be adopted especially for process and logical views

### Logical Architecture
This typically is a domain model represented as class diagrams that elucidate the entities and relationships in the problem domain.
This view helps with identifying categories. See samples below
<br/> Images courtesy - [site](http://www.cs.ubc.ca/~gregor/teaching/papers/4+1view-architecture.pdf)
![Figure - 1 Notations](four_plus_one_images/Figure-2-Notation_for_logical_blueprint.PNG)

![Figure - 2 Sample](four_plus_one_images/Figure-3-a-Logical-blueprint-for-the-Télic-PABX.png)

### The process architecture