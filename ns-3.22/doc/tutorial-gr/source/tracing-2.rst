Trace Sinks
~~~~~~~~~~~

The whole point of this exercise is to get trace callbacks from TCP
indicating the congestion window has been updated.  The next piece of
code implements the corresponding trace sink::

  static void
  CwndChange (uint32_t oldCwnd, uint32_t newCwnd)
  {
    NS_LOG_UNCOND (Simulator::Now ().GetSeconds () << "\t" << newCwnd);
  }

This should be very familiar to you now, so we won't dwell on the
details.  This function just logs the current simulation time and the
new value of the congestion window every time it is changed.  You can
probably imagine that you could load the resulting output into a
graphics program (gnuplot or Excel) and immediately see a nice graph
of the congestion window behavior over time.

We added a new trace sink to show where packets are dropped.  We are
going to add an error model to this code also, so we wanted to
demonstrate this working.

::

  static void
  RxDrop (Ptr<const Packet> p)
  {
    NS_LOG_UNCOND ("RxDrop at " << Simulator::Now ().GetSeconds ());
  }

This trace sink will be connected to the "PhyRxDrop" trace source of
the point-to-point NetDevice.  This trace source fires when a packet
is dropped by the physical layer of a ``NetDevice``.  If you take a
small detour to the source
(``src/point-to-point/model/point-to-point-net-device.cc``) you will
see that this trace source refers to
``PointToPointNetDevice::m_phyRxDropTrace``.  If you then look in
``src/point-to-point/model/point-to-point-net-device.h`` for this
member variable, you will find that it is declared as a
``TracedCallback<Ptr<const Packet> >``.  This should tell you that the
callback target should be a function that returns void and takes a
single parameter which is a ``Ptr<const Packet>`` (assuming we use
``ConnectWithoutContext``) -- just what we have above.

Main Program
~~~~~~~~~~~~

The following code should be very familiar to you by now::

  int
  main (int argc, char *argv[])
  {
    NodeContainer nodes;
    nodes.Create (2);
  
    PointToPointHelper pointToPoint;
    pointToPoint.SetDeviceAttribute ("DataRate", StringValue ("5Mbps"));
    pointToPoint.SetChannelAttribute ("Delay", StringValue ("2ms"));
  
    NetDeviceContainer devices;
    devices = pointToPoint.Install (nodes);

This creates two nodes with a point-to-point channel between them,
just as shown in the illustration at the start of the file.

The next few lines of code show something new.  If we trace a
connection that behaves perfectly, we will end up with a monotonically
increasing congestion window.  To see any interesting behavior, we
really want to introduce link errors which will drop packets, cause
duplicate ACKs and trigger the more interesting behaviors of the
congestion window.

|ns3| provides ``ErrorModel`` objects which can be attached to
``Channels``.  We are using the ``RateErrorModel`` which allows us to
introduce errors
into a ``Channel`` at a given *rate*. 

::

  Ptr<RateErrorModel> em = CreateObject<RateErrorModel> ();
  em->SetAttribute ("ErrorRate", DoubleValue (0.00001));
  devices.Get (1)->SetAttribute ("ReceiveErrorModel", PointerValue (em));

The above code instantiates a ``RateErrorModel`` Object, and we set
the "ErrorRate" ``Attribute`` to the desired value.  We then set the
resulting instantiated ``RateErrorModel`` as the error model used by
the point-to-point ``NetDevice``.  This will give us some
retransmissions and make our plot a little more interesting.

::

  InternetStackHelper stack;
  stack.Install (nodes);

  Ipv4AddressHelper address;
  address.SetBase ("10.1.1.0", "255.255.255.252");
  Ipv4InterfaceContainer interfaces = address.Assign (devices);

The above code should be familiar.  It installs internet stacks on our
two nodes and creates interfaces and assigns IP addresses for the
point-to-point devices.

Since we are using TCP, we need something on the destination Node to
receive TCP connections and data.  The ``PacketSink`` ``Application``
is commonly used in |ns3| for that purpose.

::

  uint16_t sinkPort = 8080;
  Address sinkAddress (InetSocketAddress(interfaces.GetAddress (1), sinkPort));
  PacketSinkHelper packetSinkHelper ("ns3::TcpSocketFactory", 
    InetSocketAddress (Ipv4Address::GetAny (), sinkPort));
  ApplicationContainer sinkApps = packetSinkHelper.Install (nodes.Get (1));
  sinkApps.Start (Seconds (0.));
  sinkApps.Stop (Seconds (20.));

This should all be familiar, with the exception of,

::

  PacketSinkHelper packetSinkHelper ("ns3::TcpSocketFactory", 
    InetSocketAddress (Ipv4Address::GetAny (), sinkPort));

This code instantiates a ``PacketSinkHelper`` and tells it to create
sockets using the class ``ns3::TcpSocketFactory``.  This class
implements a design pattern called "object factory" which is a
commonly used mechanism for specifying a class used to create objects
in an abstract way.  Here, instead of having to create the objects
themselves, you provide the ``PacketSinkHelper`` a string that
specifies a ``TypeId`` string used to create an object which can then
be used, in turn, to create instances of the Objects created by the
factory.

The remaining parameter tells the ``Application`` which address and
port it should ``Bind`` to.

The next two lines of code will create the socket and connect the
trace source.

::

  Ptr<Socket> ns3TcpSocket = Socket::CreateSocket (nodes.Get (0), 
    TcpSocketFactory::GetTypeId ());
  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", 
    MakeCallback (&CwndChange));

The first statement calls the static member function
``Socket::CreateSocket`` and provides a Node and an explicit
``TypeId`` for the object factory used to create the socket.  This is
a slightly lower level call than the ``PacketSinkHelper`` call above,
and uses an explicit C++ type instead of one referred to by a string.
Otherwise, it is conceptually the same thing.

Once the ``TcpSocket`` is created and attached to the Node, we can
use ``TraceConnectWithoutContext`` to connect the CongestionWindow
trace source to our trace sink.

Recall that we coded an ``Application`` so we could take that
``Socket`` we just made (during configuration time) and use it in
simulation time.  We now have to instantiate that ``Application``.  We
didn't go to any trouble to create a helper to manage the
``Application`` so we are going to have to create and install it
"manually".  This is actually quite easy::

  Ptr<MyApp> app = CreateObject<MyApp> ();
  app->Setup (ns3TcpSocket, sinkAddress, 1040, 1000, DataRate ("1Mbps"));
  nodes.Get (0)->AddApplication (app);
  app->Start (Seconds (1.));
  app->Stop (Seconds (20.));

The first line creates an ``Object`` of type ``MyApp`` -- our
``Application``.  The second line tells the ``Application`` what
``Socket`` to use, what address to connect to, how much data to send
at each send event, how many send events to generate and the rate at
which to produce data from those events.

