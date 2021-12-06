# Layering
- Layering is the one of the most common techniques.
- Example obvious layers are Operating System and Network Layers.
- Advantages of Breaking a system into Layers
	- Understanding of single layer as a coherent whole bcomes easy.
		- We can build an FTP Service on top of TCP wihout knowing details or how ethernet works.
	- Layers can be subsituted with alternative implementations.
		-  FTP Service can without change over ethernet, PPP or any network provider.
	-  Layers make good places for standardization
		-  TCP and IP are standards because they define how their layers should operate.
-  Disdvantages of Breaking a system into Layers
	-  Layers encapsulate some, but not all, things well. Cascading changesare required. 
		-  E.g a property at a lower level if exposed to UI will be travelling across all layers.
	-  Extra Layers comes at a cost of Performance.
-  Hardest Part of Layered Architecture is deciding what layers to have and what the responsibility of each layer should be.


The Evolution of Layers in Enterprise Applications.
- Layering became apparent in the 90s with the advent of client-server systems.
- Two Layered Systems : Client held the user interface and other application code and server usually a relational database.
- Problem arose when UI and Applications started to have domain logic : bussiness rules, validations, calculations etc.
- Code Duplicacy started to happen after that.
- Alternative was to put Domain in the Databases as stored procedures. 
- Stored Procudres are again were very limited in structuring things. 
- Now Object-Oriented world was rising.
- Object Community proposed the answer to above problem, Move to three-layer system.
- Presentation Layer for UI, Domain Layer for Domain Logic, and Data Layer for Data Source.
- Layer and Tier often created confusion but both are synonyms . 
- Tier implied physical saperation
-

The Three Principal Layers

| Layer        | Responsibilities                                                                     |     |
| ------------ | ------------------------------------------------------------------------------------ | --- |
| Presentation | Provision of Services, display of information                                        |     |
| Domain       | Logic that is the real point of the system                                           |     |
| Data Source  | Communication with databases, messaging systems, transaction managers other packages |     |
|              |                                                                                      |     |

Presentation
- how to handle interaction between the user and the software.
- it can be as simple as a command-line or text-based menu system.
- Primary Responsbilities
	- Display information to the user
	- interpret commands from user into actions upon the domain and the data source.


Data Source 
- communicating with other systems that carry out tasks on behalf of the application.
- Can be
	- Transaction Monitors.
	- Messaging Systems, etc


Domain Logic
- Bussiness Logic
- Application needs to do for the domain you are working with.
- Invovles cacluations based on the inputs and stored data,
- validaion of any data that comes in from the presentation,
- Figuring out exactly what data source logic to dispatch.
- Hardest part of Domain logic is to identify what is domain logic and what is not ?
- A Simple test is to try to make a different client, like Command Line Interface for Web Application, if you have to duplicate any functionality then there is a sign of Domain Logic Leaking.


Choosing wher to run Layers
- Separation between layers is useful even if the layers are all running on one physical machine.
- However, there are places where the physical structure of a system makes a difference.
- The data source pretty much always runs only on servers.
- The exception is where you might duplicate server functionality onto a suitably powerful client, usually when you want disconnected operation.
- You can run business logic all on the server or all on the client, or you can split it.
- All Domain Logic on Client
	- Transaction Script
	- Domain Model
	- 


