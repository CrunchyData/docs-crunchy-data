---
title: "ICS 500-27 (AU-2A) Validated Events"
draft: false
hidden: true
---

(1) Authentication Events
-------------------------

  Operation   Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ----------- -------- --------- --------- ------ ------ ----------- --------
  Log-ON               V-72923             Y      Y      Y           
  Log-OFF              V-72923             Y      Y      Y           

Example of the successful `Log-On` Audit Event (Pass Column)
` < 2017-06-29 14:49:05.393 UTC [unknown] vcap postgres 59551361.5597 2017-06-29 14:49:05 UTC [local] >LOG:  connection authorized: user=vcap database=postgres`

Example of the successful `Log-Off` Audit Event (Pass Column)
` < 2017-06-29 14:49:06.625 UTC psql vcap postgres 59551361.5597 2017-06-29 14:49:05 UTC [local] >LOG:  disconnection: session time: 0:00:01.233 user=vcap database=postgres host=[local]`

Example of the unsuccessful `Log-On` Audit Event (Fail Column)
` < 2017-06-29 14:54:48.223 UTC [unknown] vcap1 postgres 595514b8.1ec8 2017-06-29 14:54:48 UTC [local] >FATAL:  no pg_hba.conf entry for host "[local]", user "vcap1", database "postgres"`

(2) File Events
---------------

**WARNING:** User accounts cannot perform any file, permission, or
ownership operations.

  Operation               Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ----------------------- -------- --------- --------- ------ ------ ----------- --------
  (1) Create                                           N/A    N/A    Y           
  (2) Access                                           N/A    N/A    Y           
  (3) Delete                                           N/A    N/A    Y           
  (4) Modify                                           N/A    N/A    Y           
  (4) Modify Permission                                N/A    N/A    Y           
  (4) Modify Ownership                                 N/A    N/A    Y           

(3) Database Object Events
--------------------------

### (1) Database Object Create Events

  Operation    Object       STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ------------ ------------ --------- --------- ------ ------ ----------- --------
  (1) Create   TABLESPACE                       Y      Y      Y           
  (1) Create   DATABASE                         Y      Y      Y           
  (1) Create   SCHEMA                           Y      Y      Y           
  (1) Create   TABLE        V-72843             Y      Y      Y           
  (1) Create   FUNCTION                         Y      Y      Y           
  (1) Create   TRIGGER                          Y      TBD    Y           
  (1) Create   VIEW                             Y      Y      Y           

Example of the successful `Create` Audit Event (Pass Column)
` < 2017-06-29 15:07:11.429 UTC psql vcap postgres 595516f0.17e7 2017-06-29 15:04:16 UTC [local] >LOG:  AUDIT: SESSION,1,1,DDL,CREATE TABLESPACE,,,CREATE TABLESPACE test_tablespace OWNER crunchy LOCATION '/var/vcap/store/postgresql/data/';,<none> < 2017-06-29 15:25:18.317 UTC psql vcap postgres 59551bdd.243e 2017-06-29 15:25:17 UTC [local] >LOG:  AUDIT: SESSION,1,1,DDL,CREATE DATABASE,,,CREATE DATABASE stig_test_db1 WITH owner = crunchy;,<none>`

Example of the unsuccessful `Create` Audit Event (Fail Column)
` < 2017-06-29 21:59:03.616 UTC psql test_user2 postgres 59557713.3d7d 2017-06-29 21:54:27 UTC localhost(48550) >ERROR:  permission denied to create database`

### (2) Database Object Access Events

  Operation    Object       STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ------------ ------------ --------- --------- ------ ------ ----------- --------
  (2) Access   TABLESPACE                       Y      Y      Y           
  (2) Access   DATABASE                         Y      Y      Y           
  (2) Access   SCHEMA                           Y      Y      Y           
  (2) Access   TABLE        V-72843             Y      Y      Y           
  (2) Access   FUNCTION                         Y      Y      Y           
  (2) Access   TRIGGER                          Y      Y      Y           
  (2) Access   VIEW                             Y      Y      Y           

Example of the successful `Access` Audit Event (Pass Column)
` < 2017-06-29 15:35:00.023 UTC psql vcap postgres 59551e24.220b 2017-06-29 15:35:00 UTC [local] >LOG:  AUDIT: SESSION,1,1,WRITE,INSERT,,,INSERT INTO  test_schema.test_table VALUES (1);,<none>`

Example of the unsuccessful `Access` Audit Event (Fail Column)
` < 2017-06-29 22:07:30.377 UTC psql test_user2 postgres 59557713.3d7d 2017-06-29 21:54:27 UTC localhost(48550) >ERROR:  permission denied for database stig_test_db`

### (2a) Database Object Access via SELECT

**WARNING:** To generate audit statements for each SELECT command,
`pgaudit.log='READ'` must be enabled. This generates enormous amount of
logging and is not enabled by default. Use discretion when enabling this
option.

  Operation     Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ------------- -------- --------- --------- ------ ------ ----------- --------
  (2a) Access                                Y      Y      Y           

### (3) Database Object Delete Events

  Operation    Object       STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ------------ ------------ --------- --------- ------ ------ ----------- --------
  (3) Delete   TABLESPACE                       Y      Y      Y           
  (3) Delete   DATABASE                         Y      Y      Y           
  (3) Delete   SCHEMA                           Y      Y      Y           
  (3) Delete   TABLE        V-72843             Y      Y      Y           
  (3) Delete   FUNCTION                         Y      Y      Y           
  (3) Delete   TRIGGER                          Y      Y      Y           
  (3) Delete   VIEW                             Y      Y      Y           

