# Modeling of an Application Microservice API Diagram

This guideline describes the modeling of an application microservice (AM) API diagram. An important part is the consideration of the Domain Microservice (DM) and System Microservice (SM) API diagrams.

The AM API is designed in an AM API diagram with application entities. As the AM API uses the DM APIs and SM APIs to realize its functionality, the AM API diagram also includes the relevant domain and system entities. As a result, the AM API diagram models the conceptual relation of the AM API to the SM and DM APIs. This modeling is then the basis for the AM API specification.

In the following, a guideline to derive an AM API diagram will be presented to systematically consider the relationship of DM and SM APIs to an AM API.

1. Specify the initial version (1.0) of the AM API in the upper left corner. The version of the AM API is specified for the first time when the AM API diagram was initially created and reviewed. The API specification and the implementation must define the same version. If the AM API diagram has changed, the version is changed according to versioning concept.
2. Based on the software architecture, determine the DM APIs and SM APIs which need to be considered.
3. Copy each determined DM and SM API diagrams into a box containing the API's name and its current version. The box should be labeled with `<API-Name>V<X.Y>`. Like AM API diagrams, the DM and SM API diagrams also include their version numbers.
4. Derive application entities based on all use case descriptions which belong to the capability that will be realized with the AM API. Omit the modeling of attributes. Attributes can be modeled after the relations to the domain and system entities are worked out. The relation between the application entities can be worked out.
5. Determine the relation of application entities to the system and domain entities. The relation from an application entity to a domain entity is likely to be an inheritance, as the application entity extends the domain entities with application-specific attributes and methods. The relation from a application entity to a system entity might be a uses association, as a translation of the attributes might be required.
6. The AM API diagram will only contain the relevant excerpt of the DM and SM API diagram. Based on the use case description, determine which attributes in the domain and system entities are required by the application entities. Delete all attributes which are not required by the application entities. Next, delete all value objects and entities which are not required.
7. Add required attributes and methods to the application entities which are not modeled in domain and system entities.
   1. Attributes:
      - Extract relevant attributes from information exchange steps between the primary actor and the system in the use case description.
   2. Methods:
      - Map each use case to a method: Transform the use case title to lower camelCase, and remove the name of the entity that owns the method from the title (e.g., Add Car to Fleet --> addCar)
      - Input parameters are derived from the information passed from the primary actor to the system.
      - Return parameters are derived from the information passed from the system to the primary actors