Next, we manually add the ``MyApp Application`` to the source Node and
explicitly call the ``Start`` and ``Stop`` methods on the
``Application`` to tell it when to start and stop doing its thing.

We need to actually do the connect from the receiver point-to-point
``NetDevice`` drop event to our ``RxDrop`` callback now.

::

  devices.Get (1)->TraceConnectWithoutContext("PhyRxDrop", MakeCallback (&RxDrop));

It should now be obvious that we are getting a reference to the
receiving ``Node NetDevice`` from its container and connecting the
trace source defined by the attribute "PhyRxDrop" on that device to
the trace sink ``RxDrop``.

Finally, we tell the simulator to override any ``Applications`` and
just stop processing events at 20 seconds into the simulation.

::

    Simulator::Stop (Seconds(20));
    Simulator::Run ();
    Simulator::Destroy ();

    return 0;
  }

Recall that as soon as ``Simulator::Run`` is called, configuration
time ends, and simulation time begins.  All of the work we
orchestrated by creating the ``Application`` and teaching it how to
connect and send data actually happens during this function call.

As soon as ``Simulator::Run`` returns, the simulation is complete and
we enter the teardown phase.  In this case, ``Simulator::Destroy``
takes care of the gory details and we just return a success code after
it completes.

Running ``fifth.cc``
++++++++++++++++++++

Since we have provided the file ``fifth.cc`` for you, if you have
built your distribution (in debug mode since it uses ``NS_LOG`` -- recall
that optimized builds optimize out ``NS_LOG``) it will be waiting for you
to run.

.. sourcecode:: bash

  $ ./waf --run fifth
  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone-dev/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone-dev/ns-3-dev/build'
  'build' finished successfully (0.684s)
  1       536
  1.0093  1072
  1.01528 1608
  1.02167 2144
  ...
  1.11319 8040
  1.12151 8576
  1.12983 9112
  RxDrop at 1.13696
  ...

You can probably see immediately a downside of using prints of any
kind in your traces.  We get those extraneous waf messages printed all
over our interesting information along with those RxDrop messages.  We
will remedy that soon, but I'm sure you can't wait to see the results
of all of this work.  Let's redirect that output to a file called
``cwnd.dat``:

.. sourcecode:: bash

  $ ./waf --run fifth > cwnd.dat 2>&1

