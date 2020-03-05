
* * *


ThingLogix Foundry

Developer Reference Guide

* * *

# Objects

## Intro

Objects serve as the building blocks for creating any Foundry solution. An IoT device communicating to Foundry will likely associate to an object which will serve as its’ digital twin. Objects are not just IoT devices, however. Entirely digital things like a support ticket, device alarm, or even a contact can also be modeled through objects.

## Object Attributes

Object attributes are the core of what makes up a Foundry Object. Attributes are fields on an object that define its properties. 

For example, an object of type "Car" may have the following properties: 

* Color

* Number of axles

* Make 

* Model 

For more information on defining attributes, refer to the section defining [Attributes](#heading=h.wqkweu1vsex5).

## Object History

### Full History

An Object’s history contains the complete set of modifications made to it. Every time an object is updated all the object’s current attributes are recorded. It is important to note that the **entire** Object is saved every time an attribute is changed. If it appears that there is duplicate data, it likely means the changing attribute isn’t being shown in the table. To show the changing attribute, click the **Add/Remove Columns** button and add the attribute.

![image alt text](image_0.png)

### Posted Data

Posted data differs from the full object history in that new records are only created after an Object is updated from an MQTT message. Additionally, posted data only contains attributes that were part of the MQTT payload, while the [Full History](#heading=h.oy9isgkq64cl) contains a complete copy of the object.

## References

References serve as a way to define relationships between Objects.

### Creating References

To create a reference from Object A to Object B:

1. Ensure an attribute of data type **Reference **exists on the Object Type on Object A. If there is no attribute of type **Reference**, see [References](#heading=h.1ff7ux5vta7s)

2. On Object A, click **Add Attribute **in the **Attributes **tab.

3. Choose the attribute from step 1.

4. With the search bar that pops up, enter either the name or id of Object B.

5. Click **Save.**

### Viewing References

To view all references **to **an Object, click on the **References **tab on the detail page.

![image alt text](image_1.png)

## Dashboard

Dashboards can be added to objects by performing the following steps:

1. Create a dashboard using the [Dashboard Manager](#heading=h.v0ghf2105aqx).

2. Configure the dashboard to include visualizations with the **Data Source** field set to **Object Type**, and the **Object Type** field set to the [Object Type](#heading=h.lnavfrihn7o3) of the object being added to the dashboard.

3. [Create a reference](#heading=h.2e21drj0ztcj) from the object to the dashboard.

4. Navigate to the **Dashboard** tab on the object detail.

This dashboard will update in realtime as mqtt messages are published onto the object. 

## Groups

Groups provide another way to organize objects in Foundry and are made up of two key components; the Parents Group and Children objects. Groups are useful for creating many-to-many relationships as a child object can belong to many groups and groups can contain many children.

### Parent Groups

Groups are just like other objects in Foundry, but with the special property that they can hold other objects inside of it. To create a Group, follow the standard object creation process but check the **Create as Group** checkbox before creation. 

![image alt text](image_2.png)

Standard Objects can be converted to Groups and vice-versa by clicking the edit symbol in the object detail header.

![image alt text](image_3.png)

### Children

To add child objects to a group:

1. Go to the Parent Group Object Details page.

2. Navigate to the **Children Tab**.

3. Click **Add Objects To Group**.

![image alt text](image_4.png)

## Communication (Objects and MQTT)

### What is Mqtt?

Mqtt (MQ Telemetry Transport) is the primary communication protocol used by Foundry. Mqtt follows a pub/sub architecture that allows Mqtt messages to be published to an Mqtt topic and subscribers of this Mqtt topic to receive the messages. 

For official documentation on the IoT Broker that Foundry uses, see [https://docs.aws.amazon.com/iot/latest/developerguide/iot-message-broker.html](https://docs.aws.amazon.com/iot/latest/developerguide/iot-message-broker.html)

### Objects and Mqtt

Each Foundry Object, by default, has a special attribute called Mqtt Topic (mqtt_topic). If an mqtt topic is not specified when creating the object, Foundry will automatically assign one with the structure: **com.thinglogix/{object type id}/{object id}**. 

#### Attribute Updates

When mqtt messages are published onto an Object’s mqtt topic, Foundry breaks down the message payload and updates the attributes on the object in one of three ways.

1. The mqtt message contains an attribute that **is not** on the object, and **is** on the object’s Object Type

    1. The new attribute will be added to the object.

2. The mqtt message contains an attribute that **is **on the object, and **is** on the object’s Object Type

    2. The new attribute will update the old attribute on the object.

3. The mqtt message contains an attribute that **is not **on the object, and **is not** on the object’s Object Type

    3. The new attribute will be added to the object.

    4. The new attribute will be added to the object’s object type.

#### Notes

If an object does not have an Mqtt Topic attribute, it will not be able to receive mqtt messages. If two objects share the same Mqtt Topic, both objects will get updated with new mqtt messages.

Both IoT devices as well as Foundry Objects can use Mqtt to communicate data between each other. To send an mqtt message from one object to another, see [Mqtt Publish Foundry Action](#heading=h.emyuhi3l0uv8).

# Auto Provisioning

When an auto-provisioning rule is configured for an object type, Foundry will automatically create or update an object when a message is published to an mqtt topic matching the rule.

# Object Types

## Intro

Object Types define how an object is structured and behaves -- similar to a blueprint. From a single object type, many instances can be created from it, called Objects. From an Object Oriented Programming viewpoint, an Object Type is comparable to a class, and an object is comparable to an instantiation of that class.

## Attributes

Attributes define the structure of data on an object type.  There is no limit to the number of attributes on a single object type. 

A **Car** object type may have the following attributes:

* Color

* Make

* Model

* Number of axles

### Name vs Key

Each attribute consists of a JSON key and a name. If not specified, the JSON key is derived from replacing spaces with underscores and lowercasing the name. Once an attribute has been created, the attribute JSON key may not be changed. The attribute name may be changed at any time.

Some examples of the name to key transformation:

Temperature -> temperature

Time of Day -> time_of_day

HuMidiTy -> humidity

### Data Types

**Data Types **are what define the type of data that each attribute is. The following data types are supported:

* String (single line text value)

* Textarea (multi-line text value)

* Integer (whole, integer numbers)

* Datetime (date value with time)

* Date (date value without time)

* JSON (Javascript Object Notation)

* Location (latitude-longitude pair. More information [here](#heading=h.el6pmg73r7nj))

* Decimal (floating point number)

* Picklist (a dropdown list of text values)

* Formula (More information [here](#heading=h.fyma3do0ledl))

* File (link or file upload)

* Reference (object reference. More information [here](#heading=h.1ff7ux5vta7s))

* AutoNumber (auto incrementing number. I.e 1, 2, 3…)

* Media (Links to media to display in-line on the UI. Videos, Images, etc)

A **Car **Object Type may have the following data types for its’ attributes:

* Color: Picklist (blue, red, grey, white, black)

* Make: String

* Model: String

* Number of axles: Integer

### Creating Attributes

1. Navigate to the desired Object Type

2. Click on the **Attributes **tab

3. Click **Add new Attribute**

4. Fill in the **name **and **data type **fields.

    1. Certain data types allow for default values which will be automatically filled in when creating the object.

### Characteristics

Attributes also have certain **characteristics **associated with them which affects how they behave.

#### ![image alt text](image_5.png)Required

Attributes marked with the **required** characteristic must be added when creating Objects in the Foundry webapp.

#### Display in View Tab

On each object detail page, there is a **view tab **which formats the attributes in a user-friendly way. By checking this characteristic, the attribute will appear there. For more information on how to organize the **view tab**, see [Attribute Group](#heading=h.e3m8nai23m8).

#### Read Only

This characteristic prevents users from modifying this value in the webapp. This characteristic is only enforced on the webapp -- Mqtt messages still have the ability to overwrite this value.

#### Searchable

This marks the attribute as being searchable in the global search bar. By default, **name **is searchable on all objects. Attributes with this characteristic checked will be indexed for quicker lookups.

![image alt text](image_6.png)

## Formulas

Formulas provide a way to add logic to objects. To view Formulas all formulas on an Object Type, click the **Formula** tab on the Object Type detail page.

A more detailed explanation of formulas can be found in the [Formulas](#heading=h.fyma3do0ledl) part of this document.

## Global Actions

Global Actions can be associated to an Object Type to provide quick access to them on the object detail for Objects of that type. Add global actions to an Object Type by navigating to the **Global Actions** tab on the Object Type detail page, and clicking **Add Global Action**.

## Indexes

Indexes are groups of attributes that are indexed by Foundry in order to make searches faster on those attributes. Indexes are a pivotal part of a Foundry solution; they create more efficient searches and speed up the overall Foundry solution.

Let’s continue with the **Car **example. 

A typical solution with a car would want the ability to search on: 

* The make of the car

* The make and model of the car

* The model of the car

In this case, we have **3 **indexes: **make, make-model, **and **model**.

### Creating Indexes

1. Click on the **Indexes **tab

2. Click **Add new Index**

3. Name the index and choose the attributes to group

![image alt text](image_7.png)

For information on how to search using indexes, see [Object Search](#heading=h.2i87hyktqj24).

## Attribute Group

Attribute Groups provide a way to organize Object attributes on the Object detail page. It is important to note that current attribute groups are not inherited in the account hierarchy so attribute groups will need to be created for each account.

#### Creating Attribute Groups

1. Go to the Object Type where the attribute group is going to be created.

2. Click on the **Attribute Groups** tab and click **Create New Attribute Group**.

3. Give the **Attribute Group** a name and a sort order. Lower sort numbers will always appear above greater sort numbers. ThingLogix recommends incrementing in 10’s.![image alt text](image_8.png)

#### Adding Attributes to a Group

1. Go to the Object Type where the attribute group was created.

2. Either create a new attribute or edit an existing attribute.

3. Select the created Attribute Group from the dropdown list.![image alt text](image_9.png)

## Special Attributes

### Mqtt Topic (mqtt_topic)

The mqtt_topic attribute is used as the core mechanism for communicating to objects. Objects will automatically subscribe to their mqtt_topic and will update themselves (and their Object Type for new message keys) based on the messages received.

For more information, see [Communication (Objects and MQTT)](#heading=h.3mlme0lih3z7).

### Icon Image (icon_image)

Adding a string url to this displays the image on the Object detail page for all Objects. This can be useful for displaying a static or dynamic image that is associated with a particular type of Object. This attribute can be configured as a formula to generate dynamic urls based on the object state. E.g. Displaying a picture of fire if temperature is above a threshold, and displaying ice if below a threshold.

### Last Mqtt Timestamp (last_mqtt_timestamp)

The last_mqtt_timestamp attribute is updated with an epoch timestamp whenever the Object receives an MQTT message. This can be useful for formulas and checking whether an MQTT message has updated the Object. A formula with 

$OLD{last_mqtt_timestamp}l != ${last_mqtt_timestamp} 

will only fire on MQTT messages. For more information on Formulas, see [Formulas](#heading=h.fyma3do0ledl).

* Note the "l" after ${last_mqtt_timestamp}. This is required because the timestamp is greater than the size of a Java primitive int, and therefore must be cast as a primitive long ([https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)).

### Mqtt State (mqtt_state)

The mqtt_state attribute has two values: **Active** and **Suspended**. By default, the attribute is set to **Active**. When an object has its’ mqtt_state set to **Active**, it will update with messages published to its’ [mqtt_topic](#heading=h.9w1asbkfvnt4). When an object has its’ state set to **Suspended**, it will *not* update with messages published to its’ [mqtt_topic](#heading=h.9w1asbkfvnt4).

* If the attribute is removed from an object, the object will behave the same as if the attribute was set to **Active**. 

### Location Attributes

#### Latitude (latitude) and Longitude (longitude)

Adding string or numerical values for the two attributes, latitude and longitude, will result in a small map to appear on the object detail page. This is useful for conveying object location information at a glance to users.

#### Location Data Type

It is also possible to add multiple location attributes by specifying the data type "Location." All attributes with the “Location” data type will be plotted on a shared map on the object detail page.

# Formulas

## Intro

Formulas are a way to either manipulate an attribute or perform an action after an Object update. For simple calculations like converting Farenhiet to Celius or doing a simple count, formulas can be used to avoid unnecessary configuration on the backend. Formulas can also be used to take action based on defined criteria.

All formulas are run on an object every time the object is updated. Because conditional formulas have the potential to update the object itself, it is important

## Syntax

#### Overview

In general Foundry Formulas follow Java syntax, however there are a few key differences between the two syntaxes. 

#### Variable Lookups

Formulas are able to use attributes of the object as variables in their logic. To reference a variable formulas use the syntax **\${json\_key}** where **json_key** is the attributes JSON Key value.  Formulas are also able to use the previous value of an attribute by using the syntax **$OLD{json_key}**. It is important to note when accessing variables that are strings the **\$\{\}** must be surrounded with quotes.

* Numbers: **\$\{json_key\}**

* Strings: **"${json_key}"**

#### Available Operators

* **!= **: Not Equals (${a} == 1)

* **== **: Equals (${a} != 1)

* **+, -, /, *, % **: Addition, Subtraction, Division, Multiplication, Modulo (${a} + 1)

* **&& **: Bitwise AND (${a} > 0 && ${a} < 5 )

* **|| :** Bitwise OR (${a} < 0 || ${a} >= 5 )

* **Statement ? Value1 : Value2** : Ternary, If Statement is true then Value 1 else Value 2. (${a} < 0 ? "Negative" : “Positive”)

#### Supported Functions

Along with the basic operations, Foundry includes a few built in functions which can be used in Formulas.

<table>
  <tr>
    <td>Function</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>toString(Object arg)</td>
    <td>Converts to string</td>
  </tr>
  <tr>
    <td>timestamp_to_datetime(long ts, [String format], [String timeZone])</td>
    <td>Returns String representation of date time in the specified format. For times in Milliseconds</td>
  </tr>
  <tr>
    <td>epoch_to_datetime(long ts, [String format], [String timeZone])</td>
    <td>Returns String representation of date time in the specified format. For times in Seconds</td>
  </tr>
  <tr>
    <td>epoch()</td>
    <td>Returns current epoch in  milliseconds</td>
  </tr>
  <tr>
    <td>now()</td>
    <td>Returns current time in seconds</td>
  </tr>
  <tr>
    <td>today(String format, [String timeZone])</td>
    <td>Returns Today's date in String in specified format</td>
  </tr>
  <tr>
    <td>base64Encode(String str)</td>
    <td>Returns base64 encoding of any string</td>
  </tr>
  <tr>
    <td>base64Decode(String base64Str)</td>
    <td>Returns decoded string</td>
  </tr>
  <tr>
    <td>equals(String a, String b)</td>
    <td>Returns Boolean - true / false</td>
  </tr>
  <tr>
    <td>executeAction(String actionId)</td>
    <td>Executes an action where actionId is the deviceId of a Global Action.</td>
  </tr>
  <tr>
    <td>All java.lang.Math static methods can be used in formula fields</td>
    <td></td>
  </tr>
  <tr>
    <td>renderColor(\"color\")</td>
    <td>Renders a color block in the field</td>
  </tr>
  <tr>
    <td>renderLink(\"Label String\", \"Link String\")</td>
    <td>Renders the Link</td>
  </tr>
  <tr>
    <td>startsWith(String str1, String str2)</td>
    <td>Returns true if str1 starts with str2</td>
  </tr>
  <tr>
    <td>endsWith(String str1, String str2)</td>
    <td>Returns true if str1 ends with str2</td>
  </tr>
  <tr>
    <td>contains(String str1, String str2)</td>
    <td>Returns true if str1 contains str2</td>
  </tr>
  <tr>
    <td>equalsIgnoreCase(String str1, String str2)</td>
    <td>Returns true if str1 and str2 are identical while ignoring the case</td>
  </tr>
  <tr>
    <td>toLower(String str)</td>
    <td></td>
  </tr>
  <tr>
    <td>toUpper(String str)</td>
    <td></td>
  </tr>
  <tr>
    <td>subString(String str, int beginIndex)</td>
    <td></td>
  </tr>
  <tr>
    <td>subString(String str, int beginIndex, int endIndex)</td>
    <td></td>
  </tr>
  <tr>
    <td>isEmpty(${json_key})</td>
    <td>Returns Boolean - true if there is no value for this attribute, false value if there is a value. (this may only work on string variables and not on Decimal or Integer types)</td>
  </tr>
</table>


#### Special Attributes

* **${Updated} **: Timestamp in milliseconds of when the object was last updated.

* **${Created} **: Timestamp in milliseconds of when the object was created.

* **${deviceId} **: DeviceId of the object.

* **${accountId} **: accountId of the object.

* **${typeId} **: typeId of the object.

* **${updated_by} **: UserId of user who last edited object

#### Using References

Attributes that are of type "Reference" are special because the reference object attribute’s can also be used in the Formula. To use a reference object attribute’s in a formula use the syntax:

**${ref_json_key__r.json_key}**

Where ref_json_key is the Json key of the attribute that is a reference. And json_key is the Json key of the attribute on the referenced Object. It’s also important to note that there are TWO underscores in the syntax.

## Conditional

Conditional formulas are used for firing Global Actions based on a series of if-elseif-statements. Typically, regular formulas can be constructed with one of the following types of formulas:

### Run every time

* 1 == 1

* true **(untested)**

* Only use when **Run Only On Object Creation** is checked.

### Run when an attribute has changed

* $OLD{last_mqtt_timestamp}l != ${last_mqtt_timestamp}l

    * Checking if a numerical attribute has changed

* !equals("$OLD{username}", “${username}”)

    * Checking if a string attribute has changed

* !equals("$OLD{temperature}", “${temperature}”)

    * Checking if a numerical attribute has changed by first casting it as string

### Run when an attribute has changed and some other criteria

* $OLD{last_mqtt_timestamp}l != ${last_mqtt_timestamp}l && ${temperature} > 30

* $OLD{moisture} != ${moisture} && ${moisture} > 30

* !equals("\$OLD{username}", “\${username}”) \&\& equals(“${date}”, “Monday”)

* !equals("\$OLD{status}", “\${status}”) \&\& equals(“${status}”, “Processed”)

## Regular

Regular formulas are used for either transforming existing attribute values, or generating entirely new attribute values. Typically, regular formulas can be constructed with one of the following types of formulas:

* Transform an existing attribute value

    * (${temperature_f} - 32) / 1.8

    * ${sensor__r.moisture} + 50

* Generate a new attribute value

    * ${temperature_f} > 90 ? "fire.png" : “ice.png”

## Reference

Formulas can take advantage of attributes on a [reference](#heading=h.3cjv5d564hpj) to other objects in order to perform more advanced calculations. 

To use an attribute on Object B in a formula on Object A:

1. Create a reference attribute from Object A to Object B

2. Create formula on Object A’s Object Type:

    1. ${object_b_reference__r.temperature}

The reference syntax follows regular formula syntax with one key distinction: **__r** must be appended to the end of the attribute json key. *Note that there are two underscores*.

# Foundry Actions

## Intro

Foundry Actions are what define the logic of your application. Foundry Actions store a payload with an action associated to it. These actions can be run via formula fields, ran manually, or scheduled. Each action needs to be created in a specific way which is outlined here. 

## Types

There are 3 different types of actions that Foundry Actions can do:

* Publish to an MQTT topic

* Invoke a Lambda function 

* Make an HTTP request

### Mqtt Publish

#### What it does

Publishes the **payload** to the topic specified on the **mqtt_publish_topic** attribute

#### Required Attributes

* mqtt_publish_topic

* payload 

* action_type

*Make sure **action_type ** is set to **Mqtt Publish**

### Lambda Invoke

#### What it does

Sends the **payload** to a lambda function specified on the **lambda_arn** field. 

#### Required Attributes

* lambda_arn

* payload 

* action_type

*Make sure **action_type** is set to **Lambda Invoke**

### HTTP Request

#### What it does

Sends the **payload** to any HTTP request. You can specify a GET, POST, PUT, or DELETE operation.

#### Required Attributes

* http_url

* payload 

* action_type

* http_headers (optional)

*Make sure **action_type** is set to **HTTP {type}** where {type} is either GET, PUT, DELETE, POST.  If certain headers are required, fill those in on the **http_headers** field.

## Invoking/Scheduling

Foundry Actions are run on individual objects. Data from each object can be used in the payload of the action to perform logic on. There are 3 ways to invoke a Foundry Action:

* Via [Formulas](#heading=h.bce3b56l4ral)

* On an object detail page 

* On search results

### Object Detail

If the [Object Type is configured for Foundry Actions](#heading=h.1lfsmy6eb0ns), actions can be invoked on Objects of that type. Invoke actions configured for an object type by doing the following: 

1. Navigate to the Object where the Action is to be invoked. This can be done through the Object Manager

2. The Action Invoker will appear above the attributes. Select the Action to invoke and click **Invoke or Schedule**

![image alt text](image_10.png)

### Search Results

Actions can also be run on ad-hoc search results with no prior configuration. 

1. Navigate to the Object Manager.

2. [Perform a search on the Objects to invoke the Action on](#heading=h.2i87hyktqj24). 

3. An Action Invoker will appear over the search results. Select the Action to invoke and click **Invoke** or **Schedule**.

![image alt text](image_11.png)

### Formulas

Foundry Actions can also be invoked via [Formulas](#heading=h.bce3b56l4ral). This is useful for automating MQTT publishes and running lambda functions based on other events in a Foundry solution. 

To use Foundry Actions in a formula, use the *executeAction() *function [outlined here in the Formulas documentation](#heading=h.bce3b56l4ral). 

## The Payload Structure

The most important part of a Foundry Action is the payload. This is what defines what data to use in the Action. 

The payload is the field **payload** and is required on all Foundry Actions. It is what is passed into Lambda functions, published on MQTT topics, and what is sent to HTTP requests. 

The payload can be defined with whatever parameters are needed. In addition, fields on the Object the Action is run against are available for the Action to use. The syntax used for accessing attributes on Objects is [json_key]. For example, if there is an Object with fields: 

* Make: Ford

* Model: Explorer

Then the Action can use these values by specifying the payload like this: 

{

   "vehicleMake": “[make]”,

   "vehicleModel": “[model]”,

   "nameOfCar": “Test Car”

}

The payload that gets passed into the Action would result in: 

{

   "vehicleMake": “Ford”,

   "vehicleModel": “Explorer”,

   "nameOfCar": “Test Car”

}

## Creating a Foundry Action 

Foundry Actions are created in the same manner as [creating Objects](#heading=h.k078ozzbg2eu). 

1. Create an Object with the Object Type **Global Action**.

2. Add all the required attributes for the type of action needed. Here is where to find the required attributes per type: 
- [MQTT Publish](#heading=h.emyuhi3l0uv8)
- [Lambda Invoke](#heading=h.rmm2g21v3hz)
- [HTTP Request](#heading=h.jyupzk88kbn0)

# Accounts

## Intro

Foundry Accounts serve as a way to group together Foundry Users and other Foundry resources into logical hierarchies. Foundry accounts can have a single parent and one or multiple children, allowing for a wide range of account hierarchies.

## Hierarchy/Visibility

Every Foundry account has a single parent account, and in some cases, many child accounts. By default, users in a Foundry account have full access to the Foundry resources in their account and all accounts below them, but **not **to the accounts above them or on separate hierarchy branches. This downward-looking visibility model ensures that users further down in the account hierarchy cannot access resources out of their control. This visibility model can be changed with **Foundry ACLs.**

## ACL

Foundry Account Control Lists (ACL) allow users to grant specific accounts or users access to perform operations in their account that violate the traditional account visibility model.

ACLs can modify the ability to:

* Read, write, and delete Objects

* Read, write, and delete Users

To create an ACL from Account A to Account B that grants all Account B Users access to **write objects** in Account A:

1. Navigate to Account A.

2. Click the **ACL** tab and then click **Create New ACL**.

3. Select Account B from the **Grantee Account** dropdown.

4. Check the **Write** checkbox from the list of **Permissions to Objects**.

5. Click **Add**.


To create an ACL from Account A to Account B that grants a specific User in Account B access** to read users** in Account A:

6. Navigate to Account A.

7. Click the **ACL** tab and then click **Create New ACL.**

8. Select Account B’s user from the **Grantee User** dropdown.

9. Check the **Read** checkbox from the list of **Permissions to Users**.

10. Click **Add**.

Checking the **Inherit** slider at time of ACL creation or updating will apply that ACL to all sub-accounts of the selected **Grantee Account.**

# Users

## Intro

Within an account, unlimited users can be created. Each user must have a unique username and email combination. Without a user, it is impossible to perform Foundry operations.

## Role

Users must be assigned a role which designates which pages and operations they have access to. For configuring page roles, see: [Page User Roles](#heading=h.41koj43idg9g).

Each user can be assigned one of three roles:

* Admin

* User

* View Only

### Admin

Users with the Admin role have access to all pages and can perform all Foundry operations.

### Users

Users with the User role have access to all pages that have been designated with the role User or View Only. Users with the User role can perform all Foundry operations on the pages they’ve been granted access to.

### View Only

Users with the User role have access to all pages that have been designated with the role View Only. Users with the View Only role cannot modify any Foundry resources.

## Policy

* Policy can be attached to user (see policy section)

* Screenshots for how to attach

# Search

## Advanced Search

## Intro

Advanced Search is one of three object search solutions in Foundry. Advanced Search can be used to search all objects spanning across many accounts based on a variety of search criteria.

## Search Criteria 

### Account

Searches for objects that belong to a particular account. Note that just because an object is accessible by an account does not mean it belongs to the account.

### Object Type

Searches for objects that have a particular Object Type.

### Object Type Attribute

Searches for objects that have a particular attribute value. When building this criteria, Foundry separates the attributes by Object Type. 

Note, however, that running a search only using this criteria will not necessarily return objects of a single Object Type. For example, running a search after selecting an attribute that is shared across many object types such as "name", may return objects from many object types. To guarantee objects from only a single object type, pair this criteria with the Object Type criteria.

### Created

Searches for objects created on a particular date or time.

### Updated

Searches for objects updated on a particular date or time.

### Created By

Searches for objects created by a particular user.

### Updated By

Searches for objects updated by a particular user. Note that updates performed through non-api requests (Mqtt Messages) do not register a change in the Updated By field.

## Caveats

Though the Advanced Search excels in providing great search flexibility, it struggles to scale in medium to large Foundry instances. Because of this, Advanced Search is not recommended outside of early stage Foundry instances. Advanced Search will likely be deprecated in favor of Object Search (Link).

## Object Search

## Intro

Object Search is the second of three object search solutions in Foundry. Object search can be used to search all **indexed** objects using a similar set of search criteria as Advanced Search.

## Indexing Objects

All Foundry objects are automatically indexed by Account and Object Type. This means that all objects can be searched on these criteria with no additional configuration.

To index objects based on other attributes, an index must first be created on the object type for the objects you want to search (link). An index is constructed of a set of attributes that will be used to search on. Though there is no limit on the number of indexes that can be created, indexes should be created with careful thought. When using an index to search, all attributes must be given exact values, not ranges.

The following attributes would make for a **bad** index:

<table>
  <tr>
    <td>Attribute</td>
    <td>Sample Values</td>
  </tr>
  <tr>
    <td>Temperature</td>
    <td>40.5, 47.1, 20.3, 60.23</td>
  </tr>
  <tr>
    <td>Humidity</td>
    <td>100.3, 99.4, 90.5, 87.1</td>
  </tr>
</table>


The following attributes would make for a **good **index

<table>
  <tr>
    <td>Attribute</td>
    <td>Sample Values</td>
  </tr>
  <tr>
    <td>Dealer</td>
    <td>Honda, Toyota, Nissan</td>
  </tr>
  <tr>
    <td>Color</td>
    <td>Red, Blue, Silver</td>
  </tr>
</table>


Index attributes should generally have a small, predictable, range of values. This will keep the index performant, and lead to more useful searches.

## Index Options

### Object Type

Searches for objects using the object type index.

### Account

Searches for objects using the account index.

### Index

Searches for objects using the user defined indexes.

## Filtering

Though index values are limited in scope, filters provide an additional level of flexibility for searching. The same search criteria available to Advanced Search (link) are available as filters for Object Search

## Caveats

Though Object Search is significantly more performant than Advanced Search, especially at scale, search results must be manually paged through. It is not possible to search for values across multiple pages, so it is recommended to narrow the search to make the result set as small as possible.

After an index has been created, existing objects will not belong to this index until they have been updated. Newly created objects will automatically be placed in the index.

## Global Search

Global searches look up objects based on their name. To use the search use the Object Search in the upper right of every page.

![image alt text](image_12.png)

* Run searches on object names

### Running Search

* **Screenshots** of building a search

## Saved Searches

Both Advanced Search and Object Search can be saved to create a Saved Search. Saved searches can be used to provide quick access to frequently used search criteria, create pages and reports, and create dashboards.

Create a saved search by performing the following:

1. Construct desired search criteria

2. Click Save Search

3. Click Save

### Save Search Options

#### Show Criteria

Checking this box will reveal the criteria menu on pages created from saved searches, allowing for quick modifications to the search.

#### Show Actions

Checking this box will reveal the action invoker menu on pages created from saved searches, allowing Foundry Actions (link) to be run on saved search results.

# Workflow Manager

## Intro

Foundry Workflows are collections of Foundry Actions that can be invoked similarly to Foundry Actions. When a Foundry Workflow is invoked, however, it fires all of the actions in it — either asynchronously or synchronously depending on how the workflow is configured. Additionally, when running in synchronous mode, the workflow can be configured to stop when an action fails.

## Building a Workflow

Workflows can be created using the Workflow Manager. To create a new workflow, perform the following steps:

1. Navigate to the Workflow Manager

2. Click "Create New Workflow"

3. On the workflow details screen, click add action

4. Select the action to add to the workflow

5. Repeat action adding process as many times as desired

When running in synchronous mode, actions within a workflow can be rearranged by dragging the action to its desired location.

## Running Workflows

Behind the scenes, a workflow is just an Object Group (link) of Global Actions. Since Object Groups are Objects themselves, Workflows can be invoked in all the same ways Global Actions can be (Link). When a workflow is invoked, it will fire all of its actions synchronously or asynchronously, depending on how the workflow is configured.

A workflow’s status can be monitored by viewing the workflow in the workflow manager. Things like invocation and error counts can be viewed in real time for each action in the workflow. (Screenshot)

# Api Reference

[https://s3-us-west-2.amazonaws.com/thinglogix-foundry-swagger/latest/dist/index.html](https://s3-us-west-2.amazonaws.com/thinglogix-foundry-swagger/latest/dist/index.html)