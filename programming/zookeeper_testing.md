---
title: "zookeeper testing"
date: 2019-04-06T:21:01+08:00
draft: true
---

## Install

## Simple Usage

## Distrubuted deployment

## The Core of zookeeper

### Data Model

**znode** is the data registers in the zookeeper nomenclature.
-. maximum data size in each znode of no more than 1MB.
-. three types of znode type:
	1. persistent znode.
	2. ephemeral znode. 
	3. sequencial znode.
-. znode watcher.
	1. Any changes to the data of a znode, such as when new data is written to the znode's data field using the **setData** operation
	2. Any changes to the children of a znode, For instance, children of a znode are deleted with the **delete** operation.
	3. A znode being created or deleted, which could happen in the event that a new znode is added to a path or an existing one is deleted.

	regulurations:

	1. Watches and notifications are FIFO.(first in first out)
	2. Watch notifications are delivered to a client before any other change is made to the same znode.
	3. The order of the watch events are ordered with respect to the udpates seen by the zookeeper service.

-. ACL
	- World: anyone who is connecting to the zookeeper
	- Auth: any authenticated user, but doesnot use any ID
	- Digest: the username and password way of authentication
	- IP address: authentication with the IP address of the client

	- Create
	- Read
	- Write
	- Delete
	- Admin: sets ACLs

	- Anyone-ID-unsafe
	- Auth-IDs
	- Open-ACL-unsafe: A completely open ACL, and grants all permissions except the **Admin** permission.
	- Creator-all-ACL: ACL grants all permissions to the creator of the znode
	- Read-ACL-unsafe: the ACL gives the world ability to read

## Programming
