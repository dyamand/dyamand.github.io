# System tests
!!! success "_l.S.0_ When a token is not sent together with the request, a 401 should be returned"
!!! success "_l.S.1_ When a token is provided, but no permissions have been granted, no installations should be returned even when there is a detected installation"
!!! success "_l.S.2_ When the token subsequently is granted the permission to view unassigned installations, but the installation is not unassigned, no installation should be returned"
!!! success "_l.S.3_ When the detected installation is subsequently made unassigned, the installation should be returned"
!!! success "_l.S.4_ When the installation subsequently is assigned to a profile, the installation should no longer be returned"
!!! success "_l.S.5_ When the token subsequently gets granted the permission to view installations in the profile, the installation should be returned"

# Profiles
!!! failure "_l.0.0_ When a user does not have the CREATE_PROFILE permission, creating a profile should fail"
!!! failure "_l.0.1_ When a user creates a profile, the profile should have the exact name as defined by the creator"
!!! failure "_l.0.2_ When a user creates a profile, the profile should have a unique id"
!!! failure "_l.0.3_ When a user does not have the REMOVE_PROFILE permission, removing a profile should fail"
!!! failure "_l.0.4_ When a user creates and then removes n profiles, there should be no new profiles"
!!! failure "_l.0.5_ When a user removes a profile, the profile with the corresponding ID should no longer be retrievable"

!!! failure "_l.0.6_ When a user creates a profile, that user has all administrator permissions within that profile"

!!! failure "_l.0.7_ When a user does not have ADD_USER permission for a given profile, adding new users to that profile fails"
!!! failure "_l.0.7_ When a user adds another user to a given profile, granting specific permissions, that new user has exactly those permissions"
!!! failure "_l.0.8_ When a user does not have REMOVE_USER permission for a given profile, removing users from that profile fails"
!!! failure "_l.0.9_ When a user removes another user from a profile, the removed user loses all permissions for that profile"
!!! failure "_1.0.10_ Users can not remove themselves from a profile
!!! failure "_1.0.11_ When a user does not have ADD_USER permission for a given profile, adding permissions for an existing user within that profile fails"
!!! failure "_1.0.11_ When a user does not have REMOVE_USER permission for a given profile, removing permissions for an existing user within that profile fails"
>USER_UPDATE might be needed as a separate permission

# Installations
!!! failure "_l.1.0_ When a user does not have ADD_INSTALLATION permission for a given profile, adding an installation to that profile fails"
!!! failure "_l.1.1_ When a user adds an installation to a given profile, that installation should be retrievable through that profile"
!!! failure "_l.1.2_ When a user does not have REMOVE_INSTALLATION permission for a given profile, removing an installation for that profile fails"
!!! failure "_l.1.3_ When a user removes an installation from a given profile, that installation should no longer be retrievable through that profile"
!!! failure "_l.1.4_ When a user does not have VIEW_INSTALLATIONS permission, retrieving installations for that profile fail"