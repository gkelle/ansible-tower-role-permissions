ansible-tower-role-permissions
=========

This role updates permissions for users and teams.

Requirements
------------

You will need a username/password combination or a token of a user that is able to manage team and user settings in Ansible Tower.

Role Variables
--------------

tower_host: The IP or fqdn of the Ansible Tower UI. (Default: Null) 
tower_user: The user who will connect to Ansible Tower to perform the LDAP update. (Default: Null)  
tower_password: The password of the user who will connect to Ansible Tower to perform the LDAP update. (Default: Null)  
tower_token: A token of a user who will connect to Ansible Tower to perform the LDAP update. (Default: Null)  

Note: If you are defining a tower_token, there is no need to define tower_user or tower_password.  

tower_url_protocol: Whether to use http or https when talking to the API. (Default: https)  

tower_uri_force_basic_auth: Force sending of basic authentication header upon initial request. (Default: yes)  
tower_uri_validate_certs: Validate that https certificate is valid (Default: no)  

tower_permissions: A dictionary containing permission definitions for users and teams.  

In order to set team permissions, create a list 'team' inside of the tower_permissions dictionary.
Each list item takes three keys - name, organization, and permissions.
name: The name of the team to manage
organization: The organization that the team is a part of.  The team name is not a unique field so we required the organization to differentiate between teams with the same name, but different organization.
permissions: A list of permissions to assign to the team. You can either specify a string representing the name of the permission to assign (the target organization will be the same as the team's organization) or specify a dictionary containing entries for name and organization.  The dictionary form will allow you to assign a permission against another organization.

Example:

tower_permissions:
  team:
    - name: my_team_name
      organization: organization_of_my_team
      permissions:
        - Credential Admin
        - name: Project Admin
          organization: Default

Result: The Credential Admin permission will be assigned against the organization_of_my_team organization and the Project Admin permission will be assigned against the Default organization.



Example Playbook
----------------

    - hosts: localhost
      roles:
         - role: ansible-tower-ldap-settings
           vars:
             tower_host: 10.10.10.100
             tower_permissions:
               team:
               - name: LDAP Engineering
                 organization: LDAP Organization
                 permissions:
                   - Credential Admin
                   - Inventory Admin
                   - name: Inventory Admin
                     organization: Default

License
-------

BSD
