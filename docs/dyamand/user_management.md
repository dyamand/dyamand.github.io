--8<-- "includes/abbreviations.md"
[![Work in progress](https://img.shields.io/badge/status-wip-yellow)](https://www.repostatus.org/#wip)

# How can you organize users?

DYAMAND allows you to give users access to features on certain assets that are managed by you. A fine-grained permission system is used to allow for an authorization scheme tailored to your specific needs.

## Organizations

An organization is starting point to organize users. An organization is basically a group of users while [profiles](./device_management.md) are used to group assets, be it [installations](./device_management.md) or [devices](./device_management.md).

Users that are part of an organization can get permission to handle certain aspects of assets managed by the organization.

Users can be part of zero or more organizations.

## Teams

Teams allow you to group users that have a similar need. Teams can be created based on the role users have in the company, geographical location, the assets they have access to or any combination of all of the above. Teams can have subteams.

## Permissions

## Examples

The generic mechanism described above can be used to achieve multiple, different structures. Consider a device manufacturer that wants to give distributors access to assets that are sold by them. The organization might make teams per distributor and assign permissions to a distributor to monitor and manage all the units they sold. 

Another example could be a company that manages buildings. A team could be created per building, using subteams to indicate the responsibility of the member users. Subteams could be created based on the systems to manage and/or based on the specific permissions the subteam gets.

- [ ] Overview of permissions on an organization/team level
- [x] Examples?
- [ ] Link to tests
