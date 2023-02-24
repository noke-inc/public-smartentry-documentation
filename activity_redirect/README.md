# Nokē SmartEntry Activity Redirection 

## Overview

As clients will already know SmartEntry receives activity reports from various Nokē devices. This activity can be viewed and searched in the SmartEntry web portal. Some clients may want to receive the same activity event reports from the hardware in near realtime. SmartEntry Activity Redirection allows our servers to forward any activity it receives to a client's server as it is received.

* [Overview](#overview)
* [Setup](#setup)
  * [Subscribers](#subscribers)
  * [Event Selection](#event-selection)
* [Communication](#communication)

[Return to Main README](../README.md)

## Setup 

### Subscribers
Each client site may have one or more 'subscriber' servers to which Noke will send activity events. For each subscriber, the client will need to the following information to Noke support personnel:

* Name - for identifying the subscriber to humans
* ID - for identifying the subscriber to computers
* URL - the subscriber URL which Noke will be sending activity events
* API Key - this is an API key (random string, hash, GUUID, etc) that Noke will use to authenticate itself to the subscriber.

As an example, a client could give Noke the following for the setup of two subscribers:
* Name: "Develop"
* ID: "1"
* URL: "https://www.JoesStorage.dev.com/activity?site=312"
* API Key: "WLBJ9bs1gUw2wFqE97v5"
* Name: "Production"
* ID: "site_17B"
* URL: "https://www.JoesStorage.com/activity?site=1029"
* API Key: "1BkMQjUEtdJ5QL4bEg2K"

Notes:

1. The URL should use the https protocol
1. The URL may contain any query values the clients wish to add to the URL. SmartEntry will use the URL verbatim and will not add any values to the request body other than the requested activity events.

### Event Selection
The second thing Noke needs is a list of event types the client wishes to receive. There are a lot of events available so a client may not want to receive them all. Currently (23 Feb 2023), there are 66 event types to choose from:

* Admin Password Reset
* Lock Accesscode Remove
* Lock Accesscode Set
* Lock Accesscode Unlock
* Lock Alarm
* Lock Blacklist Add
* Lock Blacklist Remove
* Lock Edit
* Lock Failed Unlock
* Lock Failed Unlock Invalid Accesscode
* Lock Limited
* Lock Lock Time Invalid
* Lock Locked
* Lock Rekey
* Lock Schedule Remove
* Lock Schedule Set
* Lock Sensor
* Lock Unassigned
* Lock Unlock
* Share Create
* Share Delete
* Site Manager Delete
* Site Sync
* Support Companies Get
* Support Gateways Details
* Support Lock Rekey
* Support Login Attempts
* Support Logs Get
* Support Logs Get All
* Support Site Sync
* Support Site Sync Status
* Support Site Sync Units
* Support Unit Externalid Set
* Support Unit History
* Support User Fob
* Support User Fob Remove
* Support User Permissions Edit
* Support User Permissions Get
* Support User Phone
* Support User Phone Details
* Support User Pin Refresh
* Support User Pin View
* Support User Unfreeze
* Unit Assign
* Unit Edit
* Unit Unassign
* Unlock Accessed Offline Access Code
* Unlock Accessed Offline Fob
* Unlock Accessed Offline Phone
* Unlock Accessed Online Phone
* Unlock Accessed Online Phone Bt
* Unlock Accessed Online Phone Mesh
* Unlock Unlocked Mechanical Override
* User Activate Account
* User Client Checkout
* User Confirm
* User Create
* User Delete
* User Edit
* User Fob Remove
* User Fob Sync
* User Login
* User Logout
* User Password Confirm
* User Password Update
* User Site Sync

Noke support reps can provide an updated list if necessary.

## Communication

SmartEntry will send an HTTPS request, for any activity events received, to each of the subscriber URL's. The client's subscriber MUST expect a POST request with a body containing a JSON representation of one or more events. The request will also include an authorization bearer token containing the API Key the client specifies.

The subscriber may return a response with any string as the body and any http error code. SmartEntry will not parse the body content but will log it for later reference (as needed). Any HTTP status code other than OK (200) will be considered a failure.

The request will look something like the following:

	POST /activity?site=312
	Host: www.JoesStorage.dev.com
	Authorization: Bearer WLBJ9bs1gUw2wFqE97v5
	Content-Type: application/json
	Content-Length: ####

	{
		"type": "lock",
		"subtype": "lock.alarm",
		"by": {
			"firstName": "",
			"lastName": "",
			"email": "",
			"phone": "",
			"type": "",
			"byRoles": null
		},
		"for": {
			"firstName": "",
			"lastName": "",
			"email": "",
			"phone": "",
			"type": "",
			"forRoles": null
		},
		"timestamp": "",
		"reason": "",
		"zone": "",
		"lock": {
			"name": "",
			"mac": "",
			"hwType": "",
			"temperature": ""
		},
		"alarmsInZone": null,
		"recordType": "",
		"platform": "",
		"locks": null,
		"lockTimeInvalidType": "",
		"sensorType": "",
		"unit": {
			"name": "",
			"rentalState": "",
			"accessType": "",
			"user": {
				"firstName": "",
				"lastName": "",
				"email": "",
				"phone": "",
				"type": ""
			},
			"externalId": "",
			"width": 0,
			"depth": 0,
			"price": 0,
			"detailsComments": "",
			"contractId": "",
			"lock": null
		},
		"createdDate": "",
		"removedShares": null,
		"motionLimit": 0,
		"motionTimeMinutes": 0,
		"open": "",
		"close": "",
		"notificationsOn": "",
		"site": {
			"name": "",
			"latitude": "",
			"longitude": "",
			"streetAddress": "",
			"city": "",
			"state": "",
			"postalCode": "",
			"suite": "",
			"country": "",
			"phone": "",
			"mainUrl": "",
			"paymentUrl": "",
			"openTime": "",
			"closeTime": "",
			"timeZone": "",
			"lastSyncDate": "",
			"email": "",
			"motion_limit": "",
			"motion_time_minutes": "",
			"notifications_on": ""
		},
		"preferredAppColor": "",
		"mac": "",
		"units": null,
		"addedPermissions": null,
		"removedPermissions": null,
		"byUserType": "",
		"roleId": 0,
		"permissions": null,
		"externalSiteID": "",
		"motionActive": false,
		"userNotificationsSettings": null,
		"deviceDetails": {
			"id": 0,
			"userId": 0,
			"platform": "",
			"platformVersion": "",
			"device": "",
			"appVersion": "",
			"build": 0
		},
		"lockHold": {
			"id": 0,
			"lockID": 0,
			"startTime": "",
			"endTime": "",
			"commandSent": false
		},
		"publicURL": "",
		"contractIds": null,
		"entryMessage": {
			"id": 0,
			"siteID": 0,
			"messageTitle": "",
			"messageBody": "",
			"recipientType": "",
			"recipientIDs": null,
			"createdAt": "",
			"startTime": "",
			"endTime": "",
			"messageFrequency": ""
		},
		"shareWManagers": false,
		"storeId": "bob",
		"pmsId": "2"
	}

You'll notice the only fields with non-default/empty values are the first two (type and subtype) and the last two (storeId and pmsId). These four fields are guaranteed to be populated in every request. The other fields will be populated based on the event type/subtype. All fields are included here for informational purposes.

Please note that *storeId* will hold the client's subscriber's *Name* while *pmsId* will hold the subscriber *ID*
