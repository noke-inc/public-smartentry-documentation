# Nokē Smart Entry (NSE) Activity Redirection 

## Overview

Clients are aware that Nokē Smart Entry (NSE) collects activity events from various Nokē hardware devices, such as locks, gateways, repeaters, and access devices (gates, doors, and elevators). Users can view and search activity events for each device through the NSE Web Portal. For real-time activity event reports, clients can utilize the NSE Activity Redirection API, which promptly forwards received activity events to the client's subscriber server.

* [Overview](#overview)
* [Setup](#setup)
  * [Subscribers](#subscribers)
  * [Event Selection](#event-selection)
* [Communication](#communication)

[Return to Main README](../README.md)

## Setup 

### Subscribers
For each client facility, there may be one or more 'subscriber' servers that Nokē uses to send activity events. To ensure smooth communication, the client must provide the Nokē support personnel with the following information for each subscriber:

* Name - The subscriber server's name.
* ID - The subscriber server's ID number.
* URL - The subscriber server's URL address where Nokē sends activity events.
* API Key - The API key (random string, hash, GUUID, etc.) that Nokē uses to authenticate itself to the subscriber server.

For example, a client provides Nokē the following information to set up two separate subscribers:
* Name: "Develop"
* ID: "1"
* URL: "https://www.JoesStorage.dev.com/activity?site=312"
* API Key: "WLBJ9bs1gUw2wFqE97v5"* 
* Name: "Production"
* ID: "site_17B"
* URL: "https://www.JoesStorage.com/activity?site=1029"
* API Key: "1BkMQjUEtdJ5QL4bEg2K"

Notes:
1. The URL should use the HTTPS encrypted protocol.
2. The URL may contain any query values the clients want to add to the URL. NSE uses the URL verbatim and does not add any values to the request body other than the requested activity events.

### Event Selection
Nokē also requires a list of activity event types the client wants to receive. There are a lot of events available, so a client may not want to receive them all. Currently, (23 Feb 2023), there are 66 activity event types to choose from:

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

Nokē support representatives can provide an updated list, if necessary.

## Communication

NSE sends an HTTPS request for any activity events received to each of the subscriber URL's. The client's subscriber MUST expect a POST request with a body containing a JSON representation of one or more events. The request also includes an authorization bearer token containing the API Key the client specifies.

The subscriber may return a response with any string as the body and any HTTP error code. NSE does not parse the body content but we do log it for later reference, as needed. Any HTTP status code other than OK (200) is considered a failure.

For example, the request might be as follows:

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

You will notice that the only fields with non-default/empty values are the first two, type (line 126) and subtype (line 127), and the last two, storeId (line 249) and pmsId (line 250). These four fields are guaranteed to be populated in every request. The other fields are populated, based on the event type/subtype. All fields included here are for informational purposes only.

Note: The *storeId* holds the client's subscriber *Name* while *pmsId* holds the subscriber *ID*.
