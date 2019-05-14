:tocdepth: 3

policy/frameworks/packet-filter/shunt.zeek
==========================================
.. zeek:namespace:: PacketFilter


:Namespace: PacketFilter
:Imports: :doc:`base/frameworks/notice </scripts/base/frameworks/notice/index>`, :doc:`base/frameworks/packet-filter </scripts/base/frameworks/packet-filter/index>`

Summary
~~~~~~~
Redefinable Options
###################
=============================================================================== ======================================================================
:zeek:id:`PacketFilter::max_bpf_shunts`: :zeek:type:`count` :zeek:attr:`&redef` The maximum number of BPF based shunts that Bro is allowed to perform.
=============================================================================== ======================================================================

Redefinitions
#############
============================================ =
:zeek:type:`Notice::Type`: :zeek:type:`enum` 
============================================ =

Functions
#########
========================================================================== ============================================================================
:zeek:id:`PacketFilter::current_shunted_conns`: :zeek:type:`function`      Retrieve the currently shunted connections.
:zeek:id:`PacketFilter::current_shunted_host_pairs`: :zeek:type:`function` Retrieve the currently shunted host pairs.
:zeek:id:`PacketFilter::force_unshunt_host_pair`: :zeek:type:`function`    Performs the same function as the :zeek:id:`PacketFilter::unshunt_host_pair`
                                                                           function, but it forces an immediate filter update.
:zeek:id:`PacketFilter::shunt_conn`: :zeek:type:`function`                 Call this function to use BPF to shunt a connection (to prevent the
                                                                           data packets from reaching Bro).
:zeek:id:`PacketFilter::shunt_host_pair`: :zeek:type:`function`            This function will use a BPF expression to shunt traffic between
                                                                           the two hosts given in the `conn_id` so that the traffic is never
                                                                           exposed to Bro's traffic processing.
:zeek:id:`PacketFilter::unshunt_host_pair`: :zeek:type:`function`          Remove shunting for a host pair given as a `conn_id`.
========================================================================== ============================================================================


Detailed Interface
~~~~~~~~~~~~~~~~~~
Redefinable Options
###################
.. zeek:id:: PacketFilter::max_bpf_shunts

   :Type: :zeek:type:`count`
   :Attributes: :zeek:attr:`&redef`
   :Default: ``100``

   The maximum number of BPF based shunts that Bro is allowed to perform.

Functions
#########
.. zeek:id:: PacketFilter::current_shunted_conns

   :Type: :zeek:type:`function` () : :zeek:type:`set` [:zeek:type:`conn_id`]

   Retrieve the currently shunted connections.

.. zeek:id:: PacketFilter::current_shunted_host_pairs

   :Type: :zeek:type:`function` () : :zeek:type:`set` [:zeek:type:`conn_id`]

   Retrieve the currently shunted host pairs.

.. zeek:id:: PacketFilter::force_unshunt_host_pair

   :Type: :zeek:type:`function` (id: :zeek:type:`conn_id`) : :zeek:type:`bool`

   Performs the same function as the :zeek:id:`PacketFilter::unshunt_host_pair`
   function, but it forces an immediate filter update.

.. zeek:id:: PacketFilter::shunt_conn

   :Type: :zeek:type:`function` (id: :zeek:type:`conn_id`) : :zeek:type:`bool`

   Call this function to use BPF to shunt a connection (to prevent the
   data packets from reaching Bro).  For TCP connections, control
   packets are still allowed through so that Bro can continue logging
   the connection and it can stop shunting once the connection ends.

.. zeek:id:: PacketFilter::shunt_host_pair

   :Type: :zeek:type:`function` (id: :zeek:type:`conn_id`) : :zeek:type:`bool`

   This function will use a BPF expression to shunt traffic between
   the two hosts given in the `conn_id` so that the traffic is never
   exposed to Bro's traffic processing.

.. zeek:id:: PacketFilter::unshunt_host_pair

   :Type: :zeek:type:`function` (id: :zeek:type:`conn_id`) : :zeek:type:`bool`

   Remove shunting for a host pair given as a `conn_id`.  The filter
   is not immediately removed.  It waits for the occasional filter
   update done by the `PacketFilter` framework.