Now edit up "cwnd.dat" in your favorite editor and remove the waf
build status and drop lines, leaving only the traced data (you could
also comment out the ``TraceConnectWithoutContext("PhyRxDrop",
MakeCallback (&RxDrop));`` in the script to get rid of the drop prints
just as easily.

You can now run gnuplot (if you have it installed) and tell it to
generate some pretty pictures:

.. sourcecode:: bash

  $ gnuplot
  gnuplot> set terminal png size 640,480
  gnuplot> set output "cwnd.png"
  gnuplot> plot "cwnd.dat" using 1:2 title 'Congestion Window' with linespoints
  gnuplot> exit

You should now have a graph of the congestion window versus time
sitting in the file "cwnd.png" that looks like:

.. figure:: figures/cwnd.png

Using Mid-Level Helpers
+++++++++++++++++++++++

In the previous section, we showed how to hook a trace source and get
hopefully interesting information out of a simulation.  Perhaps you
will recall that we called logging to the standard output using
``std::cout`` a "blunt instrument" much earlier in this chapter.  We
also wrote about how it was a problem having to parse the log output
in order to isolate interesting information.  It may have occurred to
you that we just spent a lot of time implementing an example that
exhibits all of the problems we purport to fix with the |ns3| tracing
system!  You would be correct.  But, bear with us.  We're not done
yet.

One of the most important things we want to do is to is to have the
ability to easily control the amount of output coming out of the
simulation; and we also want to save those data to a file so we can
refer back to it later.  We can use the mid-level trace helpers
provided in |ns3| to do just that and complete the picture.

We provide a script that writes the cwnd change and drop events
developed in the example ``fifth.cc`` to disk in separate files.  The
cwnd changes are stored as a tab-separated ASCII file and the drop
events are stored in a PCAP file.  The changes to make this happen are
quite small.

Walkthrough: ``sixth.cc``
~~~~~~~~~~~~~~~~~~~~~~~~~

Let's take a look at the changes required to go from ``fifth.cc`` to
``sixth.cc``.  Open ``examples/tutorial/sixth.cc`` in your favorite
editor.  You can see the first change by searching for CwndChange.
You will find that we have changed the signatures for the trace sinks
and have added a single line to each sink that writes the traced
information to a stream representing a file.

::

  static void
  CwndChange (Ptr<OutputStreamWrapper> stream, uint32_t oldCwnd, uint32_t newCwnd)
  {
    NS_LOG_UNCOND (Simulator::Now ().GetSeconds () << "\t" << newCwnd);
    *stream->GetStream () << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;
  }
  
  static void
  RxDrop (Ptr<PcapFileWrapper> file, Ptr<const Packet> p)
  {
    NS_LOG_UNCOND ("RxDrop at " << Simulator::Now ().GetSeconds ());
    file->Write(Simulator::Now(), p);
  }

We have added a "stream" parameter to the ``CwndChange`` trace sink.
This is an object that holds (keeps safely alive) a C++ output stream.
It turns out that this is a very simple object, but one that manages
lifetime issues for the stream and solves a problem that even
experienced C++ users run into.  It turns out that the copy
constructor for ``std::ostream`` is marked private.  This means that
``std::ostreams`` do not obey value semantics and cannot be used in
any mechanism that requires the stream to be copied.  This includes
the |ns3| callback system, which as you may recall, requires objects
that obey value semantics.  Further notice that we have added the
following line in the ``CwndChange`` trace sink implementation::

  *stream->GetStream () << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;

This would be very familiar code if you replaced ``*stream->GetStream
()`` with ``std::cout``, as in::

  std::cout << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;

This illustrates that the ``Ptr<OutputStreamWrapper>`` is really just
carrying around a ``std::ofstream`` for you, and you can use it here
like any other output stream.

A similar situation happens in ``RxDrop`` except that the object being
passed around (a ``Ptr<PcapFileWrapper>``) represents a PCAP file.
There is a one-liner in the trace sink to write a timestamp and the
contents of the packet being dropped to the PCAP file::

  file->Write(Simulator::Now(), p);

Of course, if we have objects representing the two files, we need to
create them somewhere and also cause them to be passed to the trace
sinks.  If you look in the ``main`` function, you will find new code
to do just that::

  AsciiTraceHelper asciiTraceHelper;
  Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("sixth.cwnd");
  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", MakeBoundCallback (&CwndChange, stream));

  ...

  PcapHelper pcapHelper;
  Ptr<PcapFileWrapper> file = pcapHelper.CreateFile ("sixth.pcap", std::ios::out, PcapHelper::DLT_PPP);
  devices.Get (1)->TraceConnectWithoutContext("PhyRxDrop", MakeBoundCallback (&RxDrop, file));

In the first section of the code snippet above, we are creating the
ASCII trace file, creating an object responsible for managing it and
using a variant of the callback creation function to arrange for the
object to be passed to the sink.  Our ASCII trace helpers provide a
rich set of functions to make using text (ASCII) files easy.  We are
just going to illustrate the use of the file stream creation function
here.

The ``CreateFileStream`` function is basically going to instantiate
a ``std::ofstream`` object and create a new file (or truncate an existing
file).  This ``std::ofstream`` is packaged up in an |ns3| object for lifetime
management and copy constructor issue resolution.

We then take this |ns3| object representing the file and pass it to
``MakeBoundCallback()``.  This function creates a callback just like
``MakeCallback()``, but it "binds" a new value to the callback.  This
value is added as the first argument to the callback before it is called.

Essentially, ``MakeBoundCallback(&CwndChange, stream)`` causes the
trace source to add the additional "stream" parameter to the front of
the formal parameter list before invoking the callback.  This changes
the required signature of the ``CwndChange`` sink to match the one
shown above, which includes the "extra" parameter
``Ptr<OutputStreamWrapper> stream``.

In the second section of code in the snippet above, we instantiate a
``PcapHelper`` to do the same thing for our PCAP trace file that we
did with the ``AsciiTraceHelper``. The line of code,

::

  Ptr<PcapFileWrapper> file = pcapHelper.CreateFile ("sixth.pcap",
  "w", PcapHelper::DLT_PPP);

creates a PCAP file named "sixth.pcap" with file mode "w".  This means
that the new file is truncated (contents deleted) if an existing file
with that name is found.  The final parameter is the "data link type"
of the new PCAP file.  These are the same as the PCAP library data
link types defined in ``bpf.h`` if you are familar with PCAP.  In this
case, ``DLT_PPP`` indicates that the PCAP file is going to contain
packets prefixed with point to point headers.  This is true since the
packets are coming from our point-to-point device driver.  Other
common data link types are DLT_EN10MB (10 MB Ethernet) appropriate for
csma devices and DLT_IEEE802_11 (IEEE 802.11) appropriate for wifi
devices.  These are defined in ``src/network/helper/trace-helper.h``
if you are interested in seeing the list.  The entries in the list
match those in ``bpf.h`` but we duplicate them to avoid a PCAP source
dependence.

A |ns3| object representing the PCAP file is returned from
``CreateFile`` and used in a bound callback exactly as it was in the
ASCII case.

An important detour: It is important to notice that even though both
of these objects are declared in very similar ways,

::

  Ptr<PcapFileWrapper> file ...
  Ptr<OutputStreamWrapper> stream ...

The underlying objects are entirely different.  For example, the
``Ptr<PcapFileWrapper>`` is a smart pointer to an |ns3| Object that is
a fairly heavyweight thing that supports Attributes and is integrated
into the Config system.  The ``Ptr<OutputStreamWrapper>``, on the
other hand, is a smart pointer to a reference counted object that is a
very lightweight thing.  Remember to look at the object you are
referencing before making any assumptions about the "powers" that
object may have.

For example, take a look at ``src/network/utils/pcap-file-wrapper.h``
in the distribution and notice,

::

  class PcapFileWrapper : public Object

that class ``PcapFileWrapper`` is an |ns3| Object by virtue of its
inheritance.  Then look at
``src/network/model/output-stream-wrapper.h`` and notice,

::

  class OutputStreamWrapper : public
  SimpleRefCount<OutputStreamWrapper>

that this object is not an |ns3| Object at all, it is "merely" a C++
object that happens to support intrusive reference counting.

The point here is that just because you read ``Ptr<something>`` it does
not necessarily mean that ``something`` is an |ns3| Object on which you
can hang |ns3| Attributes, for example.

Now, back to the example.  If you build and run this example,

.. sourcecode:: bash

  $ ./waf --run sixth

you will see the same messages appear as when you ran "fifth", but two
new files will appear in the top-level directory of your |ns3|
distribution.

.. sourcecode:: bash

  sixth.cwnd  sixth.pcap

Since "sixth.cwnd" is an ASCII text file, you can view it with ``cat``
or your favorite file viewer.

.. sourcecode:: bash

  1       0       536
  1.0093  536     1072
  1.01528 1072    1608
  1.02167 1608    2144
  ...
  9.69256 5149	  5204
  9.89311 5204	  5259

You have a tab separated file with a timestamp, an old congestion
window and a new congestion window suitable for directly importing
into your plot program.  There are no extraneous prints in the file,
no parsing or editing is required.

Since "sixth.pcap" is a PCAP file, you can fiew it with ``tcpdump``.

.. sourcecode:: bash

  reading from file sixth.pcap, link-type PPP (PPP)
  1.136956 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 17177:17681, ack 1, win 32768, options [TS val 1133 ecr 1127,eol], length 504
  1.403196 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 33280:33784, ack 1, win 32768, options [TS val 1399 ecr 1394,eol], length 504
  ...
  7.426220 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 785704:786240, ack 1, win 32768, options [TS val 7423 ecr 7421,eol], length 536
  9.630693 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 882688:883224, ack 1, win 32768, options [TS val 9620 ecr 9618,eol], length 536

You have a PCAP file with the packets that were dropped in the
simulation.  There are no other packets present in the file and there
is nothing else present to make life difficult.

It's been a long journey, but we are now at a point where we can
appreciate the |ns3| tracing system.  We have pulled important events
out of the middle of a TCP implementation and a device driver.  We
stored those events directly in files usable with commonly known
tools.  We did this without modifying any of the core code involved,
and we did this in only 18 lines of code::

  static void
  CwndChange (Ptr<OutputStreamWrapper> stream, uint32_t oldCwnd, uint32_t newCwnd)
  {
    NS_LOG_UNCOND (Simulator::Now ().GetSeconds () << "\t" << newCwnd);
    *stream->GetStream () << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;
  }

  ...

  AsciiTraceHelper asciiTraceHelper;
  Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("sixth.cwnd");
  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", MakeBoundCallback (&CwndChange, stream));

  ...

  static void
  RxDrop (Ptr<PcapFileWrapper> file, Ptr<const Packet> p)
  {
    NS_LOG_UNCOND ("RxDrop at " << Simulator::Now ().GetSeconds ());
    file->Write(Simulator::Now(), p);
  }

  ...
  
  PcapHelper pcapHelper;
  Ptr<PcapFileWrapper> file = pcapHelper.CreateFile ("sixth.pcap", "w", PcapHelper::DLT_PPP);
  devices.Get (1)->TraceConnectWithoutContext("PhyRxDrop", MakeBoundCallback (&RxDrop, file));

Trace Helpers
*************

The |ns3| trace helpers provide a rich environment for configuring and
selecting different trace events and writing them to files.  In
previous sections, primarily :ref:`BuildingTopologies`, we have seen
several varieties of the trace helper methods designed for use inside
other (device) helpers.

Perhaps you will recall seeing some of these variations: 

::

  pointToPoint.EnablePcapAll ("second");
  pointToPoint.EnablePcap ("second", p2pNodes.Get (0)->GetId (), 0);
  csma.EnablePcap ("third", csmaDevices.Get (0), true);
  pointToPoint.EnableAsciiAll (ascii.CreateFileStream ("myfirst.tr"));

What may not be obvious, though, is that there is a consistent model
for all of the trace-related methods found in the system.  We will now
take a little time and take a look at the "big picture".

There are currently two primary use cases of the tracing helpers in
|ns3|:  device helpers and protocol helpers.  Device helpers look at
the problem of specifying which traces should be enabled through a
(node, device) pair.  For example, you may want to specify that PCAP
tracing should be enabled on a particular device on a specific node.
This follows from the |ns3| device conceptual model, and also the
conceptual models of the various device helpers.  Following naturally
from this, the files created follow a ``<prefix>-<node>-<device>`` naming
convention.

Protocol helpers look at the problem of specifying which traces should
be enabled through a protocol and interface pair.  This follows from
the |ns3| protocol stack conceptual model, and also the conceptual
models of internet stack helpers.  Naturally, the trace files should
follow a ``<prefix>-<protocol>-<interface>`` naming convention.

The trace helpers therefore fall naturally into a two-dimensional
taxonomy.  There are subtleties that prevent all four classes from
behaving identically, but we do strive to make them all work as
similarly as possible; and whenever possible there are analogs for all
methods in all classes.

  +-----------------+---------+---------+
  |                 |  PCAP   |  ASCII  |
  +=================+=========+=========+
  | Device Helper   | |check| | |check| |
  +-----------------+---------+---------+
  | Protocol Helper | |check| | |check| |
  +-----------------+---------+---------+

We use an approach called a ``mixin`` to add tracing functionality to
our helper classes.  A ``mixin`` is a class that provides
functionality when it is inherited by a subclass.  Inheriting from a
mixin is not considered a form of specialization but is really a way
to collect functionality.

Let's take a quick look at all four of these cases and their
respective ``mixins``.

Device Helpers
++++++++++++++

PCAP
~~~~

The goal of these helpers is to make it easy to add a consistent PCAP
trace facility to an |ns3| device.  We want all of the various flavors
of PCAP tracing to work the same across all devices, so the methods of
these helpers are inherited by device helpers.  Take a look at
``src/network/helper/trace-helper.h`` if you want to follow the
discussion while looking at real code.

The class ``PcapHelperForDevice`` is a ``mixin`` provides the high
level functionality for using PCAP tracing in an |ns3| device.  Every
device must implement a single virtual method inherited from this
class.

::

  virtual void EnablePcapInternal (std::string prefix, Ptr<NetDevice> nd, bool promiscuous, bool explicitFilename) = 0;

The signature of this method reflects the device-centric view of the
situation at this level.  All of the public methods inherited from
class ``PcapUserHelperForDevice`` reduce to calling this single
device-dependent implementation method.  For example, the lowest level
PCAP method,

::

  void EnablePcap (std::string prefix, Ptr<NetDevice> nd, bool promiscuous = false, bool explicitFilename = false);

will call the device implementation of ``EnablePcapInternal``
directly.  All other public PCAP tracing methods build on this
implementation to provide additional user-level functionality.  What
this means to the user is that all device helpers in the system will
have all of the PCAP trace methods available; and these methods will
all work in the same way across devices if the device implements
``EnablePcapInternal`` correctly.

Methods
#######

::

  void EnablePcap (std::string prefix, Ptr<NetDevice> nd, bool promiscuous = false, bool explicitFilename = false);
  void EnablePcap (std::string prefix, std::string ndName, bool promiscuous = false, bool explicitFilename = false);
  void EnablePcap (std::string prefix, NetDeviceContainer d, bool promiscuous = false);
  void EnablePcap (std::string prefix, NodeContainer n, bool promiscuous = false);
  void EnablePcap (std::string prefix, uint32_t nodeid, uint32_t deviceid, bool promiscuous = false);
  void EnablePcapAll (std::string prefix, bool promiscuous = false);

In each of the methods shown above, there is a default parameter
called ``promiscuous`` that defaults to ``false``.  This parameter
indicates that the trace should not be gathered in promiscuous mode.
If you do want your traces to include all traffic seen by the device
(and if the device supports a promiscuous mode) simply add a true
parameter to any of the calls above.  For example,

::

  Ptr<NetDevice> nd;
  ...
  helper.EnablePcap ("prefix", nd, true);

will enable promiscuous mode captures on the ``NetDevice`` specified
by ``nd``.

The first two methods also include a default parameter called
``explicitFilename`` that will be discussed below.

You are encouraged to peruse the API Documentation for class
``PcapHelperForDevice`` to find the details of these methods; but to
summarize ...

* You can enable PCAP tracing on a particular node/net-device pair by
  providing a ``Ptr<NetDevice>`` to an ``EnablePcap`` method.  The
  ``Ptr<Node>`` is implicit since the net device must belong to exactly
  one Node.  For example,

  ::

    Ptr<NetDevice> nd;
    ...
    helper.EnablePcap ("prefix", nd);

* You can enable PCAP tracing on a particular node/net-device pair by
  providing a ``std::string`` representing an object name service string
  to an ``EnablePcap`` method.  The ``Ptr<NetDevice>`` is looked up from
  the name string.  Again, the ``<Node>`` is implicit since the named
  net device must belong to exactly one Node.  For example,

  ::

    Names::Add ("server" ...);
    Names::Add ("server/eth0" ...);
    ...
    helper.EnablePcap ("prefix", "server/ath0");

* You can enable PCAP tracing on a collection of node/net-device pairs
  by providing a ``NetDeviceContainer``.  For each ``NetDevice`` in the
  container the type is checked.  For each device of the proper type
  (the same type as is managed by the device helper), tracing is
  enabled.  Again, the ``<Node>`` is implicit since the found net device
  must belong to exactly one Node.  For example,

  ::

    NetDeviceContainer d = ...;
    ...
    helper.EnablePcap ("prefix", d);

* You can enable PCAP tracing on a collection of node/net-device pairs
  by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` its attached ``NetDevices`` are iterated.  For each
  ``NetDevice`` attached to each Node in the container, the type of that
  device is checked.  For each device of the proper type (the same type
  as is managed by the device helper), tracing is enabled.

  ::

    NodeContainer n;
    ...
    helper.EnablePcap ("prefix", n);

* You can enable PCAP tracing on the basis of Node ID and device ID as
  well as with explicit ``Ptr``.  Each Node in the system has an
  integer Node ID and each device connected to a Node has an integer
  device ID.

  ::

    helper.EnablePcap ("prefix", 21, 1);

* Finally, you can enable PCAP tracing for all devices in the system,
  with the same type as that managed by the device helper.

  ::

    helper.EnablePcapAll ("prefix");

Filenames
#########

Implicit in the method descriptions above is the construction of a
complete filename by the implementation method.  By convention, PCAP
traces in the |ns3| system are of the form ``<prefix>-<node id>-<device
id>.pcap``

As previously mentioned, every Node in the system will have a
system-assigned Node id; and every device will have an interface index
(also called a device id) relative to its node.  By default, then, a
PCAP trace file created as a result of enabling tracing on the first
device of Node 21 using the prefix "prefix" would be
``prefix-21-1.pcap``.

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "server" to Node 21, the resulting PCAP trace file name will
automatically become, ``prefix-server-1.pcap`` and if you also assign
the name "eth0" to the device, your PCAP file name will automatically
pick this up and be called ``prefix-server-eth0.pcap``.

Finally, two of the methods shown above,

::

  void EnablePcap (std::string prefix, Ptr<NetDevice> nd, bool promiscuous = false, bool explicitFilename = false);
  void EnablePcap (std::string prefix, std::string ndName, bool promiscuous = false, bool explicitFilename = false);

have a default parameter called ``explicitFilename``.  When set to
true, this parameter disables the automatic filename completion
mechanism and allows you to create an explicit filename.  This option
is only available in the methods which enable PCAP tracing on a single
device.

For example, in order to arrange for a device helper to create a
single promiscuous PCAP capture file of a specific name
``my-pcap-file.pcap`` on a given device, one could::

  Ptr<NetDevice> nd;
  ...
  helper.EnablePcap ("my-pcap-file.pcap", nd, true, true);

The first ``true`` parameter enables promiscuous mode traces and the
second tells the helper to interpret the ``prefix`` parameter as a
complete filename.

ASCII
~~~~~

The behavior of the ASCII trace helper ``mixin`` is substantially
similar to the PCAP version.  Take a look at
``src/network/helper/trace-helper.h`` if you want to follow the
discussion while looking at real code.

The class ``AsciiTraceHelperForDevice`` adds the high level
functionality for using ASCII tracing to a device helper class.  As in
the PCAP case, every device must implement a single virtual method
inherited from the ASCII trace ``mixin``.

::

  virtual void EnableAsciiInternal (Ptr<OutputStreamWrapper> stream, 
                                    std::string prefix, 
                                    Ptr<NetDevice> nd,
                                    bool explicitFilename) = 0;


The signature of this method reflects the device-centric view of the
situation at this level; and also the fact that the helper may be
writing to a shared output stream.  All of the public
ASCII-trace-related methods inherited from class
``AsciiTraceHelperForDevice`` reduce to calling this single device-
dependent implementation method.  For example, the lowest level ascii
trace methods,

::

  void EnableAscii (std::string prefix, Ptr<NetDevice> nd, bool explicitFilename = false);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, Ptr<NetDevice> nd);


will call the device implementation of ``EnableAsciiInternal``
directly, providing either a valid prefix or stream.  All other public
ASCII tracing methods will build on these low-level functions to
provide additional user-level functionality.  What this means to the
user is that all device helpers in the system will have all of the
ASCII trace methods available; and these methods will all work in the
same way across devices if the devices implement
``EnablAsciiInternal`` correctly.

Methods
#######

::

  void EnableAscii (std::string prefix, Ptr<NetDevice> nd, bool explicitFilename = false);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, Ptr<NetDevice> nd);

  void EnableAscii (std::string prefix, std::string ndName, bool explicitFilename = false);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, std::string ndName);

  void EnableAscii (std::string prefix, NetDeviceContainer d);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, NetDeviceContainer d);

  void EnableAscii (std::string prefix, NodeContainer n);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, NodeContainer n);

  void EnableAsciiAll (std::string prefix);
  void EnableAsciiAll (Ptr<OutputStreamWrapper> stream);

  void EnableAscii (std::string prefix, uint32_t nodeid, uint32_t deviceid, bool explicitFilename);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, uint32_t nodeid, uint32_t deviceid);

You are encouraged to peruse the API Documentation for class
``AsciiTraceHelperForDevice`` to find the details of these methods;
but to summarize ...

* There are twice as many methods available for ASCII tracing as
  there were for PCAP tracing.  This is because, in addition to the
  PCAP-style model where traces from each unique node/device pair are
  written to a unique file, we support a model in which trace
  information for many node/device pairs is written to a common file.
  This means that the <prefix>-<node>-<device> file name generation
  mechanism is replaced by a mechanism to refer to a common file; and
  the number of API methods is doubled to allow all combinations.

* Just as in PCAP tracing, you can enable ASCII tracing on a
  particular (node, net-device) pair by providing a ``Ptr<NetDevice>``
  to an ``EnableAscii`` method.  The ``Ptr<Node>`` is implicit since
  the net device must belong to exactly one Node.  For example,

::

  Ptr<NetDevice> nd;
  ...
  helper.EnableAscii ("prefix", nd);

* The first four methods also include a default parameter called
  ``explicitFilename`` that operate similar to equivalent parameters
  in the PCAP case.

  In this case, no trace contexts are written to the ASCII trace file
  since they would be redundant.  The system will pick the file name
  to be created using the same rules as described in the PCAP section,
  except that the file will have the suffix ``.tr`` instead of
  ``.pcap``.

* If you want to enable ASCII tracing on more than one net device
  and have all traces sent to a single file, you can do that as well
  by using an object to refer to a single file.  We have already seen
  this in the "cwnd" example above::

    Ptr<NetDevice> nd1;
    Ptr<NetDevice> nd2;
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAscii (stream, nd1);
    helper.EnableAscii (stream, nd2);


  In this case, trace contexts *are* written to the ASCII trace file
  since they are required to disambiguate traces from the two devices.
  Note that since the user is completely specifying the file name, the
  string should include the ``,tr`` suffix for consistency.

* You can enable ASCII tracing on a particular (node, net-device)
  pair by providing a ``std::string`` representing an object name
  service string to an ``EnablePcap`` method.  The ``Ptr<NetDevice>``
  is looked up from the name string.  Again, the ``<Node>`` is
  implicit since the named net device must belong to exactly one Node.
  For example,

  ::

    Names::Add ("client" ...);
    Names::Add ("client/eth0" ...);
    Names::Add ("server" ...);
    Names::Add ("server/eth0" ...);
    ...
    helper.EnableAscii ("prefix", "client/eth0");
    helper.EnableAscii ("prefix", "server/eth0");

    This would result in two files named ``prefix-client-eth0.tr`` and
    ``prefix-server-eth0.tr`` with traces for each device in the
    respective trace file.  Since all of the ``EnableAscii`` functions
    are overloaded to take a stream wrapper, you can use that form as
    well::

    Names::Add ("client" ...);
    Names::Add ("client/eth0" ...);
    Names::Add ("server" ...);
    Names::Add ("server/eth0" ...);
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAscii (stream, "client/eth0");
    helper.EnableAscii (stream, "server/eth0");

  This would result in a single trace file called
  ``trace-file-name.tr`` that contains all of the trace events for
  both devices.  The events would be disambiguated by trace context
  strings.

* You can enable ASCII tracing on a collection of (node, net-device)
  pairs by providing a ``NetDeviceContainer``.  For each ``NetDevice``
  in the container the type is checked.  For each device of the proper
  type (the same type as is managed by the device helper), tracing is
  enabled.  Again, the ``<Node>`` is implicit since the found net
  device must belong to exactly one Node.  For example,

  ::

    NetDeviceContainer d = ...;
    ...
    helper.EnableAscii ("prefix", d);

    This would result in a number of ASCII trace files being created,
    each of which follows the ``<prefix>-<node id>-<device id>.tr``
    convention.

  Combining all of the traces into a single file is accomplished
  similarly to the examples above::

    NetDeviceContainer d = ...;
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAscii (stream, d);

* You can enable ASCII tracing on a collection of (node, net-device)
  pairs by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` its attached ``NetDevices`` are iterated.  For
  each ``NetDevice`` attached to each Node in the container, the type
  of that device is checked.  For each device of the proper type (the
  same type as is managed by the device helper), tracing is enabled.

  ::

    NodeContainer n;
    ...
    helper.EnableAscii ("prefix", n);

  This would result in a number of ASCII trace files being created,
  each of which follows the ``<prefix>-<node id>-<device id>.tr``
  convention.  Combining all of the traces into a single file is
  accomplished similarly to the examples above.

* You can enable PCAP tracing on the basis of Node ID and device ID
  as well as with explicit ``Ptr``.  Each Node in the system has an
  integer Node ID and each device connected to a Node has an integer
  device ID.

  ::

    helper.EnableAscii ("prefix", 21, 1);

  Of course, the traces can be combined into a single file as shown
  above.

* Finally, you can enable PCAP tracing for all devices in the
  system, with the same type as that managed by the device helper.

  ::

    helper.EnableAsciiAll ("prefix");

  This would result in a number of ASCII trace files being created,
  one for every device in the system of the type managed by the
  helper.  All of these files will follow the ``<prefix>-<node
  id>-<device id>.tr`` convention.  Combining all of the traces into a
  single file is accomplished similarly to the examples above.

Filenames
#########

Implicit in the prefix-style method descriptions above is the
construction of the complete filenames by the implementation method.
By convention, ASCII traces in the |ns3| system are of the form
``<prefix>-<node id>-<device id>.tr``

As previously mentioned, every Node in the system will have a
system-assigned Node id; and every device will have an interface index
(also called a device id) relative to its node.  By default, then, an
ASCII trace file created as a result of enabling tracing on the first
device of Node 21, using the prefix "prefix", would be
``prefix-21-1.tr``.

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "server" to Node 21, the resulting ASCII trace file name will
automatically become, ``prefix-server-1.tr`` and if you also assign the
name "eth0" to the device, your ASCII trace file name will
automatically pick this up and be called ``prefix-server-eth0.tr``.

Several of the methods have a default parameter called
``explicitFilename``.  When set to true, this parameter disables the
automatic filename completion mechanism and allows you to create an
explicit filename.  This option is only available in the methods which
take a prefix and enable tracing on a single device.

Protocol Helpers
++++++++++++++++

PCAP
~~~~

The goal of these ``mixins`` is to make it easy to add a consistent
PCAP trace facility to protocols.  We want all of the various flavors
of PCAP tracing to work the same across all protocols, so the methods
of these helpers are inherited by stack helpers.  Take a look at
``src/network/helper/trace-helper.h`` if you want to follow the
discussion while looking at real code.

In this section we will be illustrating the methods as applied to the
protocol ``Ipv4``.  To specify traces in similar protocols, just
substitute the appropriate type.  For example, use a ``Ptr<Ipv6>``
instead of a ``Ptr<Ipv4>`` and call ``EnablePcapIpv6`` instead of
``EnablePcapIpv4``.

The class ``PcapHelperForIpv4`` provides the high level functionality
for using PCAP tracing in the ``Ipv4`` protocol.  Each protocol helper
enabling these methods must implement a single virtual method
inherited from this class.  There will be a separate implementation
for ``Ipv6``, for example, but the only difference will be in the
method names and signatures.  Different method names are required to
disambiguate class ``Ipv4`` from ``Ipv6`` which are both derived from
class ``Object``, and methods that share the same signature.

::

  virtual void EnablePcapIpv4Internal (std::string prefix, 
                                       Ptr<Ipv4> ipv4, 
                                       uint32_t interface,
                                       bool explicitFilename) = 0;

The signature of this method reflects the protocol and
interface-centric view of the situation at this level.  All of the
public methods inherited from class ``PcapHelperForIpv4`` reduce to
calling this single device-dependent implementation method.  For
example, the lowest level PCAP method,

::

  void EnablePcapIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);


