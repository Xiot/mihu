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

We may want a simple store map showing the locations of the aisles / departments
May use gps for this we well depending on how well we can pinpoint the user.
This would involve the map to be aligned with the gps coordinates.
 - Will probably need an admin page on the application to help with this.
   - using the current gps signal, point on the map where you are.  The go to the opposite side of the store and do the same.  Knowing the size of the store we should be able to pinpoint this fairly well. This could also scale the map as well.  More points means better accuracy.
 - departments could be outlined to be able to show users where to go.
Since this feature requires alot of setup, it would be optional.  Would be very nice though for large stores.

Architecture
----------------------

web service
 - main source of data for the applications (both client and server side)

queuing service
 - messages are served from here

notification service
 - sends out push notifications and potentially hosts the websockets

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


Server - Admin Page
----------------------
User can create their own organization / location / departments
associates are then created and added to the location and/or department
stats are available at the organization, location, and department levels to show things like
 - total number of customers helped
 - avgerage sla of 'help requested' customers
 - number of product searches
 - number of product searches that evolved into 'help requested'