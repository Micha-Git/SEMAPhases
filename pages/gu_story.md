# Guideline for the Analysis Artifact Story
- Each sentence should begin with the name of a person who instances of a role.  
- Roles and names are introduced in the beginning of the story.  
    - In the case of a Proof of Concept (PoC), this is part of the story of the PoC (and not part of an involved application).  
    - In this part, different roles are seperated by an empty line.  
- Each line contains only one complete sentence.  
- Commentary lines (marked with //) can be used to structure the content of the story.  

### Example Story Excerpt:
```
Carol works for the company Galdara (G) and organizes Training Programs (TP) for practitioners.
Carol uses the web application GTPWebAppV1.0 to manage the TPs and to digitize the Training Certificates (TC).

Bob is a practitioner and takes part in Galdara's Training Program (GTP).

// Issuance of a digital TC  
Carol organizes the training program GTP2.0.  
Bob attends the training program and successfully completes the training program.  
Carol opens TCWebAppV1.0 in her web browser and enters the necessary information for Bob's training certificate.
...

// Revocation of digital TC
Bob misbehaves and violates regulations. 
...
```