will call the device implementation of ``EnablePcapIpv4Internal``
directly.  All other public PCAP tracing methods build on this
implementation to provide additional user-level functionality.  What
this means to the user is that all protocol helpers in the system will
have all of the PCAP trace methods available; and these methods will
all work in the same way across protocols if the helper implements
``EnablePcapIpv4Internal`` correctly.

Methods
#######

These methods are designed to be in one-to-one correspondence with the
Node- and ``NetDevice``- centric versions of the device versions.
Instead of Node and ``NetDevice`` pair constraints, we use
protocol and interface constraints.

Note that just like in the device version, there are six methods::

  void EnablePcapIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);
  void EnablePcapIpv4 (std::string prefix, std::string ipv4Name, uint32_t interface, bool explicitFilename = false);
  void EnablePcapIpv4 (std::string prefix, Ipv4InterfaceContainer c);
  void EnablePcapIpv4 (std::string prefix, NodeContainer n);
  void EnablePcapIpv4 (std::string prefix, uint32_t nodeid, uint32_t interface, bool explicitFilename);
  void EnablePcapIpv4All (std::string prefix);

You are encouraged to peruse the API Documentation for class
``PcapHelperForIpv4`` to find the details of these methods; but to
summarize ...

* You can enable PCAP tracing on a particular protocol/interface pair by
  providing a ``Ptr<Ipv4>`` and ``interface`` to an ``EnablePcap``
  method.  For example,

  ::

    Ptr<Ipv4> ipv4 = node->GetObject<Ipv4> ();
    ...
    helper.EnablePcapIpv4 ("prefix", ipv4, 0);

