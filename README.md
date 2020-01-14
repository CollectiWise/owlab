
    Getting Users 
    curl localhost:5000/users

    Adding Users
    curl -d "UID=uid123&FIRST_NAME=Austin&LAST_NAME=Agbo" localhost:5000/createUser, 

    Finding Synonyms 
    curl localhost:5000/synonyms/<CID>, CID = concept ID, 

    Adding concepts:
    curl -d "CONCEPT_USER=someconcept&CONCEPT_NAME=someconceptname" localhost:5000/createConcept,
 
    Creating Relationships:
    curl -d "USER=user&CONCEPT1=someconcept&CONCEPT2=someconcept2&RELATION=somerelation&WEIGHT=weightno" localhost:5000/createRel,

    Creating Attributes:
    curl -d "USER=user&CONCEPT1=someconcept&PREDICATE=somepredicate&CONTEXT=somecontext&WEIGHT=weightno" localhost:5000/createAttr,

    Change Weight Attributes:
    curl -d "USER=user&CONCEPT=someconcept&PREDICATE=somepredicate&CONTEXT=somecontext&WEIGHT=weightno" localhost:5000/changeWAttr/<int:user_id>,

    Change Weight Relations:
    curl -d "USER=user&CONCEPT1=someconcept&CONCEPT2=someconcept2&RELATION=somerelation&WEIGHT=weightno" localhost:5000/changeWRel/<int:user_id>,

    Get A Statement Maker:
    curl -G -d "STATEMENT=statement" localhost:5000/statementMaker,

    Get A Statement User:
    curl -G -d "STATEMENT=statement" localhost:5000/statementUsers,

    Get A Weight:
    curl -G -d "STATEMENT=statement" localhost:5000/getWeight,

    LookUp Points:
    curl -G -d "STATEMENT=statement&NEWWEIGHT=newweight" localhost:5000/lookupPoints,