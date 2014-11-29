Design Notes
====

General
----------------------

Organizations and locations are created in the admin page
Employees (with their own logins) are then added to locations
locations are defined as GPS coordinates

The client side application will detect the location when it is opened, and that will be matched with the configured locations
when they click the Help Request button, a notification will be sent up to the server which will route it to the employees logged in for that location

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
 - logins can be restricted to 


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