* You can enable PCAP tracing on a particular node/net-device pair by
  providing a ``std::string`` representing an object name service string
  to an ``EnablePcap`` method.  The ``Ptr<Ipv4>`` is looked up from the
  name string.  For example,

  ::

    Names::Add ("serverIPv4" ...);
    ...
    helper.EnablePcapIpv4 ("prefix", "serverIpv4", 1);

* You can enable PCAP tracing on a collection of protocol/interface
  pairs by providing an ``Ipv4InterfaceContainer``.  For each ``Ipv4`` /
  interface pair in the container the protocol type is checked.  For
  each protocol of the proper type (the same type as is managed by the
  device helper), tracing is enabled for the corresponding interface.
  For example,

  ::

    NodeContainer nodes;
    ...
    NetDeviceContainer devices = deviceHelper.Install (nodes);
    ... 
    Ipv4AddressHelper ipv4;
    ipv4.SetBase ("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer interfaces = ipv4.Assign (devices);
    ...
    helper.EnablePcapIpv4 ("prefix", interfaces);

* You can enable PCAP tracing on a collection of protocol/interface
  pairs by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` the appropriate protocol is found.  For each
  protocol, its interfaces are enumerated and tracing is enabled on the
  resulting pairs.  For example,

  ::

    NodeContainer n;
    ...
    helper.EnablePcapIpv4 ("prefix", n);

* You can enable PCAP tracing on the basis of Node ID and interface as
  well.  In this case, the node-id is translated to a ``Ptr<Node>`` and
  the appropriate protocol is looked up in the node.  The resulting
  protocol and interface are used to specify the resulting trace source.

  ::

    helper.EnablePcapIpv4 ("prefix", 21, 1);

* Finally, you can enable PCAP tracing for all interfaces in the
  system, with associated protocol being the same type as that managed
  by the device helper.

  ::

    helper.EnablePcapIpv4All ("prefix");

Filenames
#########

Implicit in all of the method descriptions above is the construction
of the complete filenames by the implementation method.  By
convention, PCAP traces taken for devices in the |ns3| system are of
the form "<prefix>-<node id>-<device id>.pcap".  In the case of
protocol traces, there is a one-to-one correspondence between
protocols and ``Nodes``.  This is because protocol ``Objects`` are
aggregated to ``Node Objects``.  Since there is no global protocol id
in the system, we use the corresponding Node id in file naming.
Therefore there is a possibility for file name collisions in
automatically chosen trace file names.  For this reason, the file name
convention is changed for protocol traces.

As previously mentioned, every Node in the system will have a
system-assigned Node id.  Since there is a one-to-one correspondence
between protocol instances and Node instances we use the Node id.
Each interface has an interface id relative to its protocol.  We use
the convention "<prefix>-n<node id>-i<interface id>.pcap" for trace
file naming in protocol helpers.

Therefore, by default, a PCAP trace file created as a result of
enabling tracing on interface 1 of the Ipv4 protocol of Node 21 using
the prefix "prefix" would be "prefix-n21-i1.pcap".

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "serverIpv4" to the Ptr<Ipv4> on Node 21, the resulting PCAP
trace file name will automatically become,
"prefix-nserverIpv4-i1.pcap".

Several of the methods have a default parameter called
``explicitFilename``.  When set to true, this parameter disables the
automatic filename completion mechanism and allows you to create an
explicit filename.  This option is only available in the methods which
take a prefix and enable tracing on a single device.

ASCII
~~~~~

The behavior of the ASCII trace helpers is substantially similar to
the PCAP case.  Take a look at ``src/network/helper/trace-helper.h``
if you want to follow the discussion while looking at real code.

In this section we will be illustrating the methods as applied to the
protocol ``Ipv4``.  To specify traces in similar protocols, just
substitute the appropriate type.  For example, use a ``Ptr<Ipv6>``
instead of a ``Ptr<Ipv4>`` and call ``EnableAsciiIpv6`` instead of
``EnableAsciiIpv4``.

The class ``AsciiTraceHelperForIpv4`` adds the high level
functionality for using ASCII tracing to a protocol helper.  Each
protocol that enables these methods must implement a single virtual
method inherited from this class.

::

  virtual void EnableAsciiIpv4Internal (Ptr<OutputStreamWrapper> stream, 
                                        std::string prefix, 
                                        Ptr<Ipv4> ipv4, 
                                        uint32_t interface,
                                        bool explicitFilename) = 0;

The signature of this method reflects the protocol- and
interface-centric view of the situation at this level; and also the
fact that the helper may be writing to a shared output stream.  All of
the public methods inherited from class
``PcapAndAsciiTraceHelperForIpv4`` reduce to calling this single
device- dependent implementation method.  For example, the lowest
level ASCII trace methods,

::

  void EnableAsciiIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, Ptr<Ipv4> ipv4, uint32_t interface);


will call the device implementation of ``EnableAsciiIpv4Internal``
directly, providing either the prefix or the stream.  All other public
ASCII tracing methods will build on these low-level functions to
provide additional user-level functionality.  What this means to the
user is that all device helpers in the system will have all of the
ASCII trace methods available; and these methods will all work in the
same way across protocols if the protocols implement
``EnablAsciiIpv4Internal`` correctly.

Methods
#######

::

  void EnableAsciiIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, Ptr<Ipv4> ipv4, uint32_t interface);

  void EnableAsciiIpv4 (std::string prefix, std::string ipv4Name, uint32_t interface, bool explicitFilename = false);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, std::string ipv4Name, uint32_t interface);

  void EnableAsciiIpv4 (std::string prefix, Ipv4InterfaceContainer c);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, Ipv4InterfaceContainer c);

  void EnableAsciiIpv4 (std::string prefix, NodeContainer n);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, NodeContainer n);

  void EnableAsciiIpv4All (std::string prefix);
  void EnableAsciiIpv4All (Ptr<OutputStreamWrapper> stream);

  void EnableAsciiIpv4 (std::string prefix, uint32_t nodeid, uint32_t deviceid, bool explicitFilename);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, uint32_t nodeid, uint32_t interface);

You are encouraged to peruse the API Documentation for class
``PcapAndAsciiHelperForIpv4`` to find the details of these methods;
but to summarize ...

* There are twice as many methods available for ASCII tracing as there
  were for PCAP tracing.  This is because, in addition to the PCAP-style
  model where traces from each unique protocol/interface pair are
  written to a unique file, we support a model in which trace
  information for many protocol/interface pairs is written to a common
  file.  This means that the <prefix>-n<node id>-<interface> file name
  generation mechanism is replaced by a mechanism to refer to a common
  file; and the number of API methods is doubled to allow all
  combinations.

* Just as in PCAP tracing, you can enable ASCII tracing on a particular
  protocol/interface pair by providing a ``Ptr<Ipv4>`` and an
  ``interface`` to an ``EnableAscii`` method.  For example,

  ::

    Ptr<Ipv4> ipv4;
    ...
    helper.EnableAsciiIpv4 ("prefix", ipv4, 1);

  In this case, no trace contexts are written to the ASCII trace file
  since they would be redundant.  The system will pick the file name to
  be created using the same rules as described in the PCAP section,
  except that the file will have the suffix ".tr" instead of ".pcap".

* If you want to enable ASCII tracing on more than one interface and
  have all traces sent to a single file, you can do that as well by
  using an object to refer to a single file.  We have already something
  similar to this in the "cwnd" example above::

    Ptr<Ipv4> protocol1 = node1->GetObject<Ipv4> ();
    Ptr<Ipv4> protocol2 = node2->GetObject<Ipv4> ();
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAsciiIpv4 (stream, protocol1, 1);
    helper.EnableAsciiIpv4 (stream, protocol2, 1);

  In this case, trace contexts are written to the ASCII trace file since
  they are required to disambiguate traces from the two interfaces.
  Note that since the user is completely specifying the file name, the
  string should include the ",tr" for consistency.

* You can enable ASCII tracing on a particular protocol by providing a
  ``std::string`` representing an object name service string to an
  ``EnablePcap`` method.  The ``Ptr<Ipv4>`` is looked up from the name
  string.  The ``<Node>`` in the resulting filenames is implicit since
  there is a one-to-one correspondence between protocol instances and
  nodes, For example,

  ::

    Names::Add ("node1Ipv4" ...);
    Names::Add ("node2Ipv4" ...);
    ...
    helper.EnableAsciiIpv4 ("prefix", "node1Ipv4", 1);
    helper.EnableAsciiIpv4 ("prefix", "node2Ipv4", 1);

  This would result in two files named "prefix-nnode1Ipv4-i1.tr" and
  "prefix-nnode2Ipv4-i1.tr" with traces for each interface in the
  respective trace file.  Since all of the EnableAscii functions are
  overloaded to take a stream wrapper, you can use that form as well::

    Names::Add ("node1Ipv4" ...);
    Names::Add ("node2Ipv4" ...);
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAsciiIpv4 (stream, "node1Ipv4", 1);
    helper.EnableAsciiIpv4 (stream, "node2Ipv4", 1);

  This would result in a single trace file called "trace-file-name.tr"
  that contains all of the trace events for both interfaces.  The events
  would be disambiguated by trace context strings.

* You can enable ASCII tracing on a collection of protocol/interface
  pairs by providing an ``Ipv4InterfaceContainer``.  For each protocol
  of the proper type (the same type as is managed by the device helper),
  tracing is enabled for the corresponding interface.  Again, the
  ``<Node>`` is implicit since there is a one-to-one correspondence
  between each protocol and its node.  For example,

  ::

    NodeContainer nodes;
    ...
    NetDeviceContainer devices = deviceHelper.Install (nodes);
    ... 
    Ipv4AddressHelper ipv4;
    ipv4.SetBase ("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer interfaces = ipv4.Assign (devices);
    ...
    ...
    helper.EnableAsciiIpv4 ("prefix", interfaces);

  This would result in a number of ASCII trace files being created, each
  of which follows the <prefix>-n<node id>-i<interface>.tr convention.
  Combining all of the traces into a single file is accomplished
  similarly to the examples above::

    NodeContainer nodes;
    ...
    NetDeviceContainer devices = deviceHelper.Install (nodes);
    ... 
    Ipv4AddressHelper ipv4;
    ipv4.SetBase ("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer interfaces = ipv4.Assign (devices);
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAsciiIpv4 (stream, interfaces);

* You can enable ASCII tracing on a collection of protocol/interface
  pairs by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` the appropriate protocol is found.  For each
  protocol, its interfaces are enumerated and tracing is enabled on the
  resulting pairs.  For example,

  ::

    NodeContainer n;
    ...
    helper.EnableAsciiIpv4 ("prefix", n);

  This would result in a number of ASCII trace files being created,
  each of which follows the <prefix>-<node id>-<device id>.tr
  convention.  Combining all of the traces into a single file is
  accomplished similarly to the examples above.

* You can enable PCAP tracing on the basis of Node ID and device ID as
  well.  In this case, the node-id is translated to a ``Ptr<Node>`` and
  the appropriate protocol is looked up in the node.  The resulting
  protocol and interface are used to specify the resulting trace source.

  ::

    helper.EnableAsciiIpv4 ("prefix", 21, 1);

  Of course, the traces can be combined into a single file as shown
  above.

* Finally, you can enable ASCII tracing for all interfaces in the
  system, with associated protocol being the same type as that managed
  by the device helper.

  ::

    helper.EnableAsciiIpv4All ("prefix");

  This would result in a number of ASCII trace files being created, one
  for every interface in the system related to a protocol of the type
  managed by the helper.  All of these files will follow the
  <prefix>-n<node id>-i<interface.tr convention.  Combining all of the
  traces into a single file is accomplished similarly to the examples
  above.

Filenames
#########

Implicit in the prefix-style method descriptions above is the
construction of the complete filenames by the implementation method.
By convention, ASCII traces in the |ns3| system are of the form
"<prefix>-<node id>-<device id>.tr"

As previously mentioned, every Node in the system will have a
system-assigned Node id.  Since there is a one-to-one correspondence
between protocols and nodes we use to node-id to identify the protocol
identity.  Every interface on a given protocol will have an interface
index (also called simply an interface) relative to its protocol.  By
default, then, an ASCII trace file created as a result of enabling
tracing on the first device of Node 21, using the prefix "prefix",
would be "prefix-n21-i1.tr".  Use the prefix to disambiguate multiple
protocols per node.

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "serverIpv4" to the protocol on Node 21, and also specify
interface one, the resulting ASCII trace file name will automatically
become, "prefix-nserverIpv4-1.tr".

Several of the methods have a default parameter called
``explicitFilename``.  When set to true, this parameter disables the
automatic filename completion mechanism and allows you to create an
explicit filename.  This option is only available in the methods which
take a prefix and enable tracing on a single device.

Summary
*******

|ns3| includes an extremely rich environment allowing users at several
levels to customize the kinds of information that can be extracted
from simulations.

There are high-level helper functions that allow users to simply
control the collection of pre-defined outputs to a fine granularity.
There are mid-level helper functions to allow more sophisticated users
to customize how information is extracted and saved; and there are
low-level core functions to allow expert users to alter the system to
present new and previously unexported information in a way that will
be immediately accessible to users at higher levels.

This is a very comprehensive system, and we realize that it is a lot
to digest, especially for new users or those not intimately familiar
with C++ and its idioms.  We do consider the tracing system a very
important part of |ns3| and so recommend becoming as familiar as
possible with it.  It is probably the case that understanding the rest
of the |ns3| system will be quite simple once you have mastered the
tracing system