# Ansible role for Fedcloud catchall ops

This ansible role configures the catchall operations of several fedcloud
components.

### Configuration

The role expects the following variables to be defined:

- `vos` a map that contains entry for each VO with the Check-in credentials:
  ```yaml
  <vo name>:
    auth:
      client_id: <checkin client id>
      client_secret: <checkin client secret>
      refresh_token: <checkin refresh token>
  ```

- `ams_project`: name of the AMS project to use (default `egi_cloud_info`)
- `ams_host`: name of the AMS host (default `msg.argo.grnet.gr`)
- `ams_token`: secret to use to connect to AMS
- `cloud_info_image`: docker image for the cloud-info-provider
  (default `egifedcloud/ops-cloud-info:latest`)
- `site_config_dir`: a directory with yaml files describing each of the
  sites to configure. Format of files:

  ```yaml
  gocdb: <name in gocdb of the site>
  endpoint: <keystone endpoint of the site>
  # optionally specify a protocol for the Keystone V3 federation API
  protocol: openid | oidc (default is openid)
  vos:
  # List of VOs defined as follows
  - name: <vo name>
    auth:
      project_id: <project id supporting the VO vo name at the site>
    # any other optional configuration for cloud-info-provider, e.g:
    defaultNetwork: private | public | private_only | public_only
    publicNetwork: <name of the public network>
  ```

## Sample playbook

```yaml
- hosts: server
  roles:
  - { role: 'fedcloud-ops' }
```


