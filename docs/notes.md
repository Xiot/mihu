Design Notes
====

General
----------------------

Organizations and locations are created in the admin page
Employees (with their own logins) are then added to locations
locations are defined as GPS coordinates

The client side application will detect the location when it is opened, and that will be matched with the configured locations
when they click the Help Request button, a notification will be sent up to the server which will route it to the employees logged in for that location

When the customer requests help, they can search for what they are looking for. If the service finds the item, they can direct them to the part of the store that has that item.  This may be broken out into a seperate 'Find Product' button to help customers service themselves.

ex. Find: drills
Power tools are located in aisle 13. would you like an associate to help you?
yes: Your request for an associate has been received.
 when the associate accepts the help request, the customer is sent a notification saying 'Help is on the way'

This functionality would involve having intimate details about the layout of the store and the location of all of the product groups

Since we may require all of the organization / location specific data, we may need an on-premise solution.

Product / location details can either be stored in a local database (default) or exposed through an external service. Will need a plugin model to support these services to normalize the data to a format that the system can understand.

Architecture
----------------------

web service
 - includes a websockets hub for notifications

queuing service
 - messages are served from here

datebase
 - probably going with mongo

admin website
 - will have to be multi-tenanted
 - logins can be restricted to organizations / locations

Application Flow
------------------------
### Help Request
 - Clicks the Help Request button.
 	- Sends location and optionally picture to service
 	- response is sent back with some sort of SLA or at least a confirmation
 - The server notifies employees that are on premise and logged in that a customer is waiting.
 - an employee can 'accept' the help request which locks the request to them
 - when employee gets to the customer, they push a button notifying the system that the customer is being helped
 - after they are finished, press another button to signify that they are ready to receive new requests.
 - employees can also signal that they are helping customers that did not come through the mihu app 
 
Client
----------------------

simple interface that will show the current location (if detected) and a Help Requested button.
may also show details for a flyer
will have to figure out where in the store the customer is
 - this will be the hardest part and will probably be the make or break aspect.
 - if the employees cant figure out where the customer is, then this is all for naught
 - maybe take a picture using the front camera so the sales associate can locate recognize the customer.


Server
----------------------
