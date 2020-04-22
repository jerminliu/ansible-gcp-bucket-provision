Role Name
=========

This simple role recreates a directory structure in google cloud storage (GCS) bucket from a source directory of files/templates.

Requirements
------------

You should already have an accesible GCS bucket as well as a service account key (JSON file) set up.

This role relies on the following environment variables:

- GCP_SERVICE_ACCOUNT_CONTENTS: content of the JSON key file
- GCP_AUTH_KIND: serviceaccount.

Role Variables
--------------

This role has an inner variable

- scopes: list of Google scopes for Oauth2 access to Google Cloud.

Apart from this variable, the role relies on several variables:

- gcp_project: ID of the Google Cloud Project.

- gcp_bucket: Name of the Google Cloud Storage Bucket within the project.

- directory: name of the directory to create in the remote bucket that will host the file structure.

Note that if the directory to be uploaded contains templated files, the specific variables should be provided independently from the above ones.

Dependencies
------------

No dependencies are declared.

Example Playbook
----------------

Here is a sample playbook to use the role. The role is looped over with a filetree lookup:

    - hosts: localhost
      connection: local
      gather_facts: no
      vars:
          gcp_project: name-of-gcp-project
          gcp_bucket: name-of-gcs-bucket
          directory: destination-directory-name
      tasks:
        - name: upload to bucket
          include_role:
            name: gcp-bucket-provision
          vars:
            file: "{{ item }}"
          with_filetree: "/some/source/path"
          when: item.state == 'file'

License
-------

GNU GENERAL PUBLIC LICENSE

Author Information
------------------

Raphaël de GAIL
