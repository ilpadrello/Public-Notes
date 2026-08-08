The core idea is just to model the software based on the real world business domain.
So you do not start with the database, you do not start with the framework etc...
You start analyzing the business domain.

Before writing a single line of code, DDD focuses on understanding the problem space and carving it up cleanly.

# The big picture
The idea is to have a big picture in mind, and to do so some important tips are required.

### Use the same names:
"Ubiquitous Language", programmers and steak holders should use the same name to call the same thing.
If the product manager call a feture "Booking" so should the code ! And not "Reservetion" or "Order"

`There should be zero translation needed between business logic and source code`

### Bounded Contexts

In a large system, words lose their meaning if they are applied everywhere. For example, the word "Product" means something completely different to the Inventory team (dimensions, weight, warehouse aisle) than it does to the Sales team (price, discount, marketing description).

- A **Bounded Context** is a explicit boundary within which a specific domain model applies. Inside the boundary, all terms have a singular, unambiguous meaning.

# Tactical Design (The Code Blueprint)
Once you've drawn boundaries around your contexts, `DDD` provides a set of design patterns to model the reality inside those boundaries.
