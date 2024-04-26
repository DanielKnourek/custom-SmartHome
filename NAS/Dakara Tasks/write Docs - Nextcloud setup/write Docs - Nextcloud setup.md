---
created: 2024-04-16T13:11
updated: 2024-04-16T13:13:00
tags:
  - task
status: Not started
depends_on:
  - "[[Dakara Tasks/write Docs - Truecharts setup/write Docs - Truecharts setup.md|write Docs - Truecharts setup]]"
  - "[[Dakara Tasks/write Docs - lldap.md|write Docs - lldap]]"
dependency_completion: 50%
---
```meta-bind
INPUT[listSuggester(
	optionQuery(#task)
):depends_on]
```

```dataviewjs
const {update} = this.app.plugins.plugins["metaedit"].api;
const result = {result: {}};
await dv.view('_Assets/Scripts/dv-StatusCategoryUtils', result);
update('dependency_completion', `${result.result}%`, dv.current().file.path)
```
## Requirements

- [https://truecharts.org/manual/SCALE/guides/getting-started](https://truecharts.org/manual/SCALE/guides/getting-started)
- TrueCharts catalog - premium
- TrueNAS version TrueNAS-SCALE-23.10.2
- app-pool setup
- lldap service - [[Dakara Tasks/write Docs - lldap|write Docs - lldap]]


## Installation

## 1. creating app-pools
- HomeArchive
	- root:root 755
- HomeArchive/nextcloud
    - nextcloud user data folder
    - apps:apps 770
- HomeArchive/HomeArchiveData
    - apps:apps 770
- HomeArchive/HomeArchiveData/Dowloads
    - torrent and other dowload folder
    - apps:apps 770
- HomeArchive/HomeArchiveData/Media
    - complete archive of all shared data
    - apps:apps 770
- HomeArchive/HomeArchiveData/Sdilene
    - extra share folder
    - apps:apps 770
- ssd-data0/app-data/Nextcloud
	- skeleton (default) folder
	- apps:apps 770

## 2. nextcloud

> [!tip]- Picture reference
> ![[app-nextcloud-28.0.4_29.10.33.png]]

> [!info]- Steps
> - Apps → Discover Apps → Application Name (nextcloud) → Install
>
> > [!note] Values
> >
| | |
| ---- | ---- |
| Application Name | name (default) |
| Version                       | 29.10.33                                   |
| App Configuration →           |                                            |
| Initial Admin User            | asuran                                     |
| Initial Admin Password        | "op://Private/nextcloud admin/password"    |
| Default Phone Region          | CZ                                         |
| Access IP                     | 192.168.0.21                               |
| Shared Folder Name            | Sdíleno se mnou                            |
| Collabora configuration →     |                                            |
| Enable Collabora              | Yes                                        |
| username                      | asuran                                     |
| password                      | "op://Private/nextcloud admin/password"    |
| dictionary                    | cs-CZ                                      |
| PHP Configuration →           |                                            |
| Memory Limit                  | 4G                                         |
| Storage and Persistence →     |                                            |
| - App HTML Storage →          |                                            |
| Type of Storage               | PVC                                        |
| - App Config Storage →        |                                            |
| Type of Storage               | Host Path                                  |
| Host Path                     | /mnt/ssd-data0/app-data/Nextcloud/config   |
| - User Data Storage →         |                                            |
| Type of Storage               | Host Path                                  |
| Host Path                     | /mnt/HomeArchive/nextcloud                 |
| - Additional App Storage →    |                                            |
| 1. Type of Storage            | Host Path                                  |
| 1. Host Path                  | /mnt/HomeArchive/HomeArchiveData/Media     |
| 1. Mount Path                 | /mnt/HomeArchiveData/Media                 |
| 2. Type of Storage            | Host Path                                  |
| 2. Host Path                  | /mnt/HomeArchive/HomeArchiveData/Sdilene   |
| 2. Mount Path                 | /mnt/HomeArchiveData/Sdilene               |
| 3. Type of Storage            | Host Path                                  |
| 3. Host Path                  | /mnt/HomeArchive/HomeArchiveData/Downloads |
| 3. Mount Path                 | /mnt/HomeArchiveData/Downloads             |
| Ingress →                     |                                            |
| Main Ingress → Enable Ingress | Yes                                        |
| - Hosts →                     |                                            |
| HostName                      | archive.dakara.stream                      |
| Path                          | /                                          |
| Path Type                     | Prefix                                     |
| Cert-Manager enabled          | YES                                        |
| Cert-Manager clusterIssuer    | dakara-stream-cloudflare                   |
| Resources →                   |                                            |
| CPU                           | 4000m                                      |
| RAM                           | 8Gi                                        |
| Postgresql →                  |                                            |
| Postgres Version              | 16                                         |
| Password                      | "op://Private/nextcloud admin/password"    |

## 3. Nextcloud config

#### 3.1. Enable Apps

> [!info]- Steps
> - log in as administrator (asuran)
> - Menu → (+) Apps → Enable Apps
> 	- [https://archive.dakara.stream/settings/apps/featured](https://archive.dakara.stream/settings/apps/featured)
> 
> > [!note] Apps
> > 
| Name | installed version | App link |
| --- | --- | --- |
| LDAP user and group backend | 1.19.0 | [settings/apps/featured/user_ldap](https://archive.dakara.stream/settings/apps/featured/user_ldap) |
| External storage support | 1.20.0 | [settings/apps/featured/files_external](https://archive.dakara.stream/settings/apps/featured/files_external) |
| Dashboard → Welcome | 1.1.0 | [settings/apps/dashboard/welcome](https://archive.dakara.stream/settings/apps/dashboard/welcome) |
| Memories | 7.2.0 | [settings/apps/multimedia/memories](https://archive.dakara.stream/settings/apps/multimedia/memories) |
| Preview Generator | 5.5.0 | [settings/apps/multimedia/previewgenerator](https://archive.dakara.stream/settings/apps/multimedia/previewgenerator) |

#### 3.2. App configuration - LDAP/AD integration
- menu → Administration settings → LDAP/AD integration
    - [https://archive.dakara.stream/settings/admin/ldap](https://archive.dakara.stream/settings/admin/ldap)
    - [https://docs.nextcloud.com/server/latest/admin_manual/configuration_user/user_auth_ldap.html](https://docs.nextcloud.com/server/latest/admin_manual/configuration_user/user_auth_ldap.html)

##### 3.2.1 LDAP/AD integration → Server

> [!tip]- Picture reference
> ![[Dakara Tasks/write Docs - Nextcloud setup/_Attachments/conf-app-nextcloud-lldap-1.png]]

> [!info]- Steps
>    - Test Base DN button     
 >	→ Configuration OK  
 >	→ 18 entries available within the provided Base DN  
 >	
> > [!note] Values
> >
| | |
| --- | --- |
| Host | ldap://lldap-ldap.ix-lldap.svc.cluster.local |
| Port | 3890 |
| User DN | uid=nextcloud_user,OU=people,DC=dakara,DC=stream |
| Password | "op://Private/lldap nextcloud_user/password" |
| Base DN | DC=dakara,DC=stream |

##### 3.2.1 LDAP/AD integration → Users

> [!tip]- Picture reference
> ![[Dakara Tasks/write Docs - Nextcloud setup/_Attachments/conf-app-nextcloud-lldap-2.png]]

> [!info]- Steps
 > - allowing only users with nextcloud_users group in lldap
 > - Verify settings and count users  
>        → 6 users found  
 >	
> > [!note] Values
> > - Edit LDAP Query 
> > ```
> > (
> > &(objectclass=person)
> > (memberOf=cn=nextcloud_users,ou=groups,DC=dakara,DC=stream)
> > )
> > ```

##### 3.2.1 LDAP/AD integration → Login Attributes

> [!tip]- Picture reference
> ![[Dakara Tasks/write Docs - Nextcloud setup/_Attachments/conf-app-nextcloud-lldap-3.png]]

> [!info]- Steps
>    - allowing only users with nextcloud_users group in lldap
>    - Verify settings - Test Loginname: daniel  
>        → User found and settings verified.
>    - Verify settings - Test Loginname: fakename  
>        → User not found.
>    - Verify settings - Test Loginname: [[daniel@knourek.com]]  
>        → User found and settings verified.
 >	
> > [!note] Values
> >
| | |
| --- | --- |
| LDAP/AD Username | Yes |
| LDAP/AD Email Address | Yes |
> LDAP Filter 
> > ```
> > (&( &(objectclass=person) (memberOf=cn=nextcloud_users,ou=groups,DC=dakara,DC=stream) )(|(uid=%uid)(|(mailPrimaryAddress=%uid)(mail=%uid))))
> > ```

##### 3.2.1 LDAP/AD integration → Groups

> [!tip]- Picture reference
> ![[Dakara Tasks/write Docs - Nextcloud setup/_Attachments/conf-app-nextcloud-lldap-4.png]]

> [!info]- Steps
>    - Verify settings and count the groups     
 >	→ Configuration OK  
 >	→ 2 groups found  
 >	
> > [!note] Values
> >
| | |
| --- | --- |
| Only these object classes | groupOfUniqueNames |
| Only from these groups | family, guests |
> LDAP Filter 
> > ```
> > (&(|(objectclass=groupOfUniqueNames))(|(cn=family)(cn=guests)))
> > ```

#### 3.2. App configuration - External storage
- Menu → Administration settings → External storage
    - [https://archive.dakara.stream/settings/admin/externalstorages](https://archive.dakara.stream/settings/admin/externalstorages)
    
> [!tip]- Picture reference
> ![[Dakara Tasks/write Docs - Nextcloud setup/_Attachments/conf-app-nextcloud-external-storage.png]]

> [!info]- Steps
> - Menu → Administration settings → External storage
>
> > [!note] Values
> >
| | |
| Folder name | External storage | Authentication | Configuration                  | Available for |
| ----------- | ---------------- | -------------- | ------------------------------ | ------------- |
| Sdilene     | Local            | None           | /mnt/HomeArchiveData/Sdilene   | family, admin |
| Media       | Local            | None           | /mnt/HomeArchiveData/Media     | All users     |
| Stažené     | Local            | None           | /mnt/HomeArchiveData/Downloads | All users     |

