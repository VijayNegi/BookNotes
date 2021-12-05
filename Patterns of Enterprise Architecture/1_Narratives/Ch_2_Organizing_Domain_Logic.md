# Organizing Domain Logic

_A [Transaction Script](https://learning.oreilly.com/library/view/patterns-of-enterprise/0321127420/ch09.xhtml#ch09lev1sec1) ([110](https://learning.oreilly.com/library/view/patterns-of-enterprise/0321127420/ch09.xhtml#page_110))_
- Simplest way to store domain logic
- is essentialy a Procedure that takes the input from Presentation Layer , processes it with validations and calculations , stores data in the databases and invokes any operations from other systems.
- Fundamental Organization is of a single procedure for each Action that a User might do. 
- Script for an action or bussiness Transaction.
- It works well with simple data source layer using Row Data Gateway or Table Gateway.
- Often there will be duplicated code as several transactions need to do similar things.
- Some of this can be dealt with by factoring out common subroutines, but even so much of the duplication is tricky to remove and harder to spot.
- The resulting application can end up being quite a tangled web of routines without a clear structure.


_[Domain Model](https://learning.oreilly.com/library/view/patterns-of-enterprise/0321127420/ch09.xhtml#ch09lev1sec2) ([116](https://learning.oreilly.com/library/view/patterns-of-enterprise/0321127420/ch09.xhtml#page_116))_
- we build a model of our domain which, at least on a first approximation, is organized primarily around the nouns in the domain.
- The logic for handling validations and calculations would be placed into this domain model
	- so shipment object might contain the logic to calculate the shipping charge for a delivery.
- There might still be routines for calculating a bill, but such a procedure would quickly delegate to a _[Domain Model](https://learning.oreilly.com/library/view/patterns-of-enterprise/0321127420/ch09.xhtml#ch09lev1sec2) ([116](https://learning.oreilly.com/library/view/patterns-of-enterprise/0321127420/ch09.xhtml#page_116))_ method.