# System tests
!!! success "_l.S.0_ When a token is not sent together with the request, a 401 should be returned"
!!! success "_l.S.1_ When a token is provided, but no permissions have been granted, no installations should be returned even when there is a detected installation"
!!! success "_l.S.2_ When the token subsequently is granted the permission to view unassigned installations, but the installation is not unassigned, no installation should be returned"
!!! success "_l.S.3_ When the detected installation is subsequently made unassigned, the installation should be returned"
!!! success "_l.S.4_ When the installation subsequently is assigned to a profile, the installation should no longer be returned"
!!! success "_l.S.5_ When the token subsequently gets granted the permission to view installations in the profile, the installation should be returned"
!!! failure "_l.S.6_ When a user does not have the CREATE_PROFILE permission, creating a profile should fail"
!!! failure "_l.S.7_ When a user does not have the REMOVE_PROFILE permission, removing a profile should fail"
!!! failure "_l.S.8_ When a user does not have ADD_INSTALLATION permission for a given profile, adding an installation to that profile fails"
!!! failure "_l.S.9_ When a user does not have REMOVE_INSTALLATION permission for a given profile, removing an installation for that profile fails"
!!! failure "_l.S.10_ When a user does not have VIEW_INSTALLATIONS permission, retrieving installations for that profile fail"
> TODO add system tests for when the user DOES have the correct permissions

# Profiles
!!! failure "_l.0.0_ When a user creates a profile, that user has all administrator permissions within that profile"
!!! failure "_l.0.1_ When a user does not have ADD_USER permission for a given profile, adding new users to that profile fails"
!!! failure "_l.0.2_ When a user adds another user to a given profile, granting specific permissions, that new user has exactly those permissions"
!!! failure "_l.0.3_ When a user does not have REMOVE_USER permission for a given profile, removing users from that profile fails"
!!! failure "_l.0.4_ When a user removes another user from a profile, the removed user loses all permissions for that profile"
!!! failure "_1.0.5_ Users can not remove themselves from a profile
!!! failure "_1.0.6_ When a user does not have ADD_USER permission for a given profile, adding permissions for an existing user within that profile fails"
!!! failure "_1.0.7_ When a user does not have REMOVE_USER permission for a given profile, removing permissions for an existing user within that profile fails"
>USER_UPDATE might be needed as a separate permission
