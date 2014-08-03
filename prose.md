#Why #
## Current events experience ##
Events, without it most UI systems would be simply impossible. Also in real life we wait for events to occur and allows us to be working on something completely different. 

I need to go to the store and get some groceries. Will I actively be polling if the store is open? Some may do this, but I strongly advise against it. 

I on the other hand like to set a timer to alert me to stop whatever I'm doing and do whatever I have to.
So the alarm clock will trigger an event.

Am I a fan of events over procedural solutions?
No I'm just a fan of the simplest solution out there. So please don't use events to sort numbers or other things you wouldn't use events for in real life.

If your domain model doesn't really involve events or waiting of sorts, please don't use events.

I've used events before as a java/swing developer.
Despite that we need them I had a very bad experience. Here is why: 

- A event driven system seldomly begins complicated, but when it grows a cascade of events can have unique effects. It can become a spiderweb of interactions. Add a little multithreading and nobody knows what happening anymore. 
-  I've spent hours checking why something didn't trigger any events, or why an event was triggered twice.
-  Some systems allow very troublesome passing of variables. When your event is triggered you get too little information, so you have to inject it through the constructor. Dependency weaving isn't fun. 
-  What/Who on earth changed my variable? In some ORMs you can add events to do something just before writing to the database. What you see, is not what you get. I've been unpleasantly surprised, and I'm guessing I'm not the only one.

#What #
E* is a language that seeks to solve the following problems in event programming:

- auditing, who changed what.
- avoid excessive weaving and keep a maintenable scope.
- centralize business logic in a clean format.

#Terms#

*Event definition*: This defines an event, based on a event period, trigger condition and multiple guarded variable spaces. 

*guarded variable space*: Is a modifier and a variable space name.

*modifier*: This can be read or readwrite. This indicates that the subject is read only(read) or read and write (readwrite).

*trigger condition*: This is what triggers the event.

*event period*: This specifies how often a trigger is evaluated. 

*variable space*: This is a set of (variable definition , type and value) tuples and/or other guarded variable spaces. A variable space MUST NOT include itself. If a variable space includes the same variable multiple times, the most permissive modifier is used.  

#An example#
Let's make a small website that records promises of people. 

People swear by using their name and promise something. 


- A User can swear an oath, this is stored in the database.  
- Users can always see the oaths.
- A User is sent an email if the deadline of the oath is reached.
- A validator can confirm that the oath has been forefilled. This marks the oath as completed.
- An oath is removed after it was marked as completed after a week. 

Please note that these specification are structured as 'when something happens' then 'something else should occur'.

**event** (a user swears an oath) **is** requestSpace.isPost and requestSpace.oath not empty **uses** *read* requestSpace**;**


**on**  (a user swears an oath) **trigger** storeOath;


The bnf syntax is:

    <program> ::= <eventDefinitionList> <triggerDefinitionList>
	<eventDefinitionList> ::= <eventDefinition> | <eventDefinition> <eventDefinitionList>
	<eventDefinition> ::= "event(" <conditional> ")" "uses" <guarded variablespace list> ";"
	<triggerDefinitionList> ::= <triggerDefinition> <triggerDefinitionList>  
	<conditional> ::= Currently this is language specific.
	<guarded variablespace list> ::= <guarded variablespace> | <guarded  variablespace><guarded variablespace list>

	<guarded variablespace> ::= TBD
	


> Can I trigger an event myself other than triggering the condition? 

No. This is a explicit decision. Events can only be triggered **iff** the trigger condition has been met. Otherwise it is very hard to understand why an event has triggered.

#How #
