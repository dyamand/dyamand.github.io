# System tests

!!! success "_l.S.0_ When a token is not sent together with the request, a 401 should be returned"
!!! success "_l.S.1_ When a token is provided, but no permissions have been granted, no installations should be returned even when there is a detected installation"
!!! success "_l.S.2_ When the token subsequently is granted the permission to view unassigned installations, but the installation is not unassigned, no installation should be returned"
!!! success "_l.S.3_ When the detected installation is subsequently made unassigned, the installation should be returned"
!!! success "_l.S.4_ When the installation subsequently is assigned to a profile, the installation should no longer be returned"
!!! success "_l.S.5_ When the token subsequently gets granted the permission to view installations in the profile, the installation should be returned"
