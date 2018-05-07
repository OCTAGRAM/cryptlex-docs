---
description: >-
  License policies help you in implementing licensing models as per your
  business requirements.
---

# License Policies

## Creating a license policy

You can easily create a license policy through dashboard. Go to the [license policies](https://app.cryptlex.com/license-policies) section in the dashboard and click the add the button. A license policy form with following fields will popup: 

#### Name

This name of the license policy.

#### Validity

The duration \(in seconds\) after which the license will expire.

{% hint style="info" %}
To create a **perpetual** license set the validity to zero and for **subscriptions** set it to any value greater than zero.
{% endhint %}

#### Type

There are three types of licenses:

* **Node Locked:** This is the default type which locks the license to the machine.
* **Floating:** This type is used for created a hosted floating license.
* **On-Premise Floating: **This** **type is used for creating an on-premise floating license.

#### Lease Duration

This option is valid for **floating** license type only. It sets the duration for which you want to lease the floating license.

#### Allowed Floating Clients

This option is valid for **on-premise** **floating** license type only. It sets the maximum number of concurrent clients which can lease the license from the server.

{% hint style="info" %}
For hosted floating licenses, the the maximum number of concurrent clients which can lease the license from the server is determined by **allowedActivations** property
{% endhint %}

#### Allowed Activations

Allowed number of activations for the license. If you allow \(say\) 10 activations for a license, then the license can be used on 10 different machines.

#### Allowed Deactivations

Allowed number of deactivations for the license. This setting is ignored for floating licenses.

#### Server Sync Interval

Whenever the application starts, the server sync occurs immediately. This setting determines the interval for further server syncs if the application is not closed.

#### Server Sync Grace Period

The duration for which the server sync failure due to network error is acceptable.

#### Expiration Strategy

The strategy to determine the expiration start date. It allows for following strategies:

* **Immediate:** The license starts expiring right after the it is created.
* **Delayed:** The license starts expiring after the license is activated first time. If the license allows more than one activation then all the other activations will also start expiring right after the first activation of the license.
* **Rolling:** The license starts expiring after the license is activated. If the license allows more than one activation even then all the activations will start expiring after they are activated.

{% hint style="info" %}
You may not allow license deactivations if **rolling** expiration strategy is being used, as it would reset the expiry.
{% endhint %}

#### Fingerprint Matching Strategy

LexActivator generates a structured fingerprint of the machine which allows for multiple fingerprint matching strategies. It allows for following strategies:

* **Exact:** This strategy requires an exact match of all the hardware parts which were fingerprinted. If their is a minor change in the hardware, fingerprint will not be accepted, and machine will be treated as a different machine.
* **Fuzzy:  **This strategy uses fuzzy matching by comparing different hardware fingerprints and if the comparison score is greater than a minimum threshold value, the machine is accepted.
* **Loose:** This strategy is similar to fuzzy but with a much lower threshold value.

{% hint style="info" %}
Sometimes machines misbehave by reporting major changes in hardware fingerprints due to many issues, in such cases you should set the strategy to **"Loose"**.
{% endhint %}

#### Allowed Countries

List of the allowed countries. The country is resolved using the IP address. If none of the country is included, this setting is ignored.

#### Disallowed Countries

List of the disallowed countries. The country is resolved using the IP address. If none of the country is included, this setting is ignored.

#### Allowed IP Addresses

List of the allowed IP addresses. If none of the IP addresses is included, this setting is ignored.

#### Disallowed IP Addresses

List of the disallowed IP addresses. If none of the IP addresses is included, this setting is ignored.

#### Allow VM Activations

Whether to allow an activation inside a virtual machine. Cloned virtual machines may report a same fingerprint.

#### User Locked

It prevents users from using the same license key in a machine for different user accounts.