Example of the successful `Delete` Audit Event (Pass Column)
` < 2017-06-29 22:34:14.947 UTC psql vcap postgres 59557f5c.7da0 2017-06-29 22:29:48 UTC [local] >LOG:  AUDIT: SESSION,3,1,DDL,DROP DATABASE,,,drop database stig_test_db1 ;,<none>`

Example of the unsuccessful `Delete` Audit Event (Fail Column)
` < 2017-06-29 22:10:08.399 UTC psql test_user2 postgres 59557713.3d7d 2017-06-29 21:54:27 UTC localhost(48550) >ERROR:  permission denied for schema test_schema`

### (4) Database Object Modify Events

  Operation    Object       STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ------------ ------------ --------- --------- ------ ------ ----------- --------
  (4) Modify   TABLESPACE                       Y      Y      Y           
  (4) Modify   DATABASE                         Y      Y      Y           
  (4) Modify   SCHEMA                           Y      Y      Y           
  (4) Modify   TABLE        V-72843             Y      Y      Y           
  (4) Modify   FUNCTION                         Y      Y      Y           
  (4) Modify   TRIGGER                          Y      Y      Y           
  (4) Modify   VIEW                             Y      Y      Y           

### (5) Database Object Modify Permission Events

  Operation               Object       STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ----------------------- ------------ --------- --------- ------ ------ ----------- --------
  (5) Modify Permission   TABLESPACE                       Y      Y      Y           
  (5) Modify Permission   DATABASE                         Y      Y      Y           
  (5) Modify Permission   SCHEMA                           Y      Y      Y           
  (5) Modify Permission   TABLE        V-72843             Y      Y      Y           
  (5) Modify Permission   FUNCTION                         Y      Y      Y           
  (5) Modify Permission   TRIGGER                          Y      Y      Y           
  (5) Modify Permission   VIEW                             Y      Y      Y           

### (6) Database Object Modify Ownership Events

  Operation              Object       STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ---------------------- ------------ --------- --------- ------ ------ ----------- --------
  (6) Modify Ownership   TABLESPACE                       Y      Y      Y           
  (6) Modify Ownership   DATABASE                         Y      Y      Y           
  (6) Modify Ownership   SCHEMA                           Y      Y      Y           
  (6) Modify Ownership   TABLE        V-72843             Y      Y      Y           
  (6) Modify Ownership   FUNCTION                         Y      Y      Y           
  (6) Modify Ownership   TRIGGER                          Y      Y      Y           
  (6) Modify Ownership   VIEW                             Y      Y      Y           

(4) Writes/Downloads to External Devices/Media
----------------------------------------------

**WARNING:** User accounts cannot perform any writes/downloads to
external devices/media operations.

(5) Uploads from External Devices
---------------------------------

**WARNING:** User accounts cannot perform any uploads from external
device operations.

(6) User and Group Management Events
------------------------------------

### (1) User Management Events

  Operation          Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ------------------ -------- --------- --------- ------ ------ ----------- --------
  (1) User ADD                                    Y      Y      Y           
  (2) User DELETE                                 Y      Y      Y           
  (3) User MODIFY                                 Y      Y      Y           
  (4) User SUSPEND                                -      -      Y           
  (5) User LOCK                                   Y      Y      Y           

### (2) Group Management Events

  Operation           Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ------------------- -------- --------- --------- ------ ------ ----------- --------
  (1) Group ADD                                    Y      Y      Y           
  (2) Group DELETE                                 Y      Y      Y           
  (3) Group MODIFY                                 Y      Y      Y           
  (4) Group SUSPEND                                -      -      Y           
  (5) Group LOCK                                   Y      Y      Y           

(7) Use of Privileged/Special Rights Events
-------------------------------------------

### (1) Security or Audit Policy Changes

  Operation                          Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ---------------------------------- -------- --------- --------- ------ ------ ----------- --------
  Security or audit policy changes                                -      -      Y           
  Configuration changes                                           -      -      Y           

(8) Admin or Root-Level Access
------------------------------

**WARNING:** User accounts do not have admin or root level access.

(9) Privilege/Role Escalation
-----------------------------

  Operation         Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ----------------- -------- --------- --------- ------ ------ ----------- --------
  Role escalation                                -      -      Y           

(10) Audit and Log Data Accesses
--------------------------------

**WARNING:** User accounts do not have access to the back end system and
files.

(11) System Reboot, Restart and Shutdown
----------------------------------------

  Operation   Object   STIG ID   NIST ID   Pass   Fail   INHERITED   HYBRID
  ----------- -------- --------- --------- ------ ------ ----------- --------
  SHUTDOWN                                 Y             Y           
  RELOAD                                   Y             Y           
  RESTART                                  Y             Y           

(12) Print to a Device
----------------------

**WARNING:** The system cannot print to a device.

(13) Print to a File (e.g. PDF Format)
--------------------------------------

**WARNING:** The system cannot print to a file.

(14) Application (e.g., Firefox, Internet Explorer, MS Office Suite, etc.) Initialization
-----------------------------------------------------------------------------------------

**WARNING:** The system cannot initialize applications.

(15) Export of Information Include (e.g. to CDRW, thumb drives, or remote systems)
----------------------------------------------------------------------------------

**WARNING:** The system cannot export information to devices such as
CDRW, thumb drives, or remote systems.

(16) Import of Information Include (e.g., from CDRW, thumb drives, or remote systems)
-------------------------------------------------------------------------------------

**WARNING:** The system cannot import information from devices such as
CDRW, thumb drives, or remote systems.
