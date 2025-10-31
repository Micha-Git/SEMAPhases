# Use Cases

Use cases specify the functional requirements that indicate what the system does in a way that defines a contract between the user and the system where the system is considered to be a black box. First, a UML use case diagram is created to provide an overview of the use cases, actors and their relationships. This diagram is by no means a sufficient specification of the use cases but it serves as a context diagram of the system and its environment. Each use case depicted in the use case diagram is further specified textually in a structured format given in the use case template. Best practices for writing use cases are given in [recommendations and hints writing use cases](#Recommendations-and-hints-for-writing-use-cases).


---
## Use Case `<Title>`

```
Title: <Goal of the use case as a short active verb phrase>

Primary Actors: <Comma-separated list of primary actors>
Secondary Actors: <Comma-separated list of secondary actors>

Preconditions: 
    - <Declarative description of the precondition>  
    - ...
Postconditions:
    - <Declarative description of the postcondition> 
    - ... 
    
Flow:
1. <Actor> <verb> <direct object> <prepositional phrase>  
2. ... 

Alternative flows:
1a. <Declarative description of the condition leading to branching>:
    1a1. <Alternative flow step>
    1a2. ... 

Information Requirements: 
    <Secondary Actor>:
    - <Information required by the system from the secondary actor>: <optional: remarks and details> 
    - ... : ... 
```

---

## Recommendations and Hints for Writing Use Cases

### General

- All (primary and secondary) actors and all relevant domain objects must be defined in the analysis artifact "Terms Used".  
- The system must be considered as a blackbox while defining use cases and actions. Internal actions of the system must not be defined
- Define only user goals, not strategic or subsystem goals.
- Write active and simple sentences.
- Use the terms systematically to avoid ambiguity.  

### Title
- Should summarize the user goal
- Writing style:
  - Active, short phrase: ```<verb> <direct object>``` 
  - Present tense
  - Title case 
  - E.g. "View Fleet Overview"

### Primary/Secondary Actors
- Primary actor is an actor whose goal the use case is trying to satisfy, in other words, the actor whose action initiates the use case. A use case may have more than one primary actors if the use case is identical for the both actors. Otherwise, the use case should be defined separately. If more than one primary actors are given, no alternative flow should be defined based on this. 
- Secondary actors can be other human or institutional actors that assist in the execution of the use case in a way or they can be non-human actors such as external systems like APIs that provide data to the system. 
  
### Preconditions/Postconditions
- A precondition is the state of the system and its surroundings that is required before the use case can be started. 
  - These conditions are already known to be true and therefore will not get revalidated or tested inside the use case
  - Write only preconditions that the system can guarantee to be true
  - The use case cannot start unless all preconditions are true
- A postcondition is the state the world after the use case has ended
  - If the use case only shows information to the actor and does not change the state of the system in any way, there are no postconditions  
- Writing style:
  - Should be simple assertions formulated declaratively 
  - Present tense
  - May be written passively (we are only interested in the condition itself)
  - Use placeholder None for empty pre- or postconditions
  - E.g. "system has a fleet registered for the actor" (precondition), "the car with the given VIN is added to the fleet" (postcondition)

### Flow 
- The necessary actions to carry out the use case under normal circumstances 
- User interface specifics such as navigation steps should be omitted
  - Focus on actor's intent instead 
- Avoid actions that don't move the process forward
- Writing style:
  - Always active sentences to make subjects and objects clear
  - Reference primary actor as "actor" instead of its actual name
  - The system under design is an implicit actor in all use cases and referenced as "system"
  - Sentence structure: ```Actor <verb> <direct object> <prepositional phrase>```
  - No period at the end of the sentence 
  - If there is data being passed in an action, describe it as a comma-separated list
    - If the comma-separated list is too long, use a dashed list instead  
  - If the action describes a validation:
    - Avoid verbs like "checks" that lead to if-else-then statements 
    - Use verbs like "verifies", "validates", "ensures", "establishes"
  - If the action describes a state change: 
    - Phrases like "system saves/changes/deletes something" should be used. 
    - Note that the system is a blackbox and only the state changes that are visible to the actors should be specified.
  - If the action is another use case: 
    - ```Actor starts the use case "<Use Case Name in Title Case>"```       

### Alternative Flows
- An alternative flow is a conditional sequence of actions that are an alternative to one or more actions in the main flow, after which the use case continues to pursue its goal
  - If the alternative flow is not to be merged back to the main flow and terminate the use case, this should be made clear in the final action of the alternative flow, e.g., "system signals error to the actor and stops the use case"
- Alphabetical enumeration is appended to the numerical enumeration (e.g. 1a) of the main flow to identify the alternative branch and the condition that has led to the branching is described declaratively using present tense. Then, the necessary actions until the branch can be merged with the main flow are listed in new lines with a numerical enumeration appended to the alphanumerical identifier. E.g., the identifier "1b3" can be translated as "the third action in the second alternative branch of the first action of the main flow"
- If the alternative branch is possible during any step and not at a particular one, the numerical identifier can be replaced with "*"
  - E.g. ```*a. System crashes```
- Writing style:
  - Same writing styles as for the actions of the main flow 

### Information Requirements
- Lists the required information for the use case which needs to be accessed through external systems (secondary actors) residing outside of the system boundaries 
- Since information requirements are shared across different use cases, the definition is made in the ubiquitous language of the application and only referenced in this section 
  - Optional remarks specific to the considered use case that extend the definition in the ubiquitous language can be added for each information requirement
- Information requirements should be as precise as possible but not technical
  - The level of abstraction shouldn't come down to technical attribute names (e.g. "customer ID" instead of "cus_id")

---

## Example

```
Title: Add Car to Fleet

Primary Actors: Fleet Manager
Secondary Actors: Connected Car System

Preconditions: 
    - System has a fleet registered for the actor 
Postconditions: 
    - The car with the given VIN is added to the fleet
    - Static car data on the car with the given VIN is saved

Flow:
1. Actor adds a new car to the fleet by providing the VIN
2. System validates that the given VIN is valid and a car with the given VIN is not present in the fleet
3. System gathers static car data on the car with the given VIN from the connected car system 
4. System adds the car to the fleet with its VIN, brand, model and production date 

Alternative flows:
2a. Given VIN is invalid
    2a1. System prompts actor to re-enter the VIN
2b. A car with the given VIN is already present in the fleet
    2b1. System informs the actor that the car is already in the fleet and stops the use case
3a. System detects failure to communicate with the connected car system
    3a1. System signals error to the actor and stops the use case 
3b. Connected car system does not have a car with the given VIN
    3b1. System informs the actor that a car with the given VIN does not exist and stops the use case 


Information Requirements: 
    Connected Car System:
    -  Static car data: VIN, brand, model, production date
```

--- 

## Appendix 

- LaTeX Template

    ```
    \begin{minipage}{\linewidth}
    \begin{lstlisting}[caption = {Use Case Template}, style = kit-cm, label = {lst:use_case_template}, captionpos=b, keywords={}]
    Title: <Goal of the use case as a short active verb phrase>

    Primary Actors: <Comma separated list of primary actors>
    Secondary Actors: <Comma separated list of secondary actor>

    Preconditions: 
        - <Declarative description of the precondition>  
        - ...
    Postconditions:
        - <Declarative description of the postcondition> 
        - ... 
        
    Flow:
    1. <Actor> <verb> <direct object> <prepositional phrase>  
    2. ... 

    Alternative flows:
    1a. <Declarative description of the condition leading to branching>:
        1a1. <Alternative flow step>
        1a2. ... 

    Information Requirements: 
      <Secondary Actor>:
      - <Information required by the system from the secondary actor>: <optional: remarks and details> 
      - ... : ... 

    \end{lstlisting}
    \end{minipage}
    ```

- References
  - [Er22] Utku Erol: Microservice Engineering Using an Integration Platform as a Service, Master Thesis, 2022
  - [Co01] Alistair Cockburn: Writing effective use cases, Pearson Education India, 2001
  - [La05] C. Larman, P. Kruchten: Applying UML and Patterns: An Introduction to Object-oriented Analysis and Design and Iterative Development, Prentice Hall PTR, 2005

