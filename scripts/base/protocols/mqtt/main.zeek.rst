:tocdepth: 3

base/protocols/mqtt/main.zeek
=============================
.. zeek:namespace:: MQTT

Implements base functionality for MQTT (v3.1.1) analysis.
Generates the mqtt.log file.

:Namespace: MQTT
:Imports: :doc:`base/protocols/mqtt/consts.zeek </scripts/base/protocols/mqtt/consts.zeek>`

Summary
~~~~~~~
Types
#####
================================================================== =
:zeek:type:`MQTT::ConnectInfo`: :zeek:type:`record`                
:zeek:type:`MQTT::PublishInfo`: :zeek:type:`record`                
:zeek:type:`MQTT::SubUnsub`: :zeek:type:`enum` :zeek:attr:`&redef` 
:zeek:type:`MQTT::SubscribeInfo`: :zeek:type:`record`              
================================================================== =

Redefinitions
#############
==================================================================== =
:zeek:type:`Log::ID`: :zeek:type:`enum`                              
:zeek:type:`connection`: :zeek:type:`record`                         
:zeek:id:`likely_server_ports`: :zeek:type:`set` :zeek:attr:`&redef` 
==================================================================== =

Events
######
============================================= ====================================================================
:zeek:id:`MQTT::log_mqtt`: :zeek:type:`event` Event that can be handled to access the MQTT record as it is sent on
                                              to the logging framework.
============================================= ====================================================================


Detailed Interface
~~~~~~~~~~~~~~~~~~
Types
#####
.. zeek:type:: MQTT::ConnectInfo

   :Type: :zeek:type:`record`

      ts: :zeek:type:`time` :zeek:attr:`&log`
         Timestamp for when the event happened

      uid: :zeek:type:`string` :zeek:attr:`&log`
         Unique ID for the connection

      id: :zeek:type:`conn_id` :zeek:attr:`&log`
         The connection's 4-tuple of endpoint addresses/ports

      proto_name: :zeek:type:`string` :zeek:attr:`&log` :zeek:attr:`&optional`
         Indicates the protocol name

      proto_version: :zeek:type:`string` :zeek:attr:`&log` :zeek:attr:`&optional`
         The version of the protocol in use

      client_id: :zeek:type:`string` :zeek:attr:`&log` :zeek:attr:`&optional`
         Unique identifier for the client

      connect_status: :zeek:type:`string` :zeek:attr:`&log` :zeek:attr:`&optional`
         Status message from the server in response to the connect request

      will_topic: :zeek:type:`string` :zeek:attr:`&log` :zeek:attr:`&optional`
         Topic to publish a "last will and testament" message to

      will_payload: :zeek:type:`string` :zeek:attr:`&log` :zeek:attr:`&optional`
         Payload to publish as a "last will and testament"


.. zeek:type:: MQTT::PublishInfo

   :Type: :zeek:type:`record`

      ts: :zeek:type:`time` :zeek:attr:`&log`
         Timestamp for when the publish message started

      uid: :zeek:type:`string` :zeek:attr:`&log`
         UID for the connection

      id: :zeek:type:`conn_id` :zeek:attr:`&log`
         ID fields for the connection

      from_client: :zeek:type:`bool` :zeek:attr:`&log`
         Indicates if the message was published by the client of
         this connection or published to the client.

      retain: :zeek:type:`bool` :zeek:attr:`&log`
         Indicates if the message was to be retained by the server

      qos: :zeek:type:`string` :zeek:attr:`&log`
         QoS level set for the message

      status: :zeek:type:`string` :zeek:attr:`&log` :zeek:attr:`&default` = ``"incomplete_qos"`` :zeek:attr:`&optional`
         Status of the published message. This will be set to "incomplete_qos"
         if the full back and forth for the requested level of QoS was not seen.
         Otherwise if it's successful the field will be "ok".

      topic: :zeek:type:`string` :zeek:attr:`&log`
         Topic the message was published to

      payload: :zeek:type:`string` :zeek:attr:`&log`
         Payload of the message

      ack: :zeek:type:`bool` :zeek:attr:`&default` = ``F`` :zeek:attr:`&optional`
         Track if the message was acked

      rec: :zeek:type:`bool` :zeek:attr:`&default` = ``F`` :zeek:attr:`&optional`
         Indicates if the server sent the RECEIVED qos message

      rel: :zeek:type:`bool` :zeek:attr:`&default` = ``F`` :zeek:attr:`&optional`
         Indicates if the client sent the RELEASE qos message

      comp: :zeek:type:`bool` :zeek:attr:`&default` = ``F`` :zeek:attr:`&optional`
         Indicates if the server sent the COMPLETE qos message

      qos_level: :zeek:type:`count` :zeek:attr:`&default` = ``0`` :zeek:attr:`&optional`
         Internally used for comparing numeric qos level


.. zeek:type:: MQTT::SubUnsub

   :Type: :zeek:type:`enum`

      .. zeek:enum:: MQTT::SUBSCRIBE MQTT::SubUnsub

      .. zeek:enum:: MQTT::UNSUBSCRIBE MQTT::SubUnsub
   :Attributes: :zeek:attr:`&redef`


.. zeek:type:: MQTT::SubscribeInfo

   :Type: :zeek:type:`record`

      ts: :zeek:type:`time` :zeek:attr:`&log`
         Timestamp for when the subscribe or unsubscribe request started

      uid: :zeek:type:`string` :zeek:attr:`&log`
         UID for the connection

      id: :zeek:type:`conn_id` :zeek:attr:`&log`
         ID fields for the connection

      action: :zeek:type:`MQTT::SubUnsub` :zeek:attr:`&log`
         Indicates if a subscribe or unsubscribe action is taking place

      topic: :zeek:type:`string` :zeek:attr:`&log`
         The topic (or topic pattern) being subscribed to

      qos_level: :zeek:type:`count` :zeek:attr:`&log` :zeek:attr:`&optional`
         QoS level requested for messages from subscribed topics

      granted_qos_level: :zeek:type:`count` :zeek:attr:`&log` :zeek:attr:`&optional`
         QoS level the server granted

      ack: :zeek:type:`bool` :zeek:attr:`&log` :zeek:attr:`&default` = ``F`` :zeek:attr:`&optional`
         Indicates if the request was acked by the server


Events
######
.. zeek:id:: MQTT::log_mqtt

   :Type: :zeek:type:`event` (rec: :zeek:type:`MQTT::ConnectInfo`)

   Event that can be handled to access the MQTT record as it is sent on
   to the logging framework.

