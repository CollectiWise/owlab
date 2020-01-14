**The Opencog and ScoringRule Interface (API)**

The following functions are the beginning of an interface that implements a new type of Logical Prediction Market, which is a generalization of a combinatorial prediction market, built on opencog and PLN (Probabilistic Logic Networks). 

The following functionality has been implemented on this image: gcr.io/collectiwise/flask-oc:latest

Functions I've built and (in the case of implication, I'm still working on):
with names of things: (uri==uri string == id string)

1) find_users (GET): 
- takes nothing 
- returns a list of user uri strings (IDs). 

2) create_user (POST):
- takes: ID: uri string, first: string of first name, last: string of last name 
- returns: nothing

3) find_synonyms (GET): 
- takes: ID:uri string  
- returns a list of names that are synonyms. 

4) create_concept (POST): 
- takes: concept_id: a uri string, concept_name: a name string, user: a uri string 
- returns nothing

5) create_predicate (POST):
- takes: name_id: a predicate name, which is also an id, user: a uri string, ** and optionally** context: a uri string, representing a concept that is a context. 
- returns nothing 

6) create_relation (POST):
-  takes: name_id, again a name that serves as id, user_id: a uri string, properties: a dictionary with properties and truth values, such as {"transitive":True, "symmetric":False}
- returns nothing

7) create_relationship (POST)
- takes: user, concept1: uri string, concept2: uri string, relation: name id, weight: float
- returns nothing

8) create_attribute (POST)
- takes: user: uri, concept: uri, predicate: name id, weight: float, **and optionally** a context: uri
-returns nothing

9) create_implication (POST)
- takes: user: uri, arguments: dictionary with labels as keys and statements as values, if_formula: a string like "(A AND B) OR (A AND C) OR (B AND (A OR C))", then_formula: another string like "C OR (B AND A)" , weight: a float.  For the two formula-stings, the parentheses must match; or it throws an error!  The formulas must only contain 1) parentheses, 2) label-strings, "A", "b", "A0, ...,W875" etc and 3) "OR" and "AND" strings.  "NOT" is not yet implemented but I'll be doing that soon! 
- returns nothing

10) change_weight_attribute (POST)
- takes: same as create_attribute, where the weight parameter now is the new weight.  It will effect the user's points though and thus, get_user_points should be called after this one.  To get the new points. It also adds to the list of participants to the discussion of the attribute and thus get_statement_users(attribute) should also be called after this function, to get an update.  
- returns nothing
11) change_weight_relationship (POST)
- same signature as create_relationship
- adds to the relationship's discussants? participants? don't know what to call them.  Also changes price of relationship and of course the points of the user etc. 

12) change_weight_implication (POST)
- same signature as create_implication. Many effects on various data points. 
 
 #there are a number of other functions that I can expose (get_statement_maker, which returns the user id of the person who initially created the statement. 

*****WARNING!  THE get_weight functions are different in that they don't take a user_id! 

13) get_weight_attribute (GET)
- takes: concept: a uri, predicate: a name_id
- returns a float between 0 and 1

14) get_weight_relationship (GET)
- takes: concept1: a uri, concept2: a uri, relation: a name_id
- returns: float between 0 and 1

15) get_weight_implication (GET)
- takes: arguments: a dictionary of the form {"A": {"type": relationship, etc etc}, "B": {"type": "implication", etc. etc.}}, if_formula: a string with balanced parenthesis, containing "OR" and "AND" statements, as well as labels, "A0", ...."R3976", "Z917656432" etc.  and finally, a then_formula: another string of the form "A AND B OR (F OR X)" etc.  If the parentheses aren't balanced an error is thrown. 

16) get_points (GET)
- takes: user: a uri
- returns an integer, such as 100054

17) lookup_exp_attribute (GET)
- takes: concept:uri, predicate: name_id, newweight: float between 0 and 1
- returns worst_case, expected_case, best_case: a tuple with three integers of points that are possible to gain or lose. 

18) lookup_exp_relationship (GET)
- takes: concept1: uri, concept2: uri, relation: name_id, newweight: float between 0 and 1
- returns as 17)

19) lookup_exp_implication (GET) 
- takes: arguments (as described above, see change_weight_implication), if_formula: as described above, then_formula: as described above
- returns as 17, 18

20) get_user_points (GET)
- takes: user: uri
- returns integer 

Here is how these functions are to be called via the API:

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
    curl -d "USER=user&CONCEPT=someconcept&PREDICATE=somepredicate&CONTEXT=somecontext&WEIGHT=weightno" localhost:5000/changeWAttr/<str:user_id>,

    Change Weight Relations:
    curl -d "USER=user&CONCEPT1=someconcept&CONCEPT2=someconcept2&RELATION=somerelation&WEIGHT=weightno" localhost:5000/changeWRel/<str:user_id>,
