## Sorting Service

!!! success "_i.0.1.a_ For all provided sort options, the length of the data should remain the same after calling the sort method. The sorting method is called after subscribing to the Observable"
!!! success "_i.0.1.b_ For all provided sort options, the length of the data should remain the same after calling the sort method. The sorting method is called before subscribing to the Observable"
!!! success "_i.0.2_ For the same provided sort option, after sorting n times whereat n is odd, element[i + 1] of the data should be greater than element[i] but if n is even, element[i] of the data should be greater than element[i + 1]"
!!! success "_i.0.3_ After sorting with n different sort options, sorting it once with a known sort option should result in element[i + 1] of the data being greater than element[i]"
!!! success "_i.0.4_ For all provided sort options, after sorting n times the histogram of the data should be the same as the original one"
!!! success "_i.0.5_ If source information gets changed and the sort method got called already, the source information should be sorted"

## GraphQL Client Service

!!! success "_i.1.0_ If executing a random query, the endpoint should be invoked with that query and the reset method of the error message service should also be invoked"
!!! success "_i.1.1_ If executing a random mutation, the endpoint should be invoked with that mutation and the reset method of the error message service should also be invoked"

## Navigation Service

!!! success "_i.2.0_ After navigating to a random unknown path and getting the current URL should return the page-not-found path"
!!! failure "_i.2.1_ After navigating to a random known path and getting the current URL should return that known path _Remark: Skipped test due to lazy loading and appGuard_"

## Search Service

!!! success "_i.3.0_ If more than one set of searchbar data get added, all added data should be found"
!!! success "_i.3.1_ If the user searches for a substring of the available search terms, the result should contain all valid search terms which have this specific substring"
!!! success "_i.3.2_ If an array of search data gets removed, the search method should return the data array without the removed elements"

## Authentication Service

!!! success "_i.4.0_ If user is logged in or logged out, calling getLoginStatus should return true or false respectively"
!!! success "_i.4.1.a_ If user is logged in, calling updateTokenInformation should return the parsed Token object correctly"
!!! success "_i.4.1.b_ If user is not logged in, calling updateTokenInformation should not return the parsed token"
!!! success "_i.4.2_ If the user is not logged in, calling the loginOrLogout method and then getting the login status should return the correct login status"
!!! success "_i.4.3.b_ If updateTokenInformation fails (due to session timeout), no token should be returned"

## Message Service

!!! success "_i.5.0_ Calling the addMessage method sets the message with the indicated information and calling getMessages returns all messages of the message service"
!!! success "_i.5.1.a_ Calling removeMessages without any parameters will remove all messages of the message service"
!!! success "_i.5.1.b_ Calling removeMessages with messages indicated will remove the messages passed to that method"

## Page information Service

!!! success "_i.6.0_ Setting different page information and getting it should return the information which got set last"
!!! success "_i.6.1_ Calling the removeInformation method and getting the page information should remove any set page information"

## Error handling Service

!!! success "_i.7.0_ Adding n errors and getting all errors should result in n error updates where the size of the errors gets incremented by 1 with each update and the last update should contain all n errors which got added to the service"
!!! success "_i.7.1_ After adding and removing n errors, getting all errors should return an empty array"	

## Logging service

!!! success "_i.8.0_ Getting all logs and log n entries should result in n nexts of the logged entries